# CloudVault User Guide

**Product version:** CloudVault 1.2.0  
**Document status:** Fictional example

> [!IMPORTANT]
> CloudVault is a fictional product. The screens, values and behavior in this guide are illustrative.

## About this guide

This guide explains how to use the CloudVault Console to protect and recover file data.

**Audience:** IT administrators and backup operators.

**Before you begin:** Complete the [Installation Guide](installation-guide.md). You need at least one backup vault, one data source and one notification profile.

### Roles used in this guide

| Role | Can do |
|---|---|
| Administrator | All tasks in this guide. |
| Backup Operator | Tasks in this guide except managing notification profiles. |
| Viewer | View the dashboard, jobs, run history and restore points only. |

### Key concepts

| Term | Definition |
|---|---|
| Backup job | A saved definition of what to back up, where to store it, and when to run it. |
| Run | One execution of a backup job. |
| Restore point | A copy of the job's data as it existed when a run completed. |
| Backup vault | The storage location that holds restore points. |
| Data source | A registered SMB file share or NFS export. |
| Retention period | The number of days CloudVault keeps a restore point. |

The first run of a job backs up all selected data. Later runs back up only the files that changed.

## Contents

1. [Navigate the dashboard](#navigate-the-dashboard)
2. [Create a backup job](#create-a-backup-job)
3. [Select data sources](#select-data-sources)
4. [Configure a backup schedule](#configure-a-backup-schedule)
5. [Start a manual backup](#start-a-manual-backup)
6. [Monitor backup job status](#monitor-backup-job-status)
7. [Understand run statuses](#understand-run-statuses)
8. [Restore individual files](#restore-individual-files)
9. [Manage notifications](#manage-notifications)

## Navigate the dashboard

**Purpose:** Find the information and controls you need in the Console.

**Prerequisites:** Any role. An active sign-in session.

1. Sign in to the CloudVault Console.
2. Review the dashboard widgets (see the table below).
3. Use the left navigation pane to open other areas.
4. Select a widget title to open the related list with filters applied.

**Expected result:** You can locate recent run results, upcoming runs and vault usage.

### Dashboard widgets

| Widget | Shows |
|---|---|
| Job Summary (last 24 hours) | The number of runs by status. |
| Recent Activity | The 10 most recent runs and restore requests. |
| Upcoming Runs | The next scheduled runs. |
| Vault Usage | Capacity used in each vault. The indicator turns amber at 80%. |
| Alerts | Failed runs that need attention. |

### Left navigation pane

| Item | Use |
|---|---|
| Dashboard | Review the overall status. |
| Backup Jobs | Create, edit, pause and run jobs. View run history. |
| Restore | Restore files from restore points. |
| Sources | Register and manage data sources. |
| Vaults | Create and manage backup vaults. |
| Notifications | Manage notification profiles. |
| Settings | Manage tenant settings, users and network information. |

> [!NOTE]
> The Job Summary widget can take up to 5 minutes to reflect a completed run. The run history in **Backup Jobs** updates immediately. See the [Release Notes](release-notes.md) for this known issue.

## Create a backup job

**Purpose:** Define what CloudVault backs up, where it stores the data, and when it runs.

**Prerequisites:**

- Administrator or Backup Operator role.
- At least one data source with the status **Connected**.
- At least one backup vault with the status **Ready**.

1. In the left navigation pane, select **Backup Jobs**.
2. Select **Create job**.
3. In **Job name**, enter a name that is 3 to 64 characters long. The name must be unique in the tenant.
4. Optional: In **Description**, enter up to 256 characters.
5. Select **Next**.
6. [Select data sources](#select-data-sources), then select **Next**.
7. In **Backup vault**, select the vault for the restore points.
8. In **Retention period**, select 7, 14, 30, 90 or 365 days. The default is 30 days.
9. Select **Next**.
10. [Configure the schedule](#configure-a-backup-schedule), then select **Next**.
11. On the **Review** page, check the settings.
12. Select **Create job**.

**Expected result:** The job appears in the job list with the state **Active**. If you set a schedule, the **Next run** column shows the date and time.

> [!NOTE]
> Creating a job does not start a run. To start a run immediately, see [Start a manual backup](#start-a-manual-backup).

> [!WARNING]
> A restore point is deleted when its retention period ends. You cannot recover deleted restore points. Select a retention period that meets your organization's recovery requirements.

## Select data sources

**Purpose:** Choose the folders and files that a job backs up.

**Prerequisites:** The job creation wizard is open at the **Sources** step. At least one data source has the status **Connected**.

1. In the **Sources** step, select one or more data sources.
2. For each selected source, select **Choose folders**.
3. Select the folders to include. If you select no folders, CloudVault backs up the entire source path.
4. Optional: Under **Exclude patterns**, enter file name patterns to exclude. Enter one pattern per line. You can enter up to 20 patterns.

   > **Illustrative example:** `*.tmp` excludes all files with the .tmp extension.

5. Select **Next**.

**Expected result:** The **Sources** step lists each selected source with its folder count and exclude patterns.

> [!NOTE]
> A job backs up only the data that the source credential can read. Files that the credential cannot read are skipped, and the run finishes with the status **Completed with Warnings**.

## Configure a backup schedule

**Purpose:** Set when CloudVault runs the job automatically.

**Prerequisites:** The job creation wizard is open at the **Schedule** step, or you are editing an existing job.

1. In **Schedule type**, select one option:
   - **Manual only**: The job runs only when you start it.
   - **Hourly**: Select an interval of 1, 2, 4, 6 or 12 hours.
   - **Daily**: Select a start time.
   - **Weekly**: Select one or more days and a start time.
   - **Monthly**: Select a day from 1 to 28 and a start time.
2. In **Time zone**, confirm or change the time zone. The default is the tenant default time zone.
3. Select **Next**, or select **Save** if you are editing a job.

**Expected result:** The **Next run** value shows the next scheduled start in the selected time zone.

> [!NOTE]
> If a run is still Queued or Running when the next scheduled start time arrives, CloudVault does not start a second run. It records the missed start as a run with the status **Skipped**.

> [!NOTE]
> A run does not start at the scheduled time if the job state is **Paused**. A run can also wait in the queue. CloudVault runs up to 5 runs at the same time in a tenant.

## Start a manual backup

**Purpose:** Run a job immediately, outside its schedule.

**Prerequisites:**

- Administrator or Backup Operator role.
- The job state is **Active**. **Run now** is unavailable for a **Paused** job.

1. In the left navigation pane, select **Backup Jobs**.
2. Select the job name.
3. Select **Run now**.
4. In the confirmation dialog, select **Run**.

**Expected result:** A new run appears at the top of the run history. The status is **Queued**, then **Running** when the run starts.

> [!NOTE]
> A manual run does not change the schedule. The next scheduled run still starts at its scheduled time.

### Cancel a run

1. Open the job.
2. In the run history, select the run.
3. Select **Cancel run**.

The run status changes to **Canceled**. **Cancel run** is available only while the status is **Queued** or **Running**. CloudVault keeps the data that the run already backed up, but does not create a restore point for a canceled run.

## Monitor backup job status

**Purpose:** Check whether jobs run as expected and find runs that need attention.

**Prerequisites:** Any role.

1. In the left navigation pane, select **Backup Jobs**.
2. Review the **Last run status** and **Next run** columns for each job.
3. To filter the list, select **Filter**, then select a status or a job state.
4. Select a job name to open the run history.
5. Select a run to open the run details.

The run details page shows:

- Start time, end time and duration.
- Amount of data backed up.
- Number of files backed up and skipped.
- Error code and message, if the run failed.
- Warnings, if the run completed with warnings.

6. To save the run log, select **Download run log**.

**Expected result:** You can identify each run's status, duration and any error code.

> [!NOTE]
> CloudVault keeps run logs for 90 days.

## Understand run statuses

Each run has one status. Each job has one state.

### Run statuses

| Status | Meaning | Action |
|---|---|---|
| Queued | The run is waiting for a free slot. | None. The run starts when a slot is available. |
| Running | The run is backing up data. | None. You can cancel the run. |
| Completed | The run finished and backed up all selected data. | None. |
| Completed with Warnings | The run finished, but CloudVault skipped one or more files. | Open the run details and review the warnings. |
| Failed | The run stopped because of an error. | Note the error code. See the [Troubleshooting Guide](troubleshooting.md#1-backup-job-failed). |
| Canceled | A user canceled the run. | Start a new run if you still need a restore point. |
| Skipped | The scheduled start was missed because the previous run was still active. | If this occurs often, increase the interval. See [Backup is taking longer than expected](troubleshooting.md#2-backup-is-taking-longer-than-expected). |

### Job states

| State | Meaning |
|---|---|
| Active | The job runs on its schedule and accepts manual runs. |
| Paused | The job does not run on its schedule and rejects manual runs. Existing restore points remain available. |

To pause or resume a job, open the job and select **Pause job** or **Resume job**.

## Restore individual files

**Purpose:** Recover files that users deleted or that a failure damaged.

**Prerequisites:**

- Administrator or Backup Operator role.
- At least one restore point that has not expired.
- For a restore to the original location, write permission for the source credential on the data source.
- For a restore to an alternate location, a registered data source with write permission.

1. In the left navigation pane, select **Restore**.
2. Select the backup job that protected the files.
3. Select a restore point. Restore points are listed by date and time.
4. Browse the folders, or enter a file name in **Search**.
5. Select the files or folders to restore. You can select up to 500 items. A folder counts as one item.
6. Select **Next**.
7. In **Destination**, select one option:
   - **Original location**: CloudVault writes each file to its original path.
   - **Alternate location**: Select a registered data source and enter a folder path.
8. In **If a file already exists**, select one option:
   - **Skip**: Keep the existing file.
   - **Overwrite**: Replace the existing file.
   - **Keep both**: Restore the file with the suffix `_restored` added to the name.
9. Select **Start restore**.
10. Open the restore request and wait for the status **Completed**.

**Expected result:** The restore request status is **Completed**. The restored files exist at the destination. The restore report lists each restored item.

> [!WARNING]
> **Overwrite** replaces the current version of a file with the version in the restore point. CloudVault cannot undo this action. If you are not sure, restore to an alternate location first.

> [!NOTE]
> A restore request has its own status: **Queued**, **Running**, **Completed** or **Failed**. If a restore request fails, see [File restoration failed](troubleshooting.md#4-file-restoration-failed).

## Manage notifications

**Purpose:** Control who receives email about backup events.

**Prerequisites:** Administrator role.

### Edit a notification profile

1. In the left navigation pane, select **Notifications**.
2. Select the profile name.
3. To add a recipient, enter an email address in **Recipients**. A profile holds up to 20 recipients.
4. To remove a recipient, select the **x** next to the address.
5. Under **Send email when**, select or clear these events:
   - **Run failed**
   - **Run completed with warnings**
   - **Run completed**
6. Select **Send test email**.
7. Select **Save**.

**Expected result:** The Console displays "Profile saved". Each recipient receives the test email within 10 minutes.

### Assign a profile to a job

1. Open the job.
2. Select **Edit**.
3. In **Notification profile**, select a profile.
4. Select **Save**.

**Expected result:** CloudVault sends email about this job's runs to the profile recipients, for the selected events.

### Turn off a profile

1. Open the profile.
2. Set the status to **Disabled**.
3. Select **Save**.

**Expected result:** CloudVault stops sending email for every job that uses the profile.

> [!IMPORTANT]
> Keep **Run failed** selected for every production job. Without it, nobody receives email when a backup fails.

> [!NOTE]
> Notification emails show times in the tenant default time zone. Delivery can take up to 10 minutes.

## Related documentation

- [Installation Guide](installation-guide.md)
- [Troubleshooting Guide](troubleshooting.md)
- [Release Notes](release-notes.md)
- [Documentation home](../README.md)
