# ISO/IEC 27001:2022 Implementation Roadmap
### A phased ISMS rollout for a small e-commerce retailer, starting from zero

**Author:** Sharumathiselvi Janakiraman
**Framework reference:** ISO/IEC 27001:2022, guided by the ISACA Germany Chapter Implementation Guide (2022)

---

## 1. Purpose of this project

This is a self-directed portfolio project. It applies the building blocks of an ISMS described in ISO/IEC 27001:2022 to a realistic, small e-commerce business that has no formal information security program today. The goal is to demonstrate a practical, phased path from "no ISMS" to "audit-ready," including scope definition, risk assessment, control selection, documentation, and continual improvement of the same lifecycle described in section 3 of the ISACA Implementation Guide.

The company used below, **SMSMarket**, is fictional but modeled on real challenges typical of a small retail/e-commerce business: a product catalog synced across systems, a point-of-sale (POS) import pipeline, customer and payment data, and a small IT team with no dedicated security function.

---

## 2. Company profile

| | |
|---|---|
| **Company** | SMSMarket (fictional) |
| **Sector** | E-commerce & retail |
| **Size** | ~45 employees |
| **IT footprint** | Cloud-hosted e-commerce platform, POS system, Google Workspace, 3 external service providers (payment processor, logistics partner, hosting provider) |
| **Data handled** | Customer PII, payment card data (via processor, not stored directly), product/inventory data |
| **Current state** | No ISMS, no formal risk register, ad hoc IT practices, no dedicated security role |
| **Trigger for the project** | A key B2B client has requested evidence of information security controls before renewing a contract |

This profile draws on skills I've developed in similar contexts - data integrity auditing, POS/import pipeline troubleshooting, and multi-system product data synchronization; so the risk scenarios below are grounded in plausible, concrete situations rather than abstract examples.

---

## 3. Scope of the ISMS (Section 3.1 of the guide)

Following the guidance on defining scope through an environment and requirements analysis:

**In scope:**
- The e-commerce platform and its customer-facing data flows
- The POS import pipeline and product data synchronization process
- Employee endpoints and Google Workspace environment
- Relationships with the payment processor and hosting provider (as they affect the above)

**Out of scope (for this initial certification cycle):**
- The physical warehouse and logistics partner's internal systems (covered instead by a supplier security clause, see Phase 4 below)
- HR systems unrelated to customer data processing

**Stakeholders identified (Section 4.2 analysis):**

| Stakeholder | Requirement |
|---|---|
| B2B client (contract renewal trigger) | Evidence of ISMS / security controls |
| Customers | Confidentiality of PII, GDPR compliance |
| Payment processor | PCI-DSS-adjacent handling requirements |
| Regulators | GDPR (EU customers) |
| Employees | Clear, workable security guidance (not obstructive) |

---

## 4. Implementation approach - phased roadmap

The rollout follows the PDCA cycle described in Section 3.14 of the guide, broken into six phases over approximately 12 months. This mirrors how a small company with no existing ISMS would realistically build one incrementally, rather than attempting everything simultaneously.

```
Phase 1: Foundation           (Months 1-2)
Phase 2: Risk Assessment      (Months 3-4)
Phase 3: Control Selection    (Months 5-7)
Phase 4: Operationalization   (Months 8-9)
Phase 5: Monitoring & Audit   (Months 10-11)
Phase 6: Review & Improve     (Month 12+, ongoing)
```

---

### Phase 1 - Foundation (Context, Leadership, Policy)

**Building blocks addressed:** 3.1 Context of the Organization, 3.2 Leadership and Commitment, 3.4 IS Policy, 3.5 Roles & Responsibilities

**Activities:**
1. Conduct the environment and requirements analysis (Section 3.1) - map internal departments (IT, customer service, finance) and external parties (payment processor, hosting provider, logistics partner) relevant to the ISMS.
2. Secure top management sponsorship. At SMSMarket's size, "top management" is the founder/CEO — per the guide's note that top management does not have to mean group-level leadership, only whoever controls resourcing for the scope in question.
3. Draft the Information Security Policy (Section 3.4) — a short, company-specific document, not a generic template, stating management's commitment to the ISMS and its continuous improvement.
4. Define roles: since SMSMarket has no dedicated security hire, assign the IT Lead as interim Information Security Officer (ISO), with the CEO as executive sponsor, avoiding the conflict-of-interest patterns flagged in Section 3.5 (e.g., ISO and IT Manager should not be the same person long-term, but is an acceptable interim step for a company this size).

**Deliverable:** Signed Information Security Policy, defined ISMS scope document, RACI matrix for security-relevant roles.

---

### Phase 2 - Risk Assessment & Treatment

**Building block addressed:** 3.6 Risk Management

**Activities:**
Following the four-step risk management process from the guide (identification, analysis, evaluation, treatment):

1. **Risk identification** via structured interviews with the IT Lead, customer service manager, and finance — the guide's recommended approach of combining multiple stakeholder viewpoints rather than a single technical assessment.
2. **Risk analysis** using a simplified 4x4 matrix (probability × impact), avoiding the "default to the middle" problem the guide flags with odd-numbered matrices.
3. **Risk register** - see the sample below.

**Sample risk register (excerpt):**

| ID | Risk | Likelihood | Impact | Risk level | Owner | Treatment |
|---|---|---|---|---|---|---|
| R-01 | Product data desync between e-commerce platform and POS leads to pricing errors at checkout | Medium | Medium | Moderate | IT Lead | Reduce — Google Apps Script automation to synchronize product data (prices, names, barcodes) across Google Sheets, replacing manual updates, plus a weekly manual reconciliation check |
| R-02 | Payment processor API credentials exposed via a misconfigured environment variable | Low | High | High | IT Lead | Reduce — secrets management tool, quarterly credential rotation |
| R-03 | Customer PII exported for marketing without a documented legal basis | Medium | High | High | Marketing Lead | Reduce — data processing register, GDPR review of marketing exports |
| R-04 | Single employee holds admin access to the e-commerce platform with no backup/offboarding process | High | Medium | High | IT Lead | Reduce — role-based access control, documented offboarding checklist |
| R-05 | Hosting provider suffers an outage with no documented recovery point | Low | High | Moderate | IT Lead | Transfer — contractual SLA with hosting provider + backup verification |
| R-06 | Duplicate GS1 barcodes assigned to product variants cause POS import conflicts and incorrect inventory records | Medium | Medium | Moderate | IT Lead | Reduce — scripted duplicate-detection check against the GS1 barcode registry before each import, with compliant barcode regeneration for conflicts |
| R-07 | Staff use free public web tools to edit sensitive documents (pricing, customer data) or reuse licensed design assets outside their license terms | Medium | Medium | Moderate | Marketing Lead | Reduce — staff KT session on licensed/secure tool use, restricted to company-approved software only |

**Deliverable:** Risk assessment methodology document, populated risk register, risk treatment plan with owners and target dates.

---

### Phase 3 - Control Selection (Statement of Applicability)

**Building block addressed:** Annex A controls, Statement of Applicability (Section 3.1, 6.1.3)

For each risk in the register, map to relevant Annex A controls and justify inclusion/exclusion in the Statement of Applicability (SoA).

**Sample SoA excerpt:**

| Annex A control | Applicable? | Justification | Linked risk |
|---|---|---|---|
| 5.15 Access control | Yes | Directly mitigates R-04 | R-04 |
| 5.17 Authentication information | Yes | Password/credential hygiene for platform access | R-02, R-04 |
| 8.24 Use of cryptography | Yes | Encrypt stored customer PII and payment references | R-03 |
| 5.9 Inventory of information and assets | Yes | Required to know what data exists before protecting it | R-03 |
| 8.16 Monitoring activities | Yes | Detect anomalies in POS import pipeline | R-01 |
| 8.9 Configuration management | Yes | Google Apps Script automation to synchronize product data (prices, names, barcodes) across Google Sheets, replacing error-prone manual updates | R-01 |
| 7.4 Physical security monitoring | No | No physical premises with sensitive processing in scope | - |
| 8.30 Outsourced development | No | No outsourced software development at this stage | - |

**Deliverable:** Completed Statement of Applicability, control implementation plan.

---

### Phase 4 - Operationalization

**Building blocks addressed:** 3.8 Documentation, 3.9 Communication, 3.10 Awareness, 3.11 Supplier Relationships

**Activities:**
1. **Documentation** — establish a lightweight document control process (owner per document, version tracking) sized appropriately for a 45-person company, avoiding the over-documentation trap the guide warns against.
2. **Communication plan** — internal (weekly security note from IT Lead), external (supplier security requirements added to the payment processor and hosting provider contracts).
3. **Awareness campaign** — following the guide's 5-phase model (needs assessment → confrontation → sensitization → sustainability → evaluation):
   - Needs assessment: staff mostly handle customer data and POS entries, so phishing and credential hygiene are the priority topics, not, e.g., physical badge security.
   - A simulated phishing email test as the "confrontation" phase, run with prior notice to the CEO (per the guide's ethical-conduct note).
   - Short onboarding training + quarterly refreshers as the sustainability mechanism.
   - A dedicated knowledge-transfer session on **asset licensing and secure tool use**, covering two practical rules relevant to the marketing/admin team: (1) stock design assets (e.g., licensed template libraries) may only be used within composed designs, not exported or reused as standalone images, to stay within license terms; and (2) sensitive documents (e.g., anything containing customer or pricing data) must only be edited using the company's licensed, account-connected tools — never free public online converters or editors, which is a real and easily overlooked data-exposure channel in small teams that default to convenient free web tools.
4. **Supplier relationships** — add a basic security clause and right-to-audit language to the payment processor and hosting provider contracts, addressing R-02 and R-05.

**Deliverable:** Document control procedure, communication plan, awareness training materials, updated supplier contracts.

---

### Phase 5 - Monitoring & Internal Audit

**Building blocks addressed:** 3.7 Performance/Risk/Compliance Monitoring, 3.12 Internal Audit

**Activities:**
1. Define a small set of KPIs appropriate to the company's size (avoiding the guide's warning against tracking too many metrics too early):

| Metric type | Example KPI | Target |
|---|---|---|
| KPI | % of staff completing security awareness training | 100% within 30 days of hire |
| KRI | % of admin accounts without MFA enabled | 0% |
| KCI | Time from vulnerability identification to patch | < 14 days for high severity |

2. Conduct a first internal audit against the SoA and policy, following the audit program structure from Section 3.12 (assignment → planning → fieldwork → reporting → follow-up), performed by the CEO or an external contractor to preserve independence from the IT Lead who owns most of the controls.

**Deliverable:** KPI dashboard (initial baseline), internal audit report with findings and corrective actions.

---

### Phase 6 - Review & Continual Improvement

**Building blocks addressed:** 3.13 Incident Management, 3.14 Continual Improvement

**Activities:**
1. Stand up a lightweight incident response plan (Section 3.13's five-phase model: plan/prepare → recognize/accept → classify/decide → respond → lessons learned), sized for a company without a dedicated security team e.g., the IT Lead as first responder, a simple reporting channel via a shared inbox, and a documented escalation path to the CEO for anything involving customer data.
2. Hold the first management review, feeding findings from the internal audit, risk register changes, and any incidents into a documented set of corrective actions - the PDCA "Act" step.
3. Set the cadence going forward: quarterly risk register review, annual internal audit, annual management review — a realistic rhythm for a company this size, rather than the more frequent cycle a larger enterprise might run.

**Deliverable:** Incident response plan, first management review minutes, continual improvement log.

---

## 5. Reflection - what this project demonstrates

- Ability to translate a general framework (ISO/IEC 27001:2022) into a scoped, sequenced plan for a specific, resource-constrained organization rather than a generic checklist.
- Comfort working across the "three views" the guide describes in its introduction — governance, risk, and compliance — rather than treating ISMS work as a purely technical exercise.
- A realistic sense of proportion: this roadmap deliberately does not over-engineer controls or documentation for a 45-person company, reflecting the guide's repeated caution that ISMS maturity should match organizational size and risk appetite.

---

## 6. References

- ISACA Germany Chapter e.V., *Implementation Guide ISO/IEC 27001:2022* (2022)
- ISO/IEC 27001:2022 - Information security, cybersecurity and privacy protection — Information security management systems - Requirements
- ISO/IEC 27005:2022 - Guidance on managing information security risks
- ISO 31000:2018 — Risk management guidelines

---

*This is an educational/portfolio project based on a fictional company. It does not represent an actual client engagement.*
