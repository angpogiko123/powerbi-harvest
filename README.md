# Power BI Harvest Tool

`powerbi_harvest.py` is a script for structured exploration and auditing of Power BI workspaces. It retrieves metadata, datasets, and reports, performs access control checks (ACL), and generates human-readable summaries with optional user enumeration and role visibility.

It is intended for educational or experimental purposes. The tool can be used to investigate inconsistencies in permissions, test dataset access boundaries, and collect structured information for further manual review.

## Features

- Lists all accessible workspaces (Power BI groups).
- Extracts reports, associated datasets, and embed token generation attempts.
- Dumps DAX query output for each dataset (if accessible).
- Performs ACL checks on reports and datasets.
- Identifies role-based access (including RLS metadata and enforcement status).
- Enumerates users in each workspace (optional).
- Maps users to workspaces and roles.
- Generates a detailed summary.
- Includes Microsoft Graph lookup (optional) to match group membership against ACLs.

## Requirements

See `requirements.txt`:

```
requests
tabulate
pandas
```

Install with:

```bash
pip install -r requirements.txt
```

## Usage

```bash
python3 powerbi_harvest.py --token <BEARER_TOKEN> [--enum-users] [--audit] [--output OUTPUT_DIR]
```

### Options

- `--token` (required): A Power BI access token (Bearer) obtained from browser or tooling.
- `--enum-users`: List users in each workspace and try to infer your role.
- `--audit`: Enable access control checks and report findings.
- `--output`: Output directory. Defaults to `output/YYYYMMDD_HHMMSS`.

---

## Output

### Terminal Output (Example)

```
[=] Workspace: Finance_Dept
    → Your role: Contributor
    ↪ Report: Quarterly_Summary
    ├─ [✓] Fetched reportId: 7a1df76...
    ├─ ⚙️ Checking embed token...
    ├─ [✓] Embed token generated (HTTP 200)
    ├─ [✓] Sent ExportTo request → jobId: 3a1f...
    ├─ [✓] Export succeeded
    [✔] DAX OK: FinancialsDataset (24 columns)

[=] Workspace: HR_Team
    → Your role: Unknown (not in list)
    ↪ Report: Employee_Stats
    ├─ [✘] Embed token failed → HTTP 403
    [✘] DAX FAIL: StaffData (HTTP 403)

🧑‍💼 Users Summary:
╭────────────────────────────┬────────────────────────────┬──────────────┬──────────╮
│ displayName                │ emailAddress               │ identifier   │ role     │
├────────────────────────────┼────────────────────────────┼──────────────┼──────────┤
│ Alice Admin                │ alice@contoso.com          │ ...          │ Admin    │
│ Bob Viewer                 │ bob@contoso.com            │ ...          │ Viewer   │
╰────────────────────────────┴────────────────────────────┴──────────────┴──────────╯

📌 User → Workspace Mapping (with roles):
╭────────────────────┬──────────────────────────────────────────────╮
│ User               │ Workspaces (Role)                            │
├────────────────────┼──────────────────────────────────────────────┤
│ alice@contoso.com  │ Finance_Dept (Admin), HR_Team (Contributor)  │
│ bob@contoso.com    │ Finance_Dept (Viewer)                        │
╰────────────────────┴──────────────────────────────────────────────╯

[✓] Summary saved to output/20250509_172302/summary.txt
[✓] Full output saved to output/20250509_172302/full_output_summary.txt

🚨 Vulnerabilities Detected: 2
╭─────────────┬─────────────────────────────────────────────────────╮
│ Type        │ Vulnerability                                       │
├─────────────┼─────────────────────────────────────────────────────┤
│ 🔴 VULN     │ Embed token can be generated for: Quarterly_Summary │
│ 🔴 VULN     │ Dataset executed without error or RLS: Financials   │
╰─────────────┴─────────────────────────────────────────────────────╯
```

---

### Example `summary.txt`

```
📊 Workspace: Finance_Dept
  📄 Reports:
    [✔] Quarterly_Summary         → exported
    [✘] Annual_Overview           → failed_403
  🧬 Datasets:
    [✔] FinancialsDataset         → DAX OK, 24 col
    [✘] HR_Dataset                → FAIL (fail_403)

📊 Workspace: HR_Team
  📄 Reports:
    [✘] Employee_Stats            → failed_403
  🧬 Datasets:
    [✘] StaffData                 → FAIL (fail_403)

📋 Enumerated Users:

| displayName | emailAddress       | identifier | role    |
|-------------|--------------------|------------|---------|
| Alice Admin | alice@contoso.com  | ...        | Admin   |
| Bob Viewer  | bob@contoso.com    | ...        | Viewer  |

=== USERS → WORKSPACES MAP ===
╭────────────────────┬──────────────────────────────────────────────╮
│ User               │ Workspaces (Role)                            │
├────────────────────┼──────────────────────────────────────────────┤
│ alice@contoso.com  │ Finance_Dept (Admin), HR_Team (Contributor)  │
│ bob@contoso.com    │ Finance_Dept (Viewer)                        │
╰────────────────────┴──────────────────────────────────────────────╯

=== AUDIT VULNERABILITY SUMMARY ===
╭─────────────┬────────────────────────────────────────────────────╮
│ Type        │ Vulnerability                                      │
├─────────────┼────────────────────────────────────────────────────┤
│ 🔴 VULN     │ Embed token can be generated for: Quarterly_Summary│
│ 🔴 VULN     │ Dataset executed without error or RLS: Financials  │
╰─────────────┴────────────────────────────────────────────────────╯
```

---

### Example `full_output_summary.txt`

```
======================================================================
SUMMARY
======================================================================
📊 Workspace: Finance_Dept
  📄 Reports:
    [✔] Quarterly_Summary         → exported
    [✘] Annual_Overview           → failed_403
  🧬 Datasets:
    [✔] FinancialsDataset         → DAX OK, 24 col
    [✘] HR_Dataset                → FAIL (fail_403)

📊 Workspace: HR_Team
  📄 Reports:
    [✘] Employee_Stats            → failed_403
  🧬 Datasets:
    [✘] StaffData                 → FAIL (fail_403)

📋 Enumerated Users:

| displayName | emailAddress       | identifier | role    |
|-------------|--------------------|------------|---------|
| Alice Admin | alice@contoso.com  | ...        | Admin   |
| Bob Viewer  | bob@contoso.com    | ...        | Viewer  |

=== USERS → WORKSPACES MAP ===
╭────────────────────┬──────────────────────────────────────────────╮
│ User               │ Workspaces (Role)                            │
├────────────────────┼──────────────────────────────────────────────┤
│ alice@contoso.com  │ Finance_Dept (Admin), HR_Team (Contributor)  │
│ bob@contoso.com    │ Finance_Dept (Viewer)                        │
╰────────────────────┴──────────────────────────────────────────────╯

=== AUDIT VULNERABILITY SUMMARY ===
╭─────────────┬────────────────────────────────────────────────────╮
│ Type        │ Vulnerability                                      │
├─────────────┼────────────────────────────────────────────────────┤
│ 🔴 VULN     │ Embed token can be generated for: Quarterly_Summary│
│ 🔴 VULN     │ Dataset executed without error or RLS: Financials  │
╰─────────────┴────────────────────────────────────────────────────╯

======================================================================
REPORT LOGS
======================================================================

📄 Quarterly_Summary.log
--------------------------------------------------
Report Name: Quarterly_Summary
Workspace: Finance_Dept
Group ID: XXXXX
Report ID: RPT-123
Dataset ID: DS-456
EmbedTokenCheck: HTTP 200
EmbedToken: eyJ0eXAi...
Job ID: JOB-789

📄 Employee_Stats.log
--------------------------------------------------
Report Name: Employee_Stats
Workspace: HR_Team
Group ID: YYYYY
Report ID: RPT-999
Dataset ID: DS-000
EmbedTokenCheck: HTTP 403
EmbedTokenError: Access Denied

======================================================================
AUDIT FINDINGS
======================================================================
[OK] Token context → service_principal=False, guest=False, admin=False
[INFO] Embed URL detected: https://app.powerbi.com/reportEmbed?reportId=...
[OK] Token subject explicitly in report ACL: Quarterly_Summary
[VULN] Embed token can be generated for: Quarterly_Summary
[VULN] Dataset executed without error or RLS: FinancialsDataset (cols: 24)
[OK] RLS roles defined for dataset: FinancialsDataset
[OK] RLS enforcement confirmed: FinancialsDataset
```

---

## Output Structure

- `reports/<workspace>/`: Exported report PDFs (if accessible).
- `dax/<workspace>/`: Dataset output in JSON format.
- `logs/<workspace>/`: Detailed logs for each report export.
- `users.csv` / `users.json`: Workspace user listings (if enabled).
- `summary.txt`: Human-readable summary.
- `full_output_summary.txt`: Verbose log and finding dump.

## Notes

- Tokens must be valid for the Power BI REST API. Microsoft Graph access (e.g., AAD group resolution) requires additional scopes but is optional.
- Export and DAX operations do not guarantee access — HTTP errors are logged and reported.

## Disclaimer

This tool is provided "as is", for educational and research purposes only. Do not use it against environments you do not have explicit authorization to assess.
