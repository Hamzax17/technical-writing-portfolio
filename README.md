# CloudVault Technical Documentation Portfolio

This repository is a technical writing portfolio. It contains a complete documentation set for CloudVault, a fictional enterprise cloud backup and recovery platform.

> [!IMPORTANT]
> CloudVault is a fictional product. It does not exist, and no feature described here has been built or tested in real software. All screens, error codes, URLs, version numbers and release dates are invented for demonstration.

## Purpose

The portfolio demonstrates how to plan, write, review and maintain customer-facing documentation for an enterprise data protection product. It was prepared for the Cohesity GenAI and Technical Documentation Intern selection process.

The documents are written for IT administrators. They follow one set of product facts, one terminology list and one review process.

## Product overview

CloudVault is a web-based platform. IT administrators use the CloudVault Console to protect file data and to recover it after accidental deletion or system failure.

An administrator registers a **data source**, creates a **backup job**, and assigns the job a **backup vault** and a **schedule**. CloudVault runs the job, stores **restore points** in the vault, and reports each run on the dashboard. When a user loses a file, the administrator selects a restore point and restores the file.

## Documentation library

| Document | Audience | Description |
|---|---|---|
| [Installation Guide](docs/installation-guide.md) | New administrators | Account activation, sign-in, and initial configuration. |
| [User Guide](docs/user-guide.md) | Administrators and backup operators | Daily tasks: jobs, schedules, monitoring, restores, notifications. |
| [Troubleshooting Guide](docs/troubleshooting.md) | Administrators and support staff | Diagnosis and resolution for seven common problems. |
| [Release Notes](docs/release-notes.md) | All users | Three fictional releases: 1.0.0, 1.1.0 and 1.2.0. |
| [Contributing Guide](CONTRIBUTING.md) | Documentation authors | Writing standards, terminology, review checklist and commit conventions. |

The documents describe CloudVault version 1.2.0.

## Product capabilities

CloudVault 1.2.0 includes these capabilities:

- Create and manage backup jobs.
- Back up SMB file shares and NFS exports.
- Run backups on a schedule (hourly, daily, weekly or monthly) or on demand.
- Exclude files from a job by name pattern.
- Store restore points in a backup vault with a configurable retention period.
- Monitor runs on the dashboard and in the run history.
- Restore individual files to the original location or to an alternate location.
- Send email notifications for failed runs, runs with warnings and completed runs.
- Control access with three roles: Administrator, Backup Operator and Viewer.

## Documentation standards

All documents in this repository follow these standards:

- **Plain language.** Short sentences, active voice, no marketing language.
- **Task-based structure.** Each procedure lists its purpose, prerequisites, numbered steps, expected result and warnings.
- **Single source of truth.** Product limits, status names and error codes are defined once and used the same way in every document.
- **Consistent terminology.** The terminology list in [CONTRIBUTING.md](CONTRIBUTING.md#terminology-list) governs all wording.
- **Clear callouts.** NOTE, IMPORTANT and WARNING callouts follow the rules in the contributing guide.
- **Reviewed before merge.** Every change passes the review checklist.

## Tools and technologies

| Tool | Use in this project |
|---|---|
| Markdown (GitHub Flavored) | Source format for all documents. |
| HTML | Inline elements where Markdown has no equivalent, such as collapsible sections and keyboard keys. |
| Git and GitHub | Version control, branching, pull requests and review. |
| Generative AI assistant | First-draft support and consistency checks. See the disclosure below. |
| Command-line link and heading checks | Verification of relative links and heading hierarchy before publishing. |

<details>
<summary>Repository structure</summary>

```text
technical-writing-portfolio/
├── README.md
├── CONTRIBUTING.md
├── docs/
│   ├── installation-guide.md
│   ├── user-guide.md
│   ├── troubleshooting.md
│   └── release-notes.md
└── assets/
    └── screenshots/
```

The `assets/screenshots/` folder is reserved for illustrative interface mockups. Any image added there must be labeled as a mockup.

</details>

## Author and portfolio disclaimer

**Author:** Sayyed Hamja  
**Contact:** hamzaxsaiyed17@gmail.com  
**Profile:** https://github.com/your-username

This portfolio is a writing sample. It is not affiliated with, endorsed by or derived from the documentation of any real company or product.

**AI-assisted drafting.** The first drafts of these documents were produced with a generative AI assistant. The author reviewed, edited and checked every document for accuracy, consistency and clarity. The author is responsible for the final content.

## License

The documentation in this repository is provided for portfolio and evaluation purposes. Contact the author before reusing it.
