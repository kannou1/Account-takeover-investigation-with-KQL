# Password-spray detection prototype

Hypothesis: one source produces at least 20 invalid-credential failures against at least 10 distinct accounts within a rolling six-hour lookback. Thresholds are illustrative and unvalidated.

Query 06 uses a fixed evaluation time for replay. Test successive evaluation points across all three spray nights; the guide does not give the first night's exact boundaries. Record the earliest matching time and counts. Do not claim night-one detection until a replay actually matches.

For Sentinel, adapt the lab table to the connected sign-in table and normalize its fields, replace EvaluationTime with now(), run every 15 minutes with a six-hour lookup period, alert on more than zero returned rows, and map IPAddress to an IP entity. Review account entity mapping separately because Accounts is an array. Start with Medium severity and analyst triage. These are proposed settings, not a deployed rule.

Return TimeGenerated (set to the last matching event) for scheduling. Overlapping lookbacks can create repeat alerts; configure grouping/deduplication and verify behaviour. An ADX free-cluster query does not deploy a Sentinel analytics rule.

Expected blind spots: rotating sources, distributed or very slow sprays, failure codes other than 50126, ingestion gaps, and thresholds higher than actual activity. Possible false positives: shared NAT, broken clients, migration/testing, and stale saved passwords. Tune against benign history and a labeled attack window; check ingestion delay and dcount approximation.

Validation record: execution pending; detection latency, recall, false-positive rate, and night-one coverage are unknown.
