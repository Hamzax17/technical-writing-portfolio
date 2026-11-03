# Contributing to the CloudVault Documentation

This guide defines how to write, review and submit changes to the documentation in this repository. It also records the standards that keep the documents consistent.

> [!IMPORTANT]
> CloudVault is a fictional product. Do not describe it as a real product, and do not claim that any feature was tested in real software.

## Contents

- [Contribution workflow](#contribution-workflow)
- [Writing standards](#writing-standards)
- [Markdown formatting conventions](#markdown-formatting-conventions)
- [Heading hierarchy](#heading-hierarchy)
- [Terminology list](#terminology-list)
- [Link conventions](#link-conventions)
- [Accuracy and technical verification](#accuracy-and-technical-verification)
- [AI-assisted drafting](#ai-assisted-drafting)
- [Review checklist](#review-checklist)
- [Git commit message conventions](#git-commit-message-conventions)

## Contribution workflow

1. Open an issue that describes the change. State the document, the section and the reason.
2. Create a branch from `main`. Use the naming pattern in the table below.
3. Make the change. Follow this guide.
4. Run the checks in the [review checklist](#review-checklist).
5. Commit the change. Follow the [commit message conventions](#git-commit-message-conventions).
6. Open a pull request. Link the issue and summarize the change.
7. Address review comments with new commits.
8. A reviewer approves the pull request. The reviewer squash-merges it into `main`.

| Change type | Branch name | Example |
|---|---|---|
| New content | `docs/add-<topic>` | `docs/add-restore-conflict-options` |
| Correction | `fix/<topic>` | `fix/schedule-time-zone-note` |
| Formatting | `style/<topic>` | `style/heading-levels` |

Do not commit directly to `main`.

## Writing standards

Apply these rules to all text.

1. Write short sentences. Aim for 25 words or fewer.
2. Use active voice. Write "Select **Save**", not "**Save** should be selected".
3. Address the reader as "you".
4. Use present tense for product behavior.
5. Put one idea in each sentence.
6. Remove marketing language and unsupported claims. Do not write "powerful", "seamless" or "best-in-class".
7. Put prerequisites before the steps that need them.
8. Describe the expected result after each important procedure.
9. Use a numbered list for sequential steps. Use bullets for items without order.
10. Start each step with a verb. Put one action in each step.
11. Write interface names exactly as they appear, in bold.
12. Write file paths, values and error codes in `code` format.
13. Mark every example as **Illustrative example**.

### Procedure template

Use this structure for every procedure.

```markdown
### Procedure title

**Purpose:** One sentence.

**Prerequisites:**

- Required role.
- Required objects or settings.

1. First step.
2. Second step.

**Expected result:** What the reader sees when the procedure succeeds.
```

### Callouts

Use GitHub callout syntax. Use each type only for the purpose in the table.

| Callout | Use for | Example |
|---|---|---|
| `> [!NOTE]` | Additional information that helps the reader. | A time limit or a default value. |
| `> [!IMPORTANT]` | Information the reader must know to succeed. | A required setting. |
| `> [!WARNING]` | Actions that can cause data loss or a security risk. | Overwriting files. |

Limit each section to two callouts. Do not use other callout types.

## Markdown formatting conventions

- Use GitHub Flavored Markdown.
- Use `#` headings. Do not use underline-style headings.
- Leave one blank line before and after headings, lists, tables, code blocks and callouts.
- Use `-` for bullet lists.
- Use `1.` style numbering. Restart numbering for each procedure.
- Indent nested list items by 3 spaces under numbered lists, and by 2 spaces under bullets.
- Use pipe tables with a header row. Keep table cells short. Do not put procedures in tables.
- Use fenced code blocks with a language tag. Use `text` when no language applies.
- Use **bold** for interface elements. Use *italics* only for a term when you define it.
- Wrap lines at the end of a sentence or paragraph. Do not insert manual line breaks, except two trailing spaces in the document header.
- Save files with UTF-8 encoding, LF line endings and one final newline.
- Use lowercase, hyphenated file names, such as `user-guide.md`.

### HTML in Markdown

Use HTML only when Markdown has no equivalent. Allowed elements:

| Element | Use |
|---|---|
| `<details>` and `<summary>` | Collapsible optional content, such as a long list. |
| `<kbd>` | Keyboard keys. |
| `<br>` | Not allowed. Use a new paragraph. |

Leave a blank line after `<summary>` so that Markdown renders inside `<details>`.

## Heading hierarchy

- Each file has one `#` heading (H1), which is the document title.
- Use `##` for major sections and `###` for subsections. Use `####` only when needed.
- Do not skip levels. A `###` heading must follow a `##` or `###` heading.
- Write headings in sentence case.
- Name procedure headings with a verb phrase, such as "Create a backup job".
- Keep headings unique within a file, so anchor links work. The only exception is the fixed subsection headings that a template requires under each item, such as "Symptoms" in the troubleshooting guide and "Known issues" in the release notes. Do not link to those headings. Link to the parent heading, which carries a number or a version.
- Do not end headings with a period or a colon.

## Terminology list

Use these terms exactly. Do not use the listed alternatives.

| Use | Definition | Do not use |
|---|---|---|
| CloudVault Console | The CloudVault web interface. | portal, UI, web app, dashboard (for the whole interface) |
| Dashboard | The Console home page with status widgets. | home screen, overview page |
| tenant | An organization's CloudVault environment. | account (for the environment), instance |
| Administrator | The role with all permissions. | admin (in running text), superuser |
| Backup Operator | The role that manages jobs and restores files. | operator |
| Viewer | The read-only role. | read-only user |
| data source | A registered SMB file share or NFS export. | source system, repository, target |
| source credential | The service account that CloudVault uses to access an SMB file share. | login, access key |
| backup job | The saved definition of what, where and when to back up. | task, policy, plan |
| run | One execution of a backup job. | session, instance, execution |
| manual backup | A run that a user starts with **Run now**. | on-demand backup, ad hoc backup |
| schedule | The rule that starts runs automatically. | timer, trigger |
| backup vault | The storage location for restore points. | repository, bucket, storage pool |
| capacity limit | The maximum storage that a vault can use. | quota, size limit |
| restore point | A copy of job data as of a completed run. | snapshot, backup copy, version |
| retention period | The number of days CloudVault keeps a restore point. | expiry, TTL |
| restore request | One restore operation that a user starts. | restore job |
| notification profile | A set of recipients and events for email. | alert rule, subscription |
| exclude pattern | A file name pattern that a job skips. | filter, ignore rule |
| error code | A code such as CV-1001 in the run or restore details. | error number, fault ID |

### Status names

Write status names exactly as shown, in bold.

| Object | Values |
|---|---|
| Run | **Queued**, **Running**, **Completed**, **Completed with Warnings**, **Failed**, **Canceled**, **Skipped** |
| Job state | **Active**, **Paused** |
| Restore request | **Queued**, **Running**, **Completed**, **Failed** |
| Data source | **Connected** |
| Backup vault | **Ready** |
| Notification profile | **Enabled**, **Disabled** |
| User | **Invited**, **Active** |

### Product facts

All documents must use these facts. Change them in every document at the same time.

| Fact | Value |
|---|---|
| Current version | 1.2.0 |
| Concurrent runs per tenant | 5 |
| Job name length | 3 to 64 characters, unique in the tenant |
| Retention options | 7, 14, 30, 90 and 365 days. Default: 30 days |
| Exclude patterns per job | 20 |
| Recipients per notification profile | 20 |
| Items per restore request | 500 |
| Run log retention | 90 days |
| Session timeout | 30 minutes of inactivity |
| Account lockout | 15 minutes after 5 failed attempts |
| Activation link validity | 72 hours |
| Password reset link validity | 24 hours |
| Email delivery time | Up to 10 minutes |
| Error codes | CV-1001 to CV-1004, CV-2001 to CV-2003 |

## Link conventions

- Use relative links for files in this repository. Do not use absolute GitHub URLs.
- Write links from the file's own location:
  - From `docs/` to another document in `docs/`: `user-guide.md`
  - From `docs/` to the root: `../README.md`
  - From the root to `docs/`: `docs/user-guide.md`
- Use descriptive link text. Write "see the User Guide" as the link text, not "click here".
- Match link text to the target document title.
- Link to a section with a lowercase, hyphenated anchor, such as `#create-a-backup-job`.
- Do not link to external websites for product facts. CloudVault is fictional.
- Reserve the `example` domain for URLs, such as `cloudvault.example`. This domain cannot belong to a real organization.
- Check every link after you rename a file or heading.

## Accuracy and technical verification

Documentation must be accurate and consistent.

1. Verify each statement against the [product facts](#product-facts) and the other documents.
2. Do not add a feature, limit or behavior to one document without updating all affected documents.
3. Do not write real commands, package names or installation steps for CloudVault. It is a fictional web application.
4. Label sample values as **Illustrative example**.
5. Label release information as fictional.
6. State only what the product does. Do not speculate.
7. For each change, record how you verified it in the pull request description.
8. When you cannot verify a statement, remove it or ask the reviewer.

> [!WARNING]
> Do not copy text from the documentation of a real product. Write original content.

## AI-assisted drafting

You can use a generative AI assistant for first drafts, rewording and consistency checks. The author remains responsible for the final text.

1. Give the assistant the terminology list and product facts as input.
2. Treat all output as an unverified draft.
3. Check every statement against the product facts. AI-generated text can introduce features and limits that do not exist.
4. Rewrite text that sounds generic, repetitive or promotional.
5. Do not paste confidential or personal information into an AI tool.
6. Disclose AI assistance in the README.

## Review checklist

Copy this checklist into the pull request description. Check each item before you request a review.

**Language**

- [ ] Grammar, spelling and punctuation are correct.
- [ ] Sentences are short and use active voice.
- [ ] Text contains no marketing language or unsupported claims.

**Structure**

- [ ] The file has one H1 heading, and no heading level is skipped.
- [ ] Headings are in sentence case and unique within the file.
- [ ] Sequential steps use numbered lists. Other lists use bullets.
- [ ] Each procedure includes a purpose, prerequisites, steps and an expected result.
- [ ] Callouts use the correct type.

**Consistency**

- [ ] Terms match the [terminology list](#terminology-list).
- [ ] Status names and product facts match the tables in this guide.
- [ ] The change does not contradict another document.
- [ ] Information is not duplicated. Link to the source instead.

**Links and files**

- [ ] All relative links work from the file's location.
- [ ] All anchor links match an existing heading.
- [ ] File names are lowercase and hyphenated.

**Accuracy**

- [ ] Illustrative examples are labeled.
- [ ] Release information is labeled as fictional.
- [ ] The pull request describes how you verified the change.

## Git commit message conventions

Write each commit message in this format:

```text
<type>(<scope>): <summary>

<body: what changed and why>
```

**Rules**

- Write the summary in the imperative mood: "Add", "Fix", "Update", not "Added".
- Limit the summary to 72 characters.
- Do not end the summary with a period.
- Use the body to explain why, when the reason is not obvious.
- Make each commit one logical change.

**Types**

| Type | Use |
|---|---|
| `docs` | New or changed documentation content. |
| `fix` | Correction of an error in the documentation. |
| `style` | Formatting changes that do not change meaning. |
| `refactor` | Reorganization of content without changing meaning. |
| `chore` | Repository maintenance, such as moving files. |

**Scopes:** `readme`, `install`, `user-guide`, `troubleshooting`, `release-notes`, `contributing`.

**Examples**

```text
docs(user-guide): add restore conflict options procedure
fix(troubleshooting): correct lockout duration to 15 minutes
style(contributing): fix heading levels in terminology list
```
