# Data Integrity & Process Improvement Roadmap

### Applying ISO/IEC 27001:2022 building blocks to real findings from hands-on data integrity work

Reference framework: ISO/IEC 27001:2022, guided by the ISACA Germany Chapter Implementation Guide (2022)

---

## Context

This roadmap documents a set of real data-integrity and process findings, the fixes applied, and the work still ahead - organized using the ISMS "building block" structure from ISO/IEC 27001:2022. It's framed here as a fictional company (**SMSMarket**) to keep it safe to share publicly, but every finding below reflects skills and work I've actually done.

The environment: a small e-commerce/retail operation with a website and database managed by a third-party vendor (who maintains their own security roadmap for the platform itself), and an internal admin/sales side handling product data, pricing, and inventory across several separate files.

---

## Stage 1 - Diagnosis (what was found)

| # | Finding | Building block(s) | Owner |
|---|---|---|---|
| 1 | 100+ products with no unique product identifier; 50+ products with duplicate identifiers across variants (causing price conflicts at point-of-sale) | 3.6 Risk Management | Administrative team |
| 2 | No backup process on the central administrative data file | 3.6 Risk Management | Administrative team |
| 3 | Staff manually updating the same data across multiple separate files, occasionally missing an update in one, causing mismatches | 3.6 Risk Management, 3.9 Communication | Administrative & Sales teams |
| 4 | Team needed higher-quality marketing images but standard licensed stock services weren't a cost-effective fit | 3.11 Supplier Relationships | Administrative team |
| 5 | Out-of-stock products remained orderable rather than being deactivated from sale | 3.13 Incident Management, 3.6 Risk Management | Administrative & Sales teams |

---

## Stage 2 - Fixes implemented (resolved)

### Finding 1 - Product identifier duplication and gaps
**Action:** Built a script to systematically scan product data and identify every duplicate or missing identifier, rather than checking manually. Generated compliant new identifiers to resolve conflicts. Applied the fix consistently across the product database, spreadsheet records, and the point-of-sale system.
**Building block:** 3.14 Continual Improvement - this addressed the root cause (variant products sharing one identifier), not just the individual conflicts as they appeared.
**Status:** ✅ Resolved.

### Finding 2 - No backup on the admin file
**Action:** Raised the issue with the team; a backup process was implemented for the file going forward.
**Status:** ✅ Resolved.

### Finding 4 - Licensed asset use
**Action:** Proposed a lower-cost licensed alternative for marketing/design images, and documented the specific usage boundary (usable within a composed design or print output, but not exportable as a standalone image) so the rule would be clear and repeatable rather than relying on memory.
**Building blocks:** 3.8 Documentation, 3.10 Awareness - the documentation is what makes this fix durable; without it, the same licensing risk could resurface with new staff or over time.
**Status:** ✅ Documented and adopted.

---

## Stage 3 - Proposed / in progress

### Finding 3 - Manual multi-file update process
**Action:** Proposed and built a script-based automation to synchronize product data (prices, names, identifiers) across the relevant files automatically, replacing manual updates.
**Building block:** 3.14 Continual Improvement - this is a root-cause fix (removing the manual step that caused mismatches) rather than repeatedly correcting individual mismatches after the fact.
**Status:** 🟡 Built and proposed to the team as a replacement for the manual process.

### Finding 5 - Out-of-stock products remaining orderable
**Status:** 🟡 Identified and flagged to the team as a process gap.

---

## Stage 4 - Remaining open items (not yet resolved)

These are still in progress as of writing, tracked and coordinated with the team via a shared task board:

- Duplicate product listings appearing on the website
- Missing product information for some listings
- Some products missing price or identifier data specifically on the website (separate from the backend data fix already applied)
- Product images not yet updated across the full catalog

---

## Reflection

The throughline across these findings is the same pattern: **a manual, single-point process (barcode assignment, file updates, backup, licensing knowledge) breaks down at scale**, and the durable fix is usually process automation or documentation, not a one-off correction. That's the core idea behind ISO 27001's Continual Improvement building block — fixing symptoms keeps you busy; fixing root causes is what actually reduces risk over time.

---

*This document generalizes real skills and findings into a fictional company scenario. It excludes any information that could identify a real employer, and excludes any legally or contractually sensitive details.*
