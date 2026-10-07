# Lab walkthrough and query explanations

## 1. Prepare the lab

The guide recommends Azure Data Explorer at https://dataexplorer.azure.com. Availability, free-cluster eligibility, and screens may change; follow the account's current UI. Create a database and ingest each CSV as a separate table: `CloudoraSignIn_CL` and `CloudoraAudit_CL`.

Set `TimeGenerated` to datetime, `ResultType` to string, and IP/account fields to string. The guide expects approximately 1,479 sign-in rows; treat that as a guide expectation, not a validated count. Run each statement in query 00 independently and inspect errors or missing columns before continuing. `tostring(ResultType)` in this pack normalizes numeric/string ingestion, but does not repair other schema errors.

Required sign-in fields: TimeGenerated, UserPrincipalName, AppDisplayName, IPAddress, City, Country, ResultType, ResultDescription. Required audit fields: TimeGenerated, ActivityDisplayName, TargetUser, IPAddress, Details. Device field names are unknown until you inspect the CSV schema.

All timestamps are interpreted as UTC following the guide's reporting convention. Verify that assumption in the source data. London local time in August differs from UTC.

## 2. Triage: query 01

`let` names the date boundaries. `where` filters the CEO and incident day. `=~` performs a case-insensitive string comparison. `project` selects useful columns and `order by` constructs a timeline. The exclusive end avoids adding the following day's midnight.

Look for failures preceding success and later application access. Save the raw rows. Ask the user/IT administrator about travel and VPN use; never infer maliciousness solely from Lagos.

## 3. Establish normal activity: query 02

`summarize` groups each account's activity by country, city, and IP. `count`, `min`, and `max` show volume and first/last observation. Compare the CEO with Omar. The guide characterizes Omar's daytime Dubai activity across three days, usual iOS device, and successful first attempts as travel-consistent. Verify the device field from the actual schema and confirm travel before closing it as benign.

The eight-day baseline is short. First-seen in these logs does not mean never seen before.

## 4. Find the precursor: query 03

The first statement finds invalid-credential failures (`50126`) and distinct targeted users per source. `dcount` is an estimate. The second statement exposes attempts per user per day using `bin(TimeGenerated, 1d)`.

Few attempts per account across many accounts are consistent with password spraying. They do not prove which password was tried or that the spray directly obtained the CEO's credentials. Inspect all result codes too; a filter for 50126 omits other failures.

## 5. Persistence: query 04

Read audit events and raw Details. Verify target, actor if available, operation success, authenticator metadata, rule conditions, and action. A registered method and an email-hiding rule require separate cleanup; changing only a password does not remove them.

The known IP list comes from the guide. Query 09 expands discovery using the prefix, but requires corroboration before adding addresses to that list. Never block an entire prefix based on this exercise.

## 6. Scope: queries 05, 07, 08

Query 05 enumerates successful access from known suspect addresses. Investigate every account rather than assuming it is already compromised. The guide promises a second victim but does not name it.

Query 07 uses `leftanti` to keep users with known-IP failures and remove users with known-IP successes. Lowercasing makes account matching consistent. It produces observed unsuccessful targets, not a guarantee they were never breached.

Replace query 08's explicit placeholder with the second account from query 05. Review its full sign-in history and audit events, including events from other IPs. Add its own baseline analysis by changing the user filter in query 02.

## 7. Detect: query 06

See DETECTION.md. Historical dates are required for old lab data: `ago(6h)` today would produce no August results. Run each query separately rather than pasting all files into one batch.

## 8. Report and verify

For each conclusion, record query filename, UTC range, matching row identifiers if available, export filename, and screenshot. Distinguish guide statements, observations, interpretations, and recommendations. Update the report only after reviewing outputs.

No tenant access is part of this project. In a real authorized incident, coordinate containment with administrators, preserve evidence, then verify across sign-ins, authentication methods, mail rules, and broader account activity. Rerunning only known-IP searches cannot prove eradication.
