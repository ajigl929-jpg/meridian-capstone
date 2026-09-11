# Data Sources

Type: concept | Last updated: 2026-09-11
Sources: [raw/client-brief.md](../../../raw/client-brief.md)

The brief names four data sources [Marcus](../entities/marcus.md) can
extract once the NDA is signed:

| Source | Classification | Notes |
|---|---|---|
| POS transactions (~3 years) | 🟢 Safe, conditional | Only if there's no customer/loyalty ID column — check before use |
| Loyalty program membership & purchase history (~40,000 members) | 🔴 Restricted | Never into an AI tool — see [Loyalty Program](loyalty-program.md) |
| Labor scheduling & hours | 🔴 Restricted | Never into an AI tool |
| Store attributes (sq ft, opening date, lease terms) | 🟢 Safe | Explicitly cleared in the brief |

Full rules: [docs/data-handling-checklist.md](../../data-handling-checklist.md).

## Related

- [Marcus](../entities/marcus.md)
- [Loyalty Program](loyalty-program.md)
- [Open Questions](../analysis/open-questions.md)
