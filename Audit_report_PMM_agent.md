# PMM Market Intelligence Agent — GDPR Peer Audit Report
**Prepared by:** External GDPR Auditor
**Date:** May 2026
**Based on:** Data Processing Brief (May 2026, MVP / single-user testing mode)

---

## Section 1: System Summary

The PMM Market Intelligence Agent is an autonomous n8n workflow that takes a target company URL as input and produces three commercial outputs — a Market Research Deck, an ICP One Pager, and an outbound sales email — without any human review before delivery. To generate these outputs, the agent pulls the last ten messages from the authenticated user's Gmail inbox, reads up to five documents from their Google Drive, and performs web research via OpenAI's built-in search tool. All three data streams are merged and sent to GPT-4o-mini for inference. The most significant aspect of the system's operation is that it autonomously identifies a sales email recipient by inferring their identity from the Gmail and Drive content, then sends an unsolicited commercial email to that person via Gmail Send — a sequence that occurs entirely without human intervention and without the recipient's knowledge.

---

## Section 2: Data and Role Map

**Personal data in scope:** Gmail message content (subjects, snippets, body text) from third-party senders; Google Drive document text including UXR interview transcripts that may contain Art. 9-category data; the autonomously inferred name and email address of the sales email recipient; and the authenticated user's Google account identifier. The most acute data subject rights deficit belongs to the email recipient, who is profiled and contacted without notice, consent, or any assessed lawful basis.

**Controller / processor split:** The deploying organisation or individual is the Controller. The builder/teammate is a Processor during development (no DPA in place). OpenAI is a Processor for all LLM inference (no DPA in place). Google is a Processor for Gmail, Drive, and Docs API operations — but critically, the authenticated account is a personal Gmail account, which carries no Art. 28 DPA; Google Workspace would be required for a compliant processing relationship. n8n Cloud (EU-hosted) is a Processor for workflow orchestration; DPA available but not confirmed as executed.

**International transfers:** Every session constitutes multiple EEA-to-US transfers — Gmail content, Drive documents, and inferred recipient identity all flow to OpenAI (USA) on each run. No Standard Contractual Clauses, no DPF reliance, and no Transfer Impact Assessment has been documented for any of these flows. The personal Gmail account means the Google relationship itself is also outside any compliant transfer framework.

---

## Section 3: Compliance Findings

---

**Finding 1 — Art. 6 GDPR: No lawful basis for processing the email recipient's personal data**
**Severity: Blocking**
**Description:** The sales email recipient is identified autonomously by the LLM and contacted without any prior relationship with the system, without notice, and without any lawful basis under Art. 6 having been assessed. Consent has not been obtained. Legitimate interests cannot be relied upon without a completed LIA and a transparency mechanism — neither exists. Contract performance and legal obligation do not apply. This individual is the most exposed data subject in the system and the processing that affects them most directly has no legal foundation.
**Recommended action:** Suspend the autonomous email send function immediately. Redesign P3 to require the user to manually supply the recipient's identity and confirm the send after reviewing the draft — this removes the autonomous profiling element and creates a human decision point that substantially reduces the Art. 22 and Art. 6 exposure simultaneously.
**Escalation needed:** Yes — legal counsel, before any further use of the send function.

---

**Finding 2 — Art. 5(1)(b) GDPR: Purpose limitation failure — Gmail content reused for incompatible purpose**
**Severity: Blocking**
**Description:** Gmail inbox content was collected for personal communication between the user and their correspondents. Its use as CRM intelligence to generate commercial sales outreach is a materially different purpose. The brief correctly identifies this as a purpose limitation failure and notes that no Art. 6(4) compatibility assessment has been conducted. On the available facts — the nature of the data (private communications), the consequences for third parties (unsolicited commercial contact), and the absence of safeguards — a compatibility finding is unlikely.
**Recommended action:** Conduct a formal Art. 6(4) compatibility assessment. If, as expected, compatibility cannot be established, Gmail inbox content must be removed from the P3 pipeline entirely. A user-supplied CRM context field in the Gradio interface would achieve the same personalisation goal without processing third-party communications data.
**Escalation needed:** Yes — legal counsel to complete the compatibility assessment.

---

**Finding 3 — Art. 9 GDPR: Special-category data processed without an Art. 9(2) condition**
**Severity: Blocking**
**Description:** The brief confirms that Google Drive documents may include UXR interview transcripts containing health, ethnicity, and other Art. 9-category data, and that the agent has no mechanism to detect or exclude such content before sending it to OpenAI. No Art. 9(2) condition has been identified. This is not a hypothetical risk — the brief treats it as a known characteristic of the document corpus. Every session involving UXR documents constitutes potential unlawful processing of special-category data.
**Recommended action:** Implement a pre-processing classification step that screens Drive documents for Art. 9 content before any data is sent to OpenAI. Where Art. 9 content is detected, the document must be excluded from the session or the relevant passages redacted. Until this control is in place, the agent must not be used with Drive documents that may contain UXR or research participant data.
**Escalation needed:** Yes — DPO and legal counsel.

---

**Finding 4 — Art. 28 GDPR: No DPA with any external processor**
**Severity: Blocking**
**Description:** No Data Processing Agreement is in place with OpenAI, Google (personal account carries no DPA), or n8n Cloud. Every session in which personal data flows to these vendors is a breach of Art. 28, which makes DPAs a legal requirement for all controller-processor relationships — not a best practice. The personal Gmail account issue is the most structurally serious: it cannot be resolved by executing a DPA on the existing account; it requires migration to Google Workspace.
**Recommended action:** (1) Execute OpenAI's API DPA via the platform portal immediately. (2) Migrate the authenticated account to a Google Workspace account and execute Google's Workspace DPA. (3) Execute n8n Cloud's DPA. None of these steps should be deferred — they are prerequisites for any processing of third-party personal data.
**Escalation needed:** Yes — legal counsel and IT/operations for Workspace migration.

---

**Finding 5 — Art. 46 GDPR: No Chapter V transfer mechanism for any EEA-to-US data flow**
**Severity: Blocking**
**Description:** Every session transfers personal data from multiple third-party data subjects to OpenAI (USA) and Google (USA). No SCCs, no DPF reliance documentation, and no Transfer Impact Assessment exists for any of these flows. This compounds Finding 4 — even once DPAs are in place, the transfer mechanism must be separately documented and assessed.
**Recommended action:** Confirm OpenAI's DPF certification status. If certified, document reliance and note challenge risk. Attach SCCs Module 2 to the OpenAI DPA as a fallback. Complete a TIA given the sensitivity of Gmail and Drive content sent in prompts. Apply the same process to Google once migrated to Workspace.
**Escalation needed:** Yes — legal counsel for TIA.

---

**Finding 6 — Art. 35 GDPR: DPIA required but not conducted**
**Severity: Blocking**
**Description:** The system meets at minimum four EDPB criteria: systematic processing of communication content, automated decision-making with significant effects (autonomous email send), confirmed Art. 9-category data, and innovative LLM technology applied to third-party personal data without their knowledge. Two criteria are sufficient to trigger a mandatory DPIA; four are present here.
**Recommended action:** Conduct a DPIA before any further use of the system with real personal data. The DPIA scope must cover the Gmail and Drive data flows, the recipient inference and send function, and the OpenAI transfer.
**Escalation needed:** Yes — DPO must be consulted; DPIA cannot be self-certified by the builder.

---

**Finding 7 — Art. 22 GDPR: Automated decision-making without safeguard**
**Severity: Blocking**
**Description:** The autonomous identification and commercial targeting of the email recipient constitutes automated processing that produces a similarly significant effect on an individual who has no relationship with the system. No human intervention mechanism, no right to contest, and no explanation of decision logic is in place. The brief's own assessment supports this conclusion.
**Recommended action:** Suspend the autonomous send. If the send function is retained after redesign, it must include: mandatory human review of the generated email and recipient identity before send; a disclosed mechanism for the recipient to object and have their data erased; and documented logic explaining how the recipient was selected.
**Escalation needed:** Yes — legal counsel.

---

**Finding 8 — Art. 14 GDPR: No transparency notice for third-party data subjects**
**Severity: Significant**
**Description:** Gmail senders, UXR research participants, and the email recipient are all Art. 14 data subjects — their data is processed without having been collected from them directly. No transparency notice has been issued or designed for any of these groups. Art. 14 requires notice within one month of data collection, or at first communication for the email recipient.
**Recommended action:** Design and issue Art. 14 notices for each data subject category. For the email recipient specifically, the first communication (the sales email itself) must include disclosure that their data was processed by an AI system and how they can exercise their rights.
**Escalation needed:** No — can be addressed by the team with legal review.

---

## Section 4: GDPR Obligations Checklist

| Obligation | Assessment | Note |
|---|---|---|
| Lawful basis identified for each processing purpose | **Gap identified** | No basis assessed for recipient profiling and email send; Gmail reuse basis incompatible; UXR data lacks Art. 9 condition |
| Purpose limitation respected | **Gap identified** | Gmail content reused for commercial outreach — textbook Art. 5(1)(b) failure confirmed by the brief |
| Data minimisation | **Gap identified** | Full Gmail message bodies pulled when subjects/snippets may suffice; all Drive docs sent to LLM without relevance filtering |
| Controller/processor roles mapped and DPAs in place | **Gap identified** | Role map exists in brief; zero DPAs in place; personal Gmail account cannot be brought into compliance without account migration |
| International transfer mechanism documented | **Gap identified** | No SCCs, no DPF reliance, no TIA for any of the US transfers |
| DPIA conducted if required | **Gap identified** | DPIA mandatory; not initiated |
| Art. 22 safeguard in place | **Gap identified** | Autonomous send with no human review, no right to contest, no explanation |
| Privacy notice covers AI processing | **Gap identified** | No Art. 13 or Art. 14 notice exists for any data subject category |
| Data subject rights operationalisable within deadlines | **Cannot determine from brief** | No rights workflow documented; erasure from Gmail Sent and OpenAI logs would require external vendor coordination with no documented procedure |

---

## Section 5: Overall Recommendation

**Do not proceed.**

The PMM Market Intelligence Agent cannot be used with real personal data in its current form. Seven of the eight compliance findings are Blocking — meaning they represent either the absence of a lawful basis for an active processing activity, or a structural condition (no DPA, no transfer mechanism, no DPIA) that makes every session a breach of GDPR regardless of intent. The most urgent concern is the autonomous email send function, which profiles and contacts individuals without any legal basis, any human oversight, or any transparency — a combination that represents one of the clearest Art. 22 violations the system design could produce. The personal Gmail account issue compounds every other finding: until the system is migrated to a Google Workspace account with an executed DPA, the entire Google relationship operates outside any GDPR-compliant processing framework. No remediation of the other findings can substitute for this structural change. The team should suspend use of the system with any real third-party personal data, resolve the blocking findings in the sequence set out above, and conduct a DPIA before restarting.

---

## Section 6: What This Report Is Not

This report is a structured compliance assessment prepared for peer review and educational purposes. It is not a legal opinion, not a completed DPIA, and not a certification of GDPR compliance or non-compliance. The findings and recommendations reflect the reviewer's analysis of the data processing brief as provided and are subject to the clarifying questions logged in Phase 3, none of which have been formally answered. The controller must obtain qualified legal counsel before relying on any finding in this report for compliance decisions, contractual commitments, or regulatory submissions.