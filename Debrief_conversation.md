# PMM Market Intelligence Agent — Audit Debrief
**Date:** May 2026
**Participants:** [Navarro] · [Olalla]

---

## 1. Auditor Presents

Builder listened carefully through the presentation.

The external audit found seven blocking findings and recommended **Do not proceed**. This is the central divergence of the whole exercise — both audits identified the same seven failures, but reached opposite verdicts on what to do next.

The most critical issues are: no lawful basis for contacting the email recipient, no DPAs with any vendor, no DPIA conducted, and the autonomous email send function operating without any human review.


---

## 2. Builder Responds

The builder was aware of some data protection issues, but not to the extent of the infringed laws that were not considered.

The builder clarified that the system is in single-user testing mode only and no real third-party personal data has been processed at scale yet. The Gmail integration was intended as a lightweight CRM proxy, not as a full inbox scrape — the scope felt limited in practice, even if the OAuth2 permissions are broad. The autonomous send was a technical proof of concept, not an intended production feature.

No clarification changes a finding.
---

## 3. Compare lawful basis selections

Both audits selected legitimate interests for P1 (Market Research Deck), P2 (ICP One Pager), and P4 (LLM inference across all nodes). The only divergence is on P3 — the autonomous email send.
The self-audit proposed legitimate interests for P3 but flagged it would probably fail the balancing test. The external audit went further and concluded that no lawful basis is available at all in the current design, because legitimate interests requires a transparency mechanism and a completed LIA — neither of which exists — and consent cannot be obtained retroactively for a recipient who was never asked.
The agreed position after discussion is that P3 has no lawful basis in its current form. Legitimate interests could theoretically apply after redesign, but only if the recipient is informed, the LIA is completed, and a human reviews the send before it happens.

## 4. Compare DPIA conclusions

Both audits agree that a DPIA is required. There is no disagreement on the conclusion.

## 5. Compare gap lists

What the **external audit** caught that the self-audit missed: The self-audit correctly noted that the sent email stays in Gmail, but missed that the recipient identity is generated inside OpenAI's US infrastructure first — so the data crosses the EU border during the inference step, before the email is composed. The self-audit also noted the personal Gmail account as a gap, but the external audit went further and identified it as a structural blocking finding: it is an architecture problem that cannot be fixed with a contract — the account must migrate to Google Workspace. Finally, the self-audit acknowledged third-party data subjects throughout but never landed on Art. 14 as a standalone obligation with a one-month notification deadline.

What the **self-audit** caught that the external audit missed: The self-audit correctly identified that accessing Gmail message content triggers the ePrivacy Directive's consent requirement independently of GDPR, and that legitimate interests does not satisfy it. The external audit did not address ePrivacy at all. The self-audit also flagged OAuth2 scope creep specifically — naming the broad Drive and Gmail permissions as a technical fix point — where the external audit only addressed data minimisation in general terms. The self-audit also raised the sole trader risk: if the target company URL resolves to an individual, publicly retrieved web research data becomes personal data. The external audit did not raise this.

## 6. Joint closing note

Self-assessment is a good way of working out what is wrong — both audits found the same major problems. The problem with self-assessment is that it's hard to know how serious a finding is. The builder always thought that blocking gaps were things that needed to be fixed, but an external reviewer thought that the same gaps were reasons to stop first. The hardest gaps to spot in your own work are the ones that require you to think from the perspective of someone who has no relationship with your system at all. In this case, that means thinking about the Gmail senders and the email recipient, whose data flows through the agent without their knowledge. When you build a system, you naturally think about the person who will use it. The job of an external auditor is to ask who else is affected.