# CloudVault Release Notes

**Document status:** Fictional example

> [!IMPORTANT]
> CloudVault is a fictional product. All version numbers, release dates, features and issues in this document are invented examples. No real software was released.

## About these release notes

This document lists the changes in each CloudVault version, newest first. Each entry uses the same format:

- **New features:** Capabilities added in this version.
- **Improvements:** Changes to existing behavior.
- **Bug fixes:** Problems corrected in this version.
- **Known issues:** Problems that exist in this version, with workarounds.

CloudVault uses the version format `MAJOR.MINOR.PATCH`. A minor version adds features. A patch version fixes bugs.

| Version | Release date (fictional) | Summary |
|---|---|---|
| [1.2.0](#version-120) | 12 August 2026 | Warnings status, restore to an alternate location, exclude patterns. |
| [1.1.0](#version-110) | 20 April 2026 | NFS sources, more schedule types, cancel run. |
| [1.0.0](#version-100) | 15 January 2026 | Initial release. |

## Version 1.2.0

**Release date:** 12 August 2026 (fictional)

### New features

- **Completed with Warnings status.** A run that skips one or more files now finishes with this status. The run details list each skipped file and the reason.
- **Run completed with warnings notification.** Notification profiles can send email for runs with the new status.
- **Restore to an alternate location.** You can restore files to any registered data source that has write permission.
- **Conflict options for restores.** When a file already exists at the destination, you can select **Skip**, **Overwrite** or **Keep both**.
- **Exclude patterns.** A job can exclude files by name pattern. A job accepts up to 20 patterns.
- **Required MFA policy.** Administrators can require MFA for all users in **Settings > Security**.

### Improvements

- The **Vault Usage** widget on the dashboard now turns amber when a vault reaches 80% of its capacity limit.
- The restore report now lists the status of each restored item.
- The restore wizard now accepts up to 500 items in one request. The previous limit was 100 items.

### Bug fixes

- Notification emails showed times in UTC instead of the tenant default time zone. Emails now use the tenant default time zone.
- The **Next run** column showed an incorrect time for weekly schedules that use a time zone with daylight saving time. The column now shows the correct time.

### Known issues

| Issue | Workaround |
|---|---|
| The **Job Summary** widget on the dashboard can take up to 5 minutes to reflect a completed run. | Check the run history in **Backup Jobs**. The run history updates immediately. |
| When **Keep both** is selected, the restore report lists the original file name instead of the renamed file name. | Open the destination folder to find files with the `_restored` suffix. |

## Version 1.1.0

**Release date:** 20 April 2026 (fictional)

### New features

- **NFS export sources.** You can register NFS exports as data sources.
- **Hourly and monthly schedules.** Hourly schedules support intervals of 1, 2, 4, 6 and 12 hours. Monthly schedules support days 1 to 28.
- **Cancel run.** You can cancel a run that has the status Queued or Running.
- **Skipped status.** CloudVault now records a **Skipped** run when a scheduled start arrives while the previous run is still active.
- **Test email.** You can send a test email from a notification profile.
- **Download run log.** You can download the log of each run.

### Improvements

- Retention period options now include 90 and 365 days.
- The job list now shows the **Next run** column.
- Run logs are now kept for 90 days. The previous period was 30 days.

### Bug fixes

- The run history filter reset after you navigated to another page. The filter now stays selected during the session.
- The **Test connection** button reported success for an SMB file share that the credential could not read. The test now checks read permission.

### Known issues

| Issue | Workaround |
|---|---|
| A run that skips files finishes with the status **Completed**. The skipped files appear only in the run log. | Download the run log and search for skipped files. Version 1.2.0 resolves this issue. |
| The test email can take up to 2 minutes to send. | Wait 2 minutes before you select **Send test email** again. |

## Version 1.0.0

**Release date:** 15 January 2026 (fictional)

### New features

- **CloudVault Console.** A web interface with a dashboard, left navigation pane and run history.
- **Backup jobs.** Create, edit, pause and resume jobs.
- **SMB file share sources.** Register SMB file shares with a source credential.
- **Backup vaults.** Create vaults with a capacity limit.
- **Schedules.** Daily and weekly schedules, and manual runs with **Run now**.
- **Retention periods.** Options of 7, 14 and 30 days.
- **Restore.** Restore individual files to the original location.
- **Email notifications.** Send email when a run fails.
- **Roles.** Administrator, Backup Operator and Viewer roles.
- **Multi-factor authentication.** Users can set up MFA with an authenticator app.

### Improvements

Not applicable. This is the initial release.

### Bug fixes

Not applicable. This is the initial release.

### Known issues

| Issue | Workaround |
|---|---|
| The run history filter resets when you navigate to another page. | Select the filter again. Version 1.1.0 resolves this issue. |
| **Test connection** can report success for an SMB file share that the credential cannot read. | Run the job and review the result. Version 1.1.0 resolves this issue. |

## Related documentation

- [Installation Guide](installation-guide.md)
- [User Guide](user-guide.md)
- [Troubleshooting Guide](troubleshooting.md)
- [Documentation home](../README.md)
