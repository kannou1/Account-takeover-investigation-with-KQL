# Cloudora: Executive Account Takeover Investigation

**Simulated client engagement | MyFirstHack | Ticket CLD-0001**

A defensive SOC portfolio project investigating suspicious access to a fictional CEO's account using KQL, Entra ID sign-in telemetry, and audit events.

> Status: guide-based case study and executable query pack. The source CSVs were not supplied, so queries have not been executed against the lab. Findings below are attributed to the student guide, not independent observations. No real tenant changes were performed.

## Scenario

Cloudora is a fictional 150-person HR software company in London. Its IT administrator flags a 03:12 Lagos sign-in for CEO Daniel Reeve. The investigation asks whether this is account compromise, how access occurred, what persistence was created, and who else was affected.

## Case summary from the guide

The guide describes a low-volume password spray across many users, successful CEO access, an added authenticator, and a finance-related email-hiding rule. These support an account-takeover and BEC-staging hypothesis. Actual invoice fraud, data theft, and the initial MFA bypass are not established by the supplied material.

| UTC time on 2026-08-10 | Guide-described event | Evidence to collect |
|---|---|---|
| 03:09 and 03:10 | Failed CEO sign-ins from 102.89.44.17 | Query 01 results |
| 03:12 | Successful CEO sign-in from Lagos | Query 01 results |
| 03:14 | Outlook Web access | Query 01 results |
| 03:18 | Authenticator registration labelled Pixel 6 | Query 04 audit Details |
| 03:26 | Azure Portal access | Query 01 results |
| 03:31 | RSS Subscriptions inbox rule | Query 04 audit Details |
| 08:41 | Normal London sign-in | Query 01 results |
| 08:55 | Administrator reports suspicious activity | Scenario narrative |

## Start here

1. Obtain the training files `cloudora_signin_logs.csv` and `cloudora_audit_logs.csv` from the original training provider. They are not included here.
2. Follow [the walkthrough](docs/WALKTHROUGH.md) to ingest the CSVs and run the queries.
3. Record outputs in [the evidence register](evidence/EVIDENCE_REGISTER.md), then update [the report](report/Cloudora_Incident_Report.md).
4. Capture genuine results using [the screenshot checklist](screenshots/README.md).
5. Follow [the publishing instructions](docs/GITHUB_SETUP.md).

## Repository contents

- `queries/`: ten commented KQL files, including detection, near-misses, and second-victim follow-up.
- `docs/`: setup, query explanations, detection tuning, interview preparation, and GitHub steps.
- `report/`: an editable case-study report and a PDF version.
- `evidence/`: a register of pending evidence, with no fabricated results.
- `screenshots/`: instructions for capturing lab evidence; screenshots are pending.

## Technique mapping

| Technique | Scenario basis | Interpretation |
|---|---|---|
| T1110.003 - Password Spraying | Few failures per user across many users | Pattern consistent with spraying; exact tested passwords unknown |
| T1078 - Valid Accounts | Successful suspect access | Credential use; mechanism of initial MFA handling unknown |
| T1098.005 - Device Registration | Added authenticator | Mapping used by the guide; verify audit semantics |
| T1564.008 - Email Hiding Rules | Finance/invoice email-hiding rule | BEC staging; fraud execution not shown |

These mappings follow the provided guide. Geo-location alone does not prove compromise. The identity of the guide's second victim remains unresolved without CSV evidence.

## Tools and validation

Target lab: Azure Data Explorer. Transferable language: KQL used in Microsoft Sentinel. Production Sentinel tables and field structures differ from these flat custom CSV tables and require adaptation. The package was checked for file completeness and report rendering; no ADX execution or live detection testing was possible.

## Sources and attribution

Primary source: *Cloudora - The CEO's Account Was Hacked at 3AM*, MyFirstHack / myfirstcyberjob, student guide, ticket CLD-0001, pp. 1-4. The original guide and datasets are not redistributed in this repository.

Technical references: [Kusto joins](https://learn.microsoft.com/en-us/kusto/query/join-operator), [leftanti](https://learn.microsoft.com/en-us/kusto/query/join-leftanti), [dcount](https://learn.microsoft.com/en-us/kusto/query/dcount-aggregation-function), [Sentinel scheduled rules](https://learn.microsoft.com/en-us/azure/sentinel/scheduled-rules-overview).

## Portfolio wording

Current: "Built a guide-based account-takeover investigation case study with commented KQL, a detection prototype, and an incident-report framework."

After execution and verification: "Investigated a simulated executive account takeover, traced password-spray activity, examined MFA and inbox-rule persistence, scoped affected accounts, and documented evidence and response recommendations."

List this under **Simulated client engagements - MyFirstHack**, never as employment.

## Usage rights

Original project materials are provided for educational use. No redistribution license is asserted for the training provider's guide or data. See `NOTICE.md`.
