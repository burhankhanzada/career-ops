# Mode: pipeline — URL Inbox (Second Brain)

Processes job offer URLs accumulated in `data/pipeline.md`. The user adds URLs at their convenience and then executes `/career-ops pipeline` to process them all.

## Workflow

1. **Read** `data/pipeline.md` → find `- [ ]` items in the "Pending" section.
2. **For each pending URL**:
   a. Calculate the next sequential `REPORT_NUM` (read `reports/`, find highest number + 1).
   b. **Extract JD** using Playwright (browser_navigate + browser_snapshot) → WebFetch → WebSearch.
   c. If URL is inaccessible → mark as `- [!]` with a note and continue.
   d. **Execute full auto-pipeline**: A-F Evaluation → Report .md → PDF (if score >= 3.0) → Tracker.
   e. **Move from "Pending" to "Processed"**: `- [x] #NNN | URL | Company | Role | Score/5 | PDF ✅/❌`
3. **If 3+ URLs are pending**, launch agents in parallel (Agent tool with `run_in_background`) to maximize speed.
4. **When finished**, display summary table:

```
| # | Company | Role | Score | PDF | Recommended Action |
```

## pipeline.md Format

```markdown
## Pending
- [ ] https://jobs.example.com/posting/123
- [ ] https://boards.greenhouse.io/company/jobs/456 | Company Inc | Senior PM
- [!] https://private.url/job — Error: login required

## Processed
- [x] #143 | https://jobs.example.com/posting/789 | Acme Corp | AI PM | 4.2/5 | PDF ✅
- [x] #144 | https://boards.greenhouse.io/xyz/jobs/012 | BigCo | SA | 2.1/5 | PDF ❌
```

## Intelligent JD Detection from URL

1. **Playwright (preferred):** `browser_navigate` + `browser_snapshot`. Works with all SPAs.
2. **WebFetch (fallback):** For static pages or when Playwright is unavailable.
3. **WebSearch (last resort):** Search secondary portals that index the JD.

**Special Cases:**
- **LinkedIn**: May require login → mark `[!]` and ask the user to paste text.
- **PDF**: If URL points to a PDF, read it directly with Read tool.
- **`local:` prefix**: Read the local file. Example: `local:jds/linkedin-pm-ai.md` → read `jds/linkedin-pm-ai.md`.

## Automatic Numbering

1. List all files in `reports/`.
2. Extract the number from the prefix (e.g., `142-medispend...` → 142).
3. New number = maximum found + 1.

## Source Synchronization

Before processing any URL, verify sync:
```bash
node cv-sync-check.mjs
```
If there is a desync, warn the user before continuing.
