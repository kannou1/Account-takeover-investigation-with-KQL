# Explain the project in an interview

Current honest introduction: "I built a guide-based SOC investigation for a fictional executive account takeover. It contains commented KQL for triage, baseline comparison, spray analysis, audit persistence, victim scoping, and a detection prototype. I still need the training CSVs to validate the results."

After completing the lab, describe your actual findings and evidence rather than memorizing the answer key.

- Why not alert only on country? VPNs and travel can change location; combine timing, failures, device history, and audit changes.
- Spray versus brute force? Spray distributes a small number of guesses across many users; brute force commonly concentrates guesses on one account. Logs support patterns, not visibility of tested passwords.
- Why is password reset insufficient? Unauthorized authentication methods, mail rules, and some sessions/application access require separate response and verification.
- How do you scope? Find successful known-source access, investigate each user, enumerate failed targets, and expand infrastructure based on corroboration.
- Why leftanti? It returns failed-target accounts absent from the successful-account set for the selected infrastructure and dataset.
- Why historical replay? A query using now/ago on old logs misses the training period. Fix the evaluation time and test multiple windows.
- What remains unknown? Second victim, exact counts, initial MFA handling, exfiltration, executed fraud, and whether the proposed detection catches night one.
