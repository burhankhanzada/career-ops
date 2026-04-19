# Mode: tracker — Application Tracker

Reads and displays `data/applications.md`.

**Tracker format:**
```markdown
| # | Date | Company | Role | Score | Status | PDF | Report |
```

Possible statuses: `Evaluated` → `Applied` → `Responded` → `Outreach` → `Interview` → `Offer` / `Rejected` / `Discarded` / `DO NOT APPLY`

- `Applied` = the candidate submitted their application.
- `Responded` = A recruiter/company contacted the candidate and they replied (inbound).
- `Outreach` = The candidate proactively contacted someone at the company (outbound, e.g., LinkedIn power move).

If the user asks to update a status, edit the corresponding row.

Also show statistics:
- Total applications
- Breakdown by status
- Average score
- % with PDF generated
- % with report generated
