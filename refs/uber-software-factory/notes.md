# Notes — Uber "Software Factory" post

My own analysis. Not part of the source article; see `article.md` for that.

## Thesis

AI cost is an engineering problem, not a procurement problem. Attack wasted
tokens rather than unit price or tool downgrades. Claimed outcome: 7× usage
growth with unit costs falling and quality held.

## The useful idea: decompose spend into six multiplied terms

- First two terms = adoption × engagement → **grow these**.
- Middle three (turns/session, requests/turn, tokens/request) = work the agent
  does on its own behalf, beyond what the engineer asked for → **the target**.
- Last term = price/token, vendor-set → only lever is routing.

Separating "usage you want" from "overhead you eat" is the real contribution.
Most cost dashboards conflate them and end up throttling adoption.

## Levers, ranked by leverage

1. **MCP schema bloat** — 100+ tools ≈ 50–70K tokens of schema in the initial
   prompt, *re-sent every turn*. Fixes: CLI tool resolution (tools projected as
   shell commands, resolved at call time) and tool search (load on demand).
   Biggest win because the cost recurs per turn.
2. **Code-mode** — tools as shell commands let the model batch N actions into
   one script; polling loops run in a subprocess. Measured >50% token reduction
   even on tiny result sets, >90% on bulk. Savings come from removed overhead
   (schema init, multi-turn polling, redundant step-by-step reasoning), not from
   avoiding large payloads.
3. **Cache TTL** — reads at 0.1× input; writes 1.25× (5 min) vs 2× (1 h). Moved
   interactive sessions to 1 h because engineers idle >5 min and were paying
   full-price prefix rebuilds. Subagents stay at 5 min (short-lived).
4. **Subagent default model** — called their most impactful default lever, and
   growing. Subagents do well-specified work that rarely needs frontier
   reasoning; primary model decomposes and evaluates, subagents execute.
5. **Reasoning effort defaulted to Medium** — output/reasoning tokens bill at a
   multiple of input.
6. **Compaction at 400K even on 1M-context models** — balances model quality
   against cache bursts and repeated input cost.
7. **Context-graph grounding** — 24M nodes / 80M edges over 30+ internal
   systems. Agents burn most turns *locating* information, not generating code.
   Their example: grounded agent answered in 38 s; ungrounded spent 20 min, spawned
   2 subagents, hit 3 errors, and concluded wrongly.
8. **Benchmark-driven model selection** — benchmarks built from *real* work, not
   public suites. uReview scores precision/recall/F1 on real PRs with known bugs,
   plus cost/PR, latency, timeouts, noise. Pick the Pareto-optimal point,
   re-evaluate every few weeks.
9. **Visibility** — live cost in the status line, shared spend tiers instead of
   hard caps, Slack nudges at 50/80/100%, and a session dashboard that flags 16
   anti-patterns with per-item cost and remediation.

## Where to be skeptical

- **No absolute figures.** Everything is relative and indexed. No $/engineer/month,
  no baseline.
- **"vs peak" baselines flatter** (−34% vs peak; −52% vs the *June* peak).
- **Cost/session −52%** could be shorter sessions as easily as better ones. No
  per-session quality metric outside uReview's F1.
- **">70% of PRs attributed to agents"** — attribution ≠ authorship; almost
  certainly includes agent-*assisted* PRs.
- **1 h TTL is not universal.** The 2× write premium is wasted on short or
  single-turn sessions. They concede this implicitly by keeping 5 min for subagents.
- **The context graph is not replicable** outside a company of this size.
- Uber is also recruiting and shaping vendor behaviour with this post.

## Relevance to my setup

Already running several of these levers: deferred tools + `ToolSearch` (their
"tool search"), 1 h cache TTL, RTK as a filtering proxy (same thesis as
code-mode/CLI resolution, applied to dev commands).

Gaps worth closing:

- **Subagent default model** — their #1 default lever. The `cavecrew-*` agents
  fit the profile exactly: well-scoped, no frontier reasoning needed.
- **Caveman mode optimises output.** Useful, since output bills at a multiple of
  input — but their measured wins are overwhelmingly input-side. The bigger
  remaining waste is MCP schemas and cache behaviour, not verbosity.
- **Code-mode** — currently-loaded MCP servers (Figma alone is ~30 tools) are
  candidates for CLI projection.
