# Meridian Markets Capstone

## Data handling — read before touching any data file

Full rules: `docs/data-handling-checklist.md`. Summary for this session:

**Never read, summarize, analyze, or otherwise process:**
- Loyalty program membership or purchase history
- Labor scheduling or hours data
- Any file/excerpt containing a customer or employee identifier (name, loyalty ID, employee ID)

This includes writing or running a script against a restricted file for a
purely mechanical step (e.g., stripping a column, deduplicating rows) —
"just running a script" is not an exception. Any cleaning of a restricted
file happens outside any AI tool entirely, by the user, in a plain
terminal/notebook/Excel; only the already-clean result comes back to
Claude Code.

If asked to work with a file matching those, or any file whose contents
are unclear, stop and ask whether it's restricted before opening it.

**Safe to use freely:**
- Sales totals by store and week
- Store attributes (square footage, opening date, lease terms)
- Raw POS transaction data, *only if* it has no customer/loyalty ID column

**Needs human sign-off first:**
- Any aggregate or derived output computed from restricted data (e.g.,
  spend by loyalty tier, labor cost ratios) — even though it's
  aggregated, don't treat it as automatically safe. Ask the user to
  confirm it's been signed off (see the sign-off log in the checklist)
  before using it in analysis.

Default posture on anything ambiguous: treat as restricted.
