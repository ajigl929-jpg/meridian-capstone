# Data Handling Checklist — Meridian Markets Capstone

Internal working doc for the workshop team. Source of truth for what can
and can't touch an AI tool (ChatGPT, Claude, Copilot, etc.), per the NDA
terms in `raw/client-brief.md`. Read that brief first if you haven't.

## Data classification

| Status | Data | Rule |
|---|---|---|
| 🔴 Restricted | Loyalty program membership & purchase history | Never into any AI tool — no pasting, uploading, or pointing a tool at the file, including excerpts |
| 🔴 Restricted | Labor scheduling & hours | Same as above |
| 🔴 Restricted | Any file/excerpt containing a customer or employee identifier | Same as above |
| 🟢 Safe | Sales totals by store and week | Explicitly cleared in the brief |
| 🟢 Safe | Store attributes (square footage, opening date, lease terms) | Explicitly cleared in the brief |
| 🟢 Safe, conditional | Raw/line-item POS transactions | Safe **only if** there's no customer/loyalty ID column linking a transaction to a person. Check for that column before treating a POS extract as safe. If present, treat the file as restricted. |
| 🟡 Case-by-case | Aggregates/derived outputs computed *from* restricted data (e.g., avg. spend by loyalty tier, labor cost as % of sales by store) | Not automatically safe just because it's aggregated. Needs a teammate's sign-off first — log it below. |

## The gate: before any AI-assisted step

Run through this every time, before pasting, uploading, or pointing an AI
tool (including Claude Code) at anything:

1. **Is this file/excerpt on the restricted list above?** → Stop. Do not use an AI tool. Work in Excel/SQL/a plain notebook instead.
2. **Does it contain a customer or employee identifier column** (loyalty ID, employee ID, name, etc.)? → Stop, same as above.
3. **Is it a derived aggregate built from restricted data?** → Get sign-off first (see log below), then proceed.
4. **None of the above?** → Safe to proceed.

When in doubt, treat it as restricted and ask, rather than guess.

## Sign-off log

Log every case-by-case approval here before using the output with an AI tool.

| Date | What was approved | Source data it came from | Approved by |
|---|---|---|---|
| | | | |

## Open questions (unresolved — confirm before Marcus's extract arrives)

1. **Will Claude Code ever be pointed directly at restricted files**, even
   for mechanical/non-interpretive steps (e.g., running a deterministic
   cleaning script)? Not yet decided as a team. **Default posture until
   resolved: no — restricted files stay out of any Claude Code session
   entirely.**
2. **Where will extract files be stored** once received (gitignored
   locally vs. outside the repo entirely)? Not yet decided. Recommend
   adding restricted-data patterns to `.gitignore` regardless of the
   final answer, as a safety net against an accidental commit.

## Why this exists

Dana's brief is explicit and non-negotiable: customer records and
employee data (loyalty data, labor schedules, and any excerpts of them)
do not go into any AI tool. Sales totals by store/week and store
attributes are fine. This doc translates that rule into something the
team can actually check against before each AI-assisted task, and keeps
a record of any judgment calls made along the way.
