---
name: resume
description: "Create, tailor, or manage resumes for job applications. Can generate targeted resumes based on job descriptions."
allowed-tools: Read Edit Write Bash Glob Grep
argument-hint: "[tailor|list] [job id, company, or details...]"
---

# Resume Manager

Manage and tailor resumes in the `resumes/` directory.

## Subcommands

### `tailor` — Create a tailored resume for a specific job

Usage: `/resume tailor <job id, company name, or job details>`

1. **Identify the target job:**
   - If a job ID or company name is given, look it up in `jobs.csv` and read the corresponding `job_description_file` from `job-descriptions/` if available.
   - If raw job details or a description are provided, use those directly.
   - If there's not enough info to tailor against, ask the user for the job description or key requirements.

2. **Find the base resume:**
   - List resumes in `resumes/` and use the most recent one as the base.
   - Read the base resume to understand the user's current experience and skills.

3. **Generate tailoring suggestions:**
   - Compare the job requirements against the base resume.
   - Identify keywords, skills, and experiences to emphasize or reword.
   - Suggest specific bullet point changes, reorderings, or additions.
   - Call out any gaps between the job requirements and the resume.

4. **If the user approves changes**, create a new resume file:
   - Name format: `{focus}-{YYYY-MM-DD}.{ext}` (e.g., `ml-engineer-2026-04-02.docx`)
   - Update the `resume_used` field in `jobs.csv` if this is for an existing application.

### `list` — Show all resumes on file

Usage: `/resume list`

1. List all files in `resumes/` with their names and dates.
2. For each, note which applications in `jobs.csv` reference it.

## Rules

- Follow the resume naming convention from CLAUDE.md: `{focus}-{YYYY-MM-DD}.{ext}`
- Never overwrite existing resumes — always create new versions.
- When reading .docx files, note that the content may need to be extracted via a tool. If the file can't be read directly, inform the user and ask them to provide the content or a text version.
