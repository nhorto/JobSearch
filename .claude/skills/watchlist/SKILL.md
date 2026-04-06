---
name: watchlist
description: "Manage a queue of jobs and companies to apply to later. Track interesting opportunities and target companies you haven't applied to yet."
allowed-tools: Read Edit Write Bash Glob Grep WebFetch
argument-hint: "[add|remove|list|apply] [details...]"
---

# Job Watchlist

Manage a queue of future opportunities in `watchlist.csv`. This tracks two kinds of entries:

- **Jobs**: Specific openings you want to apply to when you have time.
- **Companies**: Companies you're interested in that you want to check for openings later.

## Watchlist CSV Schema

File: `watchlist.csv` in the project root. Create it if it doesn't exist, with this header:

```
id,type,company,job_title,location,source_url,priority,date_added,notes
```

| Column | Description | Values |
|--------|-------------|--------|
| `id` | Auto-incrementing integer | `1`, `2`, `3`... |
| `type` | Entry type | `job` or `company` |
| `company` | Company name | Free text |
| `job_title` | Specific job title (empty for company-type entries) | Free text or empty |
| `location` | Location if known | `Remote`, `City, State`, etc. or empty |
| `source_url` | URL to the posting or company careers page | URL or empty |
| `priority` | How interested you are | `high`, `medium`, `low` |
| `date_added` | When you added this to the watchlist | `YYYY-MM-DD` |
| `notes` | Why you're interested, deadline info, etc. | Free text |

## Subcommands

### `add` — Add a job or company to the watchlist

Usage: `/watchlist add <url, company name, or job details>`

1. **If a URL is provided**, try to fetch it and extract job/company details.
2. **If a company name is provided**, create a `company`-type entry.
3. **If job details are provided**, create a `job`-type entry.
4. Ask for priority if not obvious (default to `medium`).
5. Set `date_added` to today's date.
6. Append to `watchlist.csv` (create the file with headers if it doesn't exist).
7. Confirm what was added.

**Bulk add**: If the user provides a list of companies or jobs (e.g., from a report), parse them all and add each as a separate entry. Show a summary of everything added and ask for confirmation before writing.

### `remove` — Remove an entry from the watchlist

Usage: `/watchlist remove <id or company name>`

1. Find the matching entry by ID or company name (case-insensitive partial match).
2. If multiple matches, show them and ask the user to pick.
3. Remove the row from `watchlist.csv`.
4. Confirm the removal.

### `list` — View the watchlist

Usage: `/watchlist list [filter]`

1. Read `watchlist.csv` and display entries.
2. Optional filters: `jobs`, `companies`, `high` (priority), or a search term.
3. Sort by priority (high first), then by date added (newest first).
4. Flag any `job`-type entries older than 14 days — the posting may have expired.
5. Show the total count.

### `apply` — Move a watchlist item to an active application

Usage: `/watchlist apply <id or company name>`

1. Find the watchlist entry.
2. Invoke the jobsearch `add` workflow to create a new row in `jobs.csv` using the watchlist entry's details.
3. Remove the entry from `watchlist.csv`.
4. Confirm the transition.

## Rules

- Follow all date and CSV conventions from CLAUDE.md.
- Always quote CSV fields that contain commas.
- For bulk adds, always show the full list and get confirmation before writing.
- IDs in watchlist.csv are independent from jobs.csv IDs.
