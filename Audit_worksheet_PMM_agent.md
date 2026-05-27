# Audit worksheet for PMM Market Intelligence Agent Brief

---

## Phase 1: Annotated Review

---

### Personal Data Categories — Identified and Annotated

| # | Data mentioned in brief | Special-category status | Notes |
|---|---|---|---|
| 1 | Gmail sender names and email addresses | No — standard identifiers | Third-party data subjects who did not interact with this system in any capacity |
| 2 | Gmail message subjects, snippets, and body content | No — but **content is unconstrained** | ⚠️ Message body content could contain health information, religious views, financial distress, or other Art. 9-inferable content; no filter exists |
| 3 | Google Drive document text — UXR interview transcripts, research notes, verbatim quotes | **Potentially yes** ⚠️ | Research participant data is explicitly flagged as potentially including health, ethnicity, and other Art. 9 data; the brief correctly identifies this but offers no mitigation |
| 4 | Research participant names, roles, organisations, verbatim quotes | No — but **indirectly identified individuals** | These are third parties who consented to UXR research, not to LLM processing or sales outreach; their data is being repurposed without their knowledge |
| 5 | Email recipient identity — name and email address inferred by LLM | No | ⚠️ **Highest risk category in the system:** this individual is identified, targeted, and contacted without knowledge, consent, or any legal basis assessed |
| 6 | Authenticated user's Google account email address | No | Low risk; standard account identifier; user is the system operator |

---

### Annotations by Section

---

**Section 1 — What personal data does the system process?**

> *"Message body content, which includes communications authored by third parties who are not party to this system and have no knowledge their data is being processed"*

🔴 **FLAG — Data subject without notice or basis.** The third-party email senders are data subjects under GDPR. They have no relationship with this system, no privacy notice has been issued to them, and no lawful basis for processing their communications has been assessed. This is not a gap — it is an absence.

> *"The agent has no mechanism to detect or exclude Article 9 data from Google Drive documents before passing them to OpenAI"*

🔴 **FLAG — Art. 9 exposure confirmed by the brief itself.** The author has correctly identified this but proposed no mitigation. The absence of a redaction or classification step means that every session involving UXR documents carries a risk of transferring special-category data to a US processor with no Art. 9(2) condition in place. This needs to be treated as a live risk, not a theoretical one.

> *"The name and email address of the sales email recipient, autonomously inferred by GPT-4o-mini"*

🔴 **FLAG — No lawful basis identified for this processing.** The recipient is identified by an LLM inferring from data the recipient never provided to this system. This is not retrieval; it is autonomous profiling. No lawful basis under Art. 6, no Art. 22 safeguard, and no transparency obligation towards the recipient has been assessed or implemented.

---

**Section 2 — Where does the data come from?**

> *"Email recipient identity — inferred autonomously by GPT-4o-mini from Gmail and Drive content — not supplied by the user"*

🟡 **UNCLEAR — Confirm before forming a view.** What exactly does the LLM infer from? Does it extract a name from a prior email thread? Does it match a contact to a company via web search? The inference mechanism determines whether this is retrieval of existing data or generation of a new derived profile — the distinction matters for the Art. 22 analysis. The brief does not describe the inference logic with sufficient precision to conclude.

---

**Section 3 — What is the data used for?**

> *"Gmail content is used as CRM context to personalise an outbound sales email"*

🔴 **PURPOSE INCOMPATIBILITY FLAG — Art. 5(1)(b) / Art. 6(4).** Gmail inbox content was collected for personal communication between the user and their correspondents. Its use as CRM intelligence to generate commercial outreach is a materially different purpose. The brief correctly identifies this as a purpose limitation failure but does not assess the Art. 6(4) compatibility factors (link between purposes, context, nature of data, consequences, safeguards). This analysis must be completed — and on the available facts, it is unlikely to produce a finding of compatibility.

> *"Drive document content...sent to GPT-4o-mini to generate an Ideal Customer Profile"*

🟡 **PURPOSE INCOMPATIBILITY FLAG — secondary, but real.** UXR interview documents were collected under a research consent framework. Using them as input to a commercial ICP generation tool is a secondary purpose that the research participants almost certainly did not anticipate or consent to. The compatibility analysis under Art. 6(4) is needed here too, and the nature of the data (research participant quotes, potentially sensitive) makes it harder to argue compatibility.

> *"The email is sent autonomously via Gmail Send to a recipient inferred by the LLM — no human review before delivery"*

🔴 **HIGHEST RISK FLAG — Art. 22 + Art. 6 combined.** This is the single most significant compliance risk in the entire system. There is no lawful basis identified for contacting the recipient, no human in the loop before the email is sent, no transparency towards the recipient, and no mechanism to contest or reverse the action. The brief acknowledges this but recommends only that it "should be suspended or redesigned" — that recommendation should be strengthened to **must be suspended** before any further use.

---

**Section 4 — Who processes the data?**

> *"OpenAI API (USA) — Receives on every session: Gmail message content, Google Drive document text, web search results, and inferred recipient identity"*

🔴 **CROSS-BORDER TRANSFER FLAG.** Every single session constitutes an EEA-to-US transfer of personal data from multiple third-party data subjects (email senders, UXR research participants, the sales email recipient) — none of whom have been informed of or consented to international transfer. No SCCs, no DPF reliance, no TIA documented.

> *"No DPA exists between the controller and Google for personal Gmail accounts — Google Workspace carries a DPA; personal accounts do not"*

🟡 **UNCLEAR — Confirm before forming a view.** This is a critical structural issue. If the controller is an organisation, using a personal Gmail account as the operational backbone of a business data processing system means the entire Google relationship is outside any Art. 28 DPA. Confirm: is the authenticated account a personal account or a Workspace account? If personal, this is not a gap that can be papered over — the account needs to change.

---

**Section 5 — Where is data stored?**

> *"Autonomously sent sales email retained in Gmail Sent folder; no deletion schedule"*

🟡 **FLAG — Indefinite retention of third-party personal data.** The sent email contains the recipient's name and email address, and the content generated by the LLM about them. It sits in a personal Gmail account indefinitely with no retention schedule and no mechanism for the recipient to exercise erasure rights. This is a retention principle violation under Art. 5(1)(e).

---

**Section 6 — Automated decision-making**

> *"The agent autonomously selects a sales email recipient...then sends an outbound email to that person without any human review before delivery"*

🔴 **Art. 22 FLAG — Confirmed.** The brief's own Art. 22 assessment is correct and well-reasoned. The gap is that it concludes with a recommendation to redesign, when the appropriate immediate action is suspension. A system that sends unsolicited commercial emails to individuals inferred by an LLM from third-party communications data, without any human review, is not a borderline Art. 22 case — it is the scenario the provision was designed to address.

---

### Summary of Annotation Flags

| Flag | Count | Severity |
|---|---|---|
| 🔴 Hard compliance failure — must be resolved before any further use | 6 | Critical |
| 🟡 Unclear — requires confirmation before forming a final view | 4 | Material |
| Art. 9 latent exposure (confirmed by brief) | 1 | High |
| Purpose incompatibility flags | 2 | High |
| Cross-border transfers with no mechanism | 1 (affects all sessions) | Critical |

---

> **Auditor's note going into Phase 2:** This brief is unusually candid — the author has correctly identified most of the material risks. The problem is not analytical; it is that several findings that should produce a **stop** recommendation are instead framed as things to address in a future version. The audit report will need to sharpen those conclusions.

---

## Phase 2: Personal Data Summary & Role Map 

---

## Phase 2A: Personal Data Summary

| Data category | Source | Purpose(s) | Crosses EU border? | Special category? |
|---|---|---|---|---|
| Gmail sender names and email addresses (third-party data subjects) | Third-party individuals who emailed the authenticated user — data pulled via Gmail API on each session | P1 — fed into LLM as CRM context for Market Research Deck; P3 — used to personalise and target outbound sales email | **Yes** — sent to OpenAI API (USA) on every session. No transfer mechanism documented. | No — but unconstrained content may contain Art. 9-inferable information |
| Gmail message subjects, snippets, and body content (third-party communications) | Same third-party senders — content of their communications pulled without their knowledge | P1 — Market Research Deck generation; P3 — Sales Email personalisation and recipient inference | **Yes** — OpenAI API (USA). No SCCs, no DPF reliance, no TIA in place. | **Potentially yes** ⚠️ — body content is unconstrained; health, financial, religious, or other Art. 9 content may be present. No detection or redaction mechanism exists. |
| Google Drive document text — UXR transcripts, research notes, participant quotes | User's Google Drive, pulled via Drive API v3 — originally collected under a UXR research consent framework | P1 — Market Research Deck; P2 — ICP One Pager generation | **Yes** — OpenAI API (USA). Same transfer gap as above. | **Yes — confirmed latent exposure** ⚠️ — brief explicitly states documents may contain health, ethnicity, and other Art. 9 data. No Art. 9(2) condition assessed or in place. |
| Research participant names, roles, organisations, verbatim quotes | Embedded in Drive documents — originally provided by participants under research consent | P1 and P2 — fed into LLM for commercial output generation | **Yes** — OpenAI API (USA) | No — but indirectly identified individuals whose data is repurposed far beyond original research consent |
| Email recipient identity — name and email address inferred by LLM | Autonomously inferred by GPT-4o-mini from Gmail and Drive content — never provided directly by any party | P3 — sole purpose is to identify and contact this individual with unsolicited commercial email | **Yes** — inferred and processed within OpenAI API (USA); email sent via Gmail (USA) | No — but this individual has the most acute rights deficit in the entire system: no notice, no basis, no opt-out, no human review |
| Authenticated user's Google account email address | Google OAuth2 authentication flow | System authentication and API authorisation | **Yes** — Google infrastructure (USA) | No — low risk; operator identity only |

---

### International Transfer Summary

| Transfer | Recipient | Mechanism documented? | What is needed |
|---|---|---|---|
| Gmail content → OpenAI | OpenAI API (USA) | **No** | Art. 28 DPA + SCCs Module 2 or DPF reliance + TIA |
| Drive document text → OpenAI | OpenAI API (USA) | **No** | Same as above; urgency elevated by confirmed Art. 9 exposure |
| Inferred recipient identity → OpenAI | OpenAI API (USA) | **No** | Same; compounded by absence of any lawful basis for processing this individual's data |
| All outputs → Google Drive / Gmail | Google (USA) | **No — personal account has no DPA** | Google Workspace account + Workspace DPA required; personal Gmail account cannot serve as a compliant processing environment |

---

## Phase 2B: Role Map

| Entity | Role | Processing activity | DPA needed? |
|---|---|---|---|
| **Client** (the organisation or individual deploying the PMM Agent) | **Controller** — determines the purpose (PMM market intelligence, sales outreach) and the means (Gmail + Drive + OpenAI stack) | Authorises the OAuth2 scopes; defines which inputs the agent accepts; owns the output Google Docs and the sent email | N/A — controller |
| **Builder / teammate** (developer of the agent) | **Processor** in build/test phase — processes personal data on the controller's instruction to construct and operate the workflow | Builds and operates the n8n workflow, Gradio interface, and OpenAI integration; has full access to all data flows during development and testing | **Yes — not in place** ⚠️. If the builder is a separate legal entity from the controller, an Art. 28 DPA is required before any personal data is processed during development |
| **OpenAI API** (GPT-4o-mini, web search tool) | **Processor** — processes personal data strictly to fulfil inference requests on the controller's instruction | Receives Gmail content, Drive documents, and inferred recipient identity on every session; generates all three outputs; web search tool retrieves public data about target companies | **Yes — not in place** ⚠️. OpenAI's DPA must be executed via API platform portal. SCCs Module 2 required as transfer mechanism (or DPF reliance if certified). TIA strongly recommended given sensitivity of Gmail and Drive content. |
| **Google** (Gmail API, Drive API, Docs API, Gmail Send) | **Dual role:** Processor for API operations (reading inbox, reading/writing Drive, sending email) / Controller for account-level data under Google's own terms | Reads Gmail inbox (last 10 messages); reads up to 5 Drive documents; writes output Google Docs; sends outbound email via Gmail Send; retains all activity in Google account history | **Yes — not in place for personal account** ⚠️. Google Workspace DPA covers business accounts; personal Gmail accounts have no Art. 28 DPA. This is a structural gap: the entire Google relationship is outside any compliant processing framework until the account is migrated to Workspace. |
| **n8n** (workflow orchestration — Cloud or self-hosted) | **Processor** — routes data between nodes in transit | Receives all data inputs and outputs on each session; no persistent personal data storage by default; if n8n Cloud (EU-hosted), no Chapter V transfer issue | **Yes — check deployment.** n8n GmbH is Berlin-based (EU); n8n Cloud EU instance: DPA available and should be executed. Self-hosted: no third-party processor relationship, but hosting environment must be confirmed as EU. |
| **Gradio / Jupyter** (local interface) | **No independent processor role** — local interface layer only | Receives URL input from user; passes to n8n webhook; no personal data persisted locally | No — local processing only in current configuration |

---

## Phase 3: Clarifying Questions Log

---

### Q1 — Is the authenticated Google account a personal account or a Google Workspace account?

**What I need to know:** The brief states the authenticated account is `olalla.murciego@gmail.com` — a personal Gmail address. I need to confirm whether this is genuinely a personal consumer account or a Workspace account with a custom domain configured as a gmail address.

**Why it matters:** Google Workspace carries a Data Processing Agreement and is covered by Google's GDPR compliance framework for business customers. Personal Gmail accounts are not — they operate under Google's consumer terms of service, which do not constitute an Art. 28 DPA. If this is a personal account, the entire Google relationship (Gmail read, Drive read/write, Gmail Send) is outside any compliant processing framework. Every session involving third-party personal data would be processed through a channel with no controller-processor agreement, no documented transfer mechanism, and no data subject rights obligations on Google's part. This affects Art. 28, Art. 46, and the lawful basis analysis for every purpose in the system.

**Provisional assumption:** This is a personal Gmail account. The system cannot be used with real personal data of third parties until migrated to a Google Workspace account with an executed DPA.

---

### Q2 — What exactly is the LLM inference logic for identifying the sales email recipient?

**What I need to know:** The brief states the recipient is "autonomously inferred by GPT-4o-mini from Gmail and Drive content" but does not describe the mechanism. Does the LLM extract a named contact from a prior email thread? Does it match a company URL to a contact record via web search? Does it select the most recent sender in the inbox? Does it generate a hypothetical contact based on the target company?

**Why it matters:** The inference mechanism determines the legal characterisation of this processing activity. If the LLM is extracting an existing contact from the inbox, it is retrieval of data that was already in the system. If it is generating a new derived identity by combining signals from multiple sources, it is autonomous profiling of an individual who has no relationship with the system — which is the harder Art. 22 scenario and one where no lawful basis under Art. 6 is readily available. The distinction also affects whether the recipient is a data subject whose rights are exercisable against the controller, and on what basis.

**Provisional assumption:** The LLM infers the recipient by extracting or combining identity signals from Gmail and Drive content — treating this as autonomous profiling for the purposes of the audit.

---

### Q3 — What Google Drive documents does the agent actually access, and how are they selected?

**What I need to know:** The brief states the agent reads "up to 5 documents per session" from the user's Google Drive but does not specify how documents are selected — by folder, by recency, by the user manually specifying them, or by the agent searching Drive autonomously. It also states documents "may include UXR interview transcripts" but does not confirm whether such documents are actually present in the Drive being accessed.

**Why it matters:** This affects two separate compliance questions. First, the Art. 9 latent exposure: if UXR transcripts are routinely included in the five documents pulled, Art. 9 risk is not latent — it is live and present in every session. If documents are user-selected and UXR transcripts can be excluded, the risk is controllable by design. Second, the purpose incompatibility analysis: if research participants consented to their data being stored in Drive for research purposes, the scope of that consent matters — were they told the documents could be accessed by automated systems for commercial output generation?

**Provisional assumption:** Documents are selected by recency or automated search, meaning the agent could pull UXR transcripts on any given session. Treating Art. 9 exposure as live rather than latent for the purposes of this audit.

---

### Q4 — Has any privacy notice or transparency communication been issued to any of the third-party data subjects — Gmail senders, UXR research participants, or the sales email recipient?

**What I need to know:** The brief is silent on transparency obligations towards the third-party data subjects whose data is processed most extensively. I need to confirm whether any Art. 13 or Art. 14 notice has been issued or planned for these groups.

**Why it matters:** Art. 14 GDPR requires that where personal data is not obtained directly from the data subject, the controller must provide transparency information within one month of obtaining the data (or at the time of first communication, for the email recipient). Gmail senders, UXR participants, and the email recipient are all Art. 14 subjects. Without any notice mechanism, the controller is in ongoing breach of Art. 14 for every session in which their data is processed. This also directly affects the lawful basis analysis: a legitimate interests basis cannot be validly established without a transparency mechanism that enables data subjects to exercise their right to object.

**Provisional assumption:** No Art. 14 notice has been issued or is planned. The system has no mechanism to identify, locate, or contact Gmail senders or UXR research participants to provide transparency information at scale.

---

### Q5 — Is there any human review step between the LLM generating the sales email and the Gmail Send being executed?

**What I need to know:** The brief states "no human reviews any output before it is saved or sent" but I want to confirm whether there is even a soft confirmation step — for example, a Gradio UI element showing the draft email before send, even if the user is not required to approve it.

**Why it matters:** The Art. 22 analysis hinges on whether there is "meaningful human review" before the consequential action occurs. If even a passive display of the draft email exists before send — one that the user could interrupt — that is architecturally significant: it means the system could be redesigned to require confirmation with minimal engineering effort, which would substantially reduce the Art. 22 exposure and make a proceed-with-conditions recommendation viable. If the send is truly fire-and-forget with no interruption point, the only compliant path is to suspend the autonomous send entirely and rebuild it with a mandatory human approval gate.

**Provisional assumption:** The send is autonomous and uninterrupted. The Art. 22 analysis in the brief supports this reading. Treating the autonomous send as a hard compliance failure in the absence of contradicting evidence.