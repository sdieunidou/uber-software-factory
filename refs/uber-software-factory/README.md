# Running a Software Factory Efficiently at Uber Scale — archive

Local archive of a publicly published Uber Engineering long-form post, kept for
personal reference and note-taking.

| | |
|---|---|
| Source | https://x.com/ubereng/status/2093444169037762840 |
| Publisher | Uber Engineering (@UberEng) |
| Post author | @udaykiran |
| Published | 2026-08-28 21:02 UTC |
| Retrieved | 2026-09-01 |
| Copyright | © Uber Technologies, Inc. All rights reserved. |

Archived under personal-use/fair-dealing assumptions. **Do not redistribute or
republish** this copy; link to the source instead.

## Contents

| Path | What |
|---|---|
| `article.md` | Article text, converted to Markdown, figures inlined as local refs |
| `images/figure-01..15.png` | The 15 figures, highest resolution X still serves |
| `notes.md` | My own analysis and takeaways — not part of the source article |

## Figure index

Figures are numbered in document order and mapped to the section they appear in.

| # | Section | Resolution |
|---|---|---|
| 01 | Introduction | 1516×556 |
| 02 | Introduction | 1478×554 |
| 03 | The Software Factory and Its Cost Equation | 1522×966 |
| 04 | The Software Factory and Its Cost Equation | 1334×412 |
| 05 | How We Measure | 1094×1428 |
| 06 | Optimization Levers | 1572×1290 |
| 07 | Optimizing Price / Token | 1472×744 |
| 08 | Optimizing Tokens / Request | 1524×750 |
| 09 | Optimizing Tokens / Request | 1258×600 |
| 10 | Optimizing Tokens / Request | 1514×724 |
| 11 | Optimizing Tokens / Request | 1560×1186 |
| 12 | Optimizing Tokens / Request | 1532×564 |
| 13 | Optimizing Tokens / Request | 1518×552 |
| 14 | Visibility & Education | 1532×230 |
| 15 | Visibility & Education | 1290×348 |

## How this was captured

- Figures: fetched directly from `pbs.twimg.com`. `name=orig` 404s for most of
  this article's media, so the fallback chain is `orig → 4096x4096 → large →
  medium`; 12 of 15 resolved at `4096x4096`, 3 at `orig`.
- Text: converted from the rendered article DOM
  (`[data-testid="twitterArticleRichTextView"]`) to Markdown in the browser.
  x.com's CSP blocks `connect-src` and `form-action` to localhost, and the
  automation sandbox blocks page-initiated downloads, so the file was saved
  from the browser manually.
