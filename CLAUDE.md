# Job Search Tracker

Personal job application tracking system.

## Project Structure

```
JobSearch/
  jobs.csv                    # Master tracking spreadsheet
  watchlist.csv               # Jobs/companies to apply to later
  jobs-applied-raw-backup.csv # Original unmodified CSV backup
  resumes/                    # All resume versions used for applications
  job-descriptions/           # Saved job postings (text files)
  .claude/skills/             # Project-local skills
```

## jobs.csv Schema

| Column | Description | Values |
|--------|-------------|--------|
| `id` | Auto-incrementing integer | `1`, `2`, `3`... |
| `job_title` | Job title as posted | Free text |
| `company` | Company name (clean, no typos) | Free text |
| `location` | Standardized: `Remote`, `US Remote`, `Remote (Country)`, `City, State` | Free text |
| `date_applied` | Date application submitted | `YYYY-MM-DD` |
| `status` | Current application status | `applied`, `screening`, `interviewing`, `offer`, `rejected`, `withdrawn`, `no_response` |
| `resume_used` | Filename of resume in `resumes/` | e.g. `ai-engineer-2026-03-28.docx` |
| `job_description_file` | Filename in `job-descriptions/` (if saved) | e.g. `clickup-senior-ai-engineer.md` |
| `source_url` | URL of original job posting | URL or empty |
| `notes` | Freeform notes (interview dates, contacts, etc.) | Free text |

## Conventions

- **Dates**: Always `YYYY-MM-DD` format
- **Status updates**: When updating status, add context to `notes` (e.g. "Rejected via email 2026-04-02")
- **Resumes**: Name format `{focus}-{YYYY-MM-DD}.{ext}` (e.g. `data-scientist-2026-04-01.docx`)
- **Job descriptions**: Name format `{company}-{short-title}.md` (e.g. `clickup-senior-ai-engineer.md`)
- **IDs**: Never reuse. Next ID = highest existing ID + 1
- **CSV safety**: Always quote fields containing commas. Never delete rows - use `withdrawn` status instead.

## Resumes on File

| Filename | Description |
|----------|-------------|
| `ai-engineer-2026-03-28.docx` | AI Engineer & Data Scientist resume (used for initial batch of applications) |

## Skills (project-local, in `.claude/skills/`)

### `/jobsearch` — Application tracker
- `add <url or details>` — Add a new application
- `update <company or id> <status>` — Update application status
- `summary` — View stats and breakdown

### `/resume` — Resume manager
- `tailor <job id, company, or details>` — Generate tailored resume suggestions for a specific job
- `list` — Show all resumes and which applications use them

### `/watchlist` — Future opportunities queue
- `add <url, company, or job details>` — Save a job or company to apply to later (supports bulk add from lists/reports)
- `remove <id or company>` — Remove an entry
- `list [filter]` — View watchlist (filter by `jobs`, `companies`, `high`, or search term)
- `apply <id or company>` — Move a watchlist item into an active application in jobs.csv
