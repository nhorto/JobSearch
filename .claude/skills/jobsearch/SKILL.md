---
name: jobsearch
description: "Manage job applications: add new applications, update status, and view summary stats for the job search tracker."
allowed-tools: Read Edit Write Bash Glob Grep WebFetch
argument-hint: "[add|update|summary] [details...]"
---

# Job Search Tracker

Manage job applications in `jobs.csv`. This skill supports three subcommands based on the argument provided.

## Subcommands

### `add` — Add a new application

Usage: `/jobsearch add <url or job details>`

1. **If a URL is provided**, fetch the page with WebFetch and extract: job title, company, location, and any other relevant details.
2. **If text/details are provided instead of a URL**, parse them directly.
3. **If no details are provided**, ask the user for the job title, company, and location at minimum.
4. Determine the next ID by reading `jobs.csv`, finding the highest `id`, and adding 1.
5. Set `date_applied` to today's date (YYYY-MM-DD format).
6. Set `status` to `applied`.
7. Ask the user which resume to use. Show available resumes from the `resumes/` directory. Default to the most recently dated resume if the user doesn't specify.
8. If a job description was fetched or provided, save it to `job-descriptions/{company}-{short-title}.md` (lowercase, hyphenated) and set `job_description_file`.
9. Append the new row to `jobs.csv`. Always quote any field that contains commas.
10. Confirm what was added by showing the key details.

### `update` — Update application status

Usage: `/jobsearch update <company or id> <new status> [notes]`

1. Find the matching row in `jobs.csv` by company name (case-insensitive partial match) or by ID.
2. If multiple matches are found, show them and ask the user to clarify.
3. Valid statuses: `applied`, `screening`, `interviewing`, `offer`, `rejected`, `withdrawn`, `no_response`.
4. Update the `status` column.
5. Append context to the `notes` column (e.g., "Status changed to rejected 2026-04-02. <user's note>"). Preserve any existing notes.
6. Confirm the update by showing the before and after.

### `summary` — View application stats

Usage: `/jobsearch summary`

1. Read `jobs.csv` and compute:
   - Total applications
   - Breakdown by status (count and percentage)
   - Applications by week
   - Response rate (any status other than `applied` or `no_response` / total)
2. Display as a clean formatted table/summary.

## Rules

- Follow all conventions from CLAUDE.md (date format, naming, CSV safety, etc.).
- Never delete rows. Use `withdrawn` status instead.
- Never reuse IDs.
- Always quote CSV fields that contain commas.
- When fetching URLs, gracefully handle failures — ask the user to paste the details manually.
