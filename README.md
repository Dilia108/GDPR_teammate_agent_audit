# GDPR Teammate Agent Audit

This repository contains a GDPR-focused audit of the **PMM Market Intelligence Agent**, a prototype workflow that combines Gmail, Google Drive, web research, and LLM inference to generate PMM outputs.

## Contents

- [`PMM_agent_brief.md`](./PMM_agent_brief.md) - data processing brief describing the system, data flows, and GDPR context.
- [`Audit_worksheet_PMM_agent.md`](./Audit_worksheet_PMM_agent.md) - working notes and structured review used during the audit.
- [`Audit_report_PMM_agent.md`](./Audit_report_PMM_agent.md) - final peer audit report with findings and recommendations.
- [`Debrief_conversation.md`](./Debrief_conversation.md) - post-audit discussion between reviewer and builder.

## Summary

The audit concludes that the current MVP should not be used with real third-party personal data in its current form. The main concerns are lawful basis, purpose limitation, special-category data handling, processor agreements, international transfers, DPIA requirements, and the autonomous email-sending flow.

## Status

This project appears to be a documentation-first exercise for compliance analysis and review.
