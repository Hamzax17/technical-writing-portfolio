# CloudVault Troubleshooting Guide

**Product version:** CloudVault 1.2.0  
**Document status:** Fictional example

> [!IMPORTANT]
> CloudVault is a fictional product. The error codes, addresses and behavior in this guide are illustrative.

## About this guide

This guide helps administrators diagnose and resolve common CloudVault problems. Each problem section lists symptoms, possible causes, diagnostic steps, resolution steps, a verification procedure and escalation guidance.

**Audience:** Administrators, backup operators and support staff.

**Prerequisites:** Access to the CloudVault Console. Some resolutions require the Administrator role, or help from your network, mail or identity teams.

## Quick reference

| Issue | Possible cause | Resolution |
|---|---|---|
| [1. Backup job failed](#1-backup-job-failed) | Source unreachable, credential rejected, path missing or vault full. | Match the error code in the run details to [the error code table](#error-codes), then apply the listed fix. |
| [2. Backup is taking longer than expected](#2-backup-is-taking-longer-than-expected) | First full run, large change volume, many small files, or queued runs. | Check the run type and queue position. Adjust the schedule or exclusions. |
| [3. Backup schedule did not execute](#3-backup-schedule-did-not-execute) | Job is paused, schedule type is Manual only, time zone mismatch, or run skipped. | Check the job state, schedule and run history. |
| [4. File restoration failed](#4-file-restoration-failed) | Restore point expired, destination not writable or not found. | Select a valid restore point and destination. |
| [5. Email notification not received](#5-email-notification-not-received) | Profile disabled, event not selected, profile not assigned, or mail filtering. | Check the profile, then send a test email. |
| [6. Dashboard not loading](#6-dashboard-not-loading) | Unsupported browser, blocked network access, or stale browser data. | Check browser support and network access. Clear site data. |
| [7. Authentication failure](#7-authentication-failure) | Wrong password, expired MFA code, locked account or expired session. | Verify credentials and device time. Wait for the lockout to end. |

## Before you begin

Collect this information. You need it for diagnosis and for escalation.

- Tenant URL.
- Job name and run ID, from the run details page.
- Error code and message.
- Start time of the problem, in your local time zone and in UTC.
- The run log. In the run details, select **Download run log**.

## Error codes

| Code | Name | Meaning |
|---|---|---|
| CV-1001 | Source unreachable | CloudVault cannot connect to the data source. |
| CV-1002 | Source credential rejected | The data source rejected the service account. |
| CV-1003 | Vault capacity exceeded | The vault reached 100% of its capacity limit. |
| CV-1004 | Source path not found | The folder in the job no longer exists on the source. |
| CV-2001 | Restore destination not writable | The credential has no write permission at the destination. |
| CV-2002 | Restore point expired | The retention period ended and CloudVault deleted the restore point. |
| CV-2003 | Destination path not found | The destination folder does not exist. |

## 1. Backup job failed

### Symptoms

- The run status is **Failed**.
- The dashboard **Alerts** widget lists the run.
- The run details show an error code that starts with CV-1.

### Possible causes

| Error code | Cause |
|---|---|
| CV-1001 | The source server is offline, or a firewall blocks the CloudVault source addresses. |
| CV-1002 | The service account password changed, expired, or the account is locked. |
| CV-1003 | The vault is full. |
| CV-1004 | Someone renamed, moved or deleted a folder that the job includes. |

### Diagnostic steps

1. Open **Backup Jobs**, select the job, then select the failed run.
2. Note the error code and message.
3. Select **Download run log**. Find the first error entry. Later errors are often side effects.
4. Open **Sources**, select the source, then select **Test connection**.
5. For CV-1003, open **Vaults** and check the usage of the vault.

### Resolution steps

**CV-1001**

1. Confirm that the source server is online.
2. Open **Settings > Network** and copy the CloudVault source addresses.
3. Ask your network team to allow these addresses to reach the source.
4. Select **Test connection** on the source.

**CV-1002**

1. Confirm that the service account is enabled and the password has not expired.
2. Open **Sources**, select the source, then enter the current credential.
3. Select **Test connection**, then select **Save**.

**CV-1003**

1. Open **Vaults** and select the vault.
2. Increase the capacity limit, or reduce the retention period of the jobs that use the vault.
3. Save the changes.

> [!NOTE]
> A shorter retention period frees space only after CloudVault deletes the expired restore points.

**CV-1004**

1. Open the job and select **Edit**.
2. In **Sources**, select **Choose folders**.
3. Clear the folder that no longer exists, or select the new folder path.
4. Select **Save**.

### Verification

1. Open the job and select **Run now**.
2. Confirm that the run status changes to **Completed**.
3. Confirm that a new restore point appears in the **Restore** area.

### Escalation

Escalate if the run fails again with the same error after you apply the resolution, or if the error code is not in the [error code table](#error-codes). Provide the information from [Before you begin](#before-you-begin).

## 2. Backup is taking longer than expected

### Symptoms

- A run stays in **Running** for longer than previous runs.
- Runs show the status **Skipped**.
- Runs stay in **Queued** before they start.

### Possible causes

- The run is the first run of the job. The first run backs up all selected data.
- A large volume of data changed since the last run.
- The source holds a very large number of small files.
- The tenant has 5 runs in progress. CloudVault queues additional runs.
- The network path between the source and CloudVault has limited bandwidth.

### Diagnostic steps

1. Open the run details. Compare the amount of data and the number of files with a previous run.
2. Check whether this is the first run of the job. The run history lists the oldest run at the bottom.
3. Open **Backup Jobs** and filter by the status **Running** and **Queued**. Count the runs.
4. Review the schedules of the jobs that run at the same time.

### Resolution steps

1. If this is the first run, wait for it to finish. Later runs are faster.
2. If many runs are queued, move some schedules to different start times.
3. If the job includes temporary or cache files, add exclude patterns such as `*.tmp`.
4. If runs are **Skipped**, increase the schedule interval so that a run can finish before the next start time.
5. If the network path is slow, ask your network team to check the bandwidth between the source and CloudVault.

### Verification

1. Wait for the next scheduled run, or select **Run now**.
2. Compare the duration with the previous run.
3. Confirm that no new **Skipped** runs appear in the run history.

### Escalation

Escalate if a run that backs up a small amount of data stays in **Running** for more than 24 hours, or if runs stay **Queued** while fewer than 5 runs are in progress.

## 3. Backup schedule did not execute

### Symptoms

- No run appears in the run history at the expected time.
- The **Next run** column is empty or shows an unexpected time.
- A run has the status **Skipped**.

### Possible causes

- The job state is **Paused**.
- The schedule type is **Manual only**.
- The schedule uses a different time zone than you expect.
- The previous run was still active at the scheduled start, so CloudVault recorded a **Skipped** run.
- The run is in the queue.

### Diagnostic steps

1. Open **Backup Jobs** and check the job state.
2. Open the job and select **Edit**. Check **Schedule type** and **Time zone**.
3. Compare the **Next run** time with the expected time in the schedule time zone.
4. Open the run history and look for a **Skipped** or **Queued** run near the expected time.

### Resolution steps

1. If the job is **Paused**, select **Resume job**.
2. If the schedule type is **Manual only**, select a schedule type and save the job.
3. If the time zone is wrong, select the correct time zone and save the job.
4. If a run was **Skipped**, follow the resolution steps in [Backup is taking longer than expected](#2-backup-is-taking-longer-than-expected).

### Verification

1. Open the job and confirm that the state is **Active**.
2. Confirm that **Next run** shows the expected date and time.
3. After the scheduled time, confirm that a new run appears in the run history.

### Escalation

Escalate if the job is **Active**, the schedule is correct, no run is active, and no run starts within 15 minutes of the scheduled time.

## 4. File restoration failed

### Symptoms

- The restore request status is **Failed**.
- The restore report shows error CV-2001, CV-2002 or CV-2003.
- Some files are missing at the destination.

### Possible causes

| Error code | Cause |
|---|---|
| CV-2001 | The source credential has no write permission at the destination. |
| CV-2002 | The restore point passed its retention period and CloudVault deleted it. |
| CV-2003 | The destination folder does not exist. |

Files can also be missing because you selected **Skip** under **If a file already exists**.

### Diagnostic steps

1. Open **Restore** and open the failed restore request.
2. Note the error code.
3. Open the restore report and review the status of each item.
4. Check the retention period of the job. Compare it with the age of the restore point.
5. Check the **If a file already exists** option that the request used.

### Resolution steps

**CV-2001**

1. Grant the source credential write permission on the destination folder.
2. Start a new restore request.

**CV-2002**

1. Select a newer restore point that contains the file.
2. If no restore point contains the file, the data cannot be recovered from CloudVault.

**CV-2003**

1. Create the destination folder on the data source.
2. Start a new restore request, or enter an existing folder path.

> [!NOTE]
> If you are unsure about the destination, restore to an **Alternate location** first. Compare the files, then copy them to the final location.

### Verification

1. Confirm that the restore request status is **Completed**.
2. Open the restore report and confirm that each item shows **Restored**.
3. Open the restored file at the destination and confirm that it opens correctly.

### Escalation

Escalate if a restore request fails with an error code that is not in the [error code table](#error-codes), or if the request is **Completed** but the destination files are missing or damaged.

## 5. Email notification not received

### Symptoms

- A run failed, but no email arrived.
- The test email does not arrive.
- Email arrives late or only for some recipients.

### Possible causes

- The notification profile status is **Disabled**.
- The profile is not assigned to the job.
- The event is not selected under **Send email when**.
- The recipient address is missing or has a typing error.
- The mail system filtered the message as spam or rejected the sender.
- Less than 10 minutes passed since the event. Delivery can take up to 10 minutes.

### Diagnostic steps

1. Open **Notifications** and select the profile. Check that the status is **Enabled**.
2. Check that the recipient list contains the correct address.
3. Check that the event is selected under **Send email when**. For a failed run, **Run failed** must be selected.
4. Open the job and select **Edit**. Check that **Notification profile** shows the profile.
5. Select **Send test email** on the profile.
6. Check the spam or quarantine folder of the recipient.

### Resolution steps

1. Set the profile to **Enabled**.
2. Assign the profile to the job.
3. Select the missing event, then save the profile.
4. Correct the recipient address.
5. Ask your mail administrator to allow `noreply@notifications.cloudvault.example`.

### Verification

1. Select **Send test email** on the profile.
2. Confirm that every recipient receives the test email within 10 minutes.

### Escalation

Escalate if the profile is correct, the mail system allows the sender, and the test email does not arrive after 30 minutes. Include the profile name, the recipient addresses and the time that you selected **Send test email**.

## 6. Dashboard not loading

### Symptoms

- The dashboard shows a blank page or a loading indicator that does not finish.
- Widgets show an error message.
- The Console displays "Unable to reach the service".

### Possible causes

- The browser is not supported or is out of date.
- JavaScript or cookies are disabled.
- A proxy or firewall blocks HTTPS access to the tenant URL.
- A browser extension blocks Console requests.
- Cached data in the browser is stale.
- A service incident is in progress.

### Diagnostic steps

1. Check the browser version against the supported browsers in the [Installation Guide](installation-guide.md#supported-browsers).
2. Open `https://status.cloudvault.example` and check for a service incident.
3. Open the Console in a private browsing window with extensions disabled.
4. Open the browser developer tools and select the network view. Reload the page. Note any requests that fail, and note their status codes.
5. Try from a different network.

### Resolution steps

1. Update the browser to a supported version.
2. Turn on JavaScript and cookies.
3. Clear the site data for the tenant URL, then reload the page.
4. Turn off the browser extension that blocks the requests.
5. Ask your network team to allow HTTPS (port 443) to the tenant URL.
6. If a service incident is in progress, wait for the status page to report that it is resolved.

### Verification

1. Reload the dashboard.
2. Confirm that all widgets load without error messages.

### Escalation

Escalate if the dashboard fails to load in a supported browser on a different network, and the status page shows no incident. Include the failed request status codes and the time of the failure.

## 7. Authentication failure

### Symptoms

- The sign-in page displays "Sign-in failed".
- The MFA code is rejected.
- The Console displays "Account locked".
- The Console returns you to the sign-in page during use.

### Possible causes

- The password is wrong.
- The MFA code is wrong, or the clock on the authenticator device is not accurate.
- The account is locked. CloudVault locks an account for 15 minutes after 5 failed sign-in attempts.
- The session expired. The Console ends a session after 30 minutes of inactivity.
- The account is not activated. The activation link expires after 72 hours.
- The password reset link expired. Reset links expire after 24 hours.

### Diagnostic steps

1. Check the exact message on the sign-in page.
2. Confirm that you use the correct tenant URL.
3. Check the time on the authenticator device. Turn on automatic date and time.
4. Ask an Administrator to open **Settings > Users** and check your account status.

### Resolution steps

1. If the password is wrong, select **Forgot password** and follow the reset email.
2. If the MFA code is rejected, correct the device time and enter a new code.
3. If the account is locked, wait 15 minutes, then sign in again.
4. If the device is lost, use a recovery code. If you have no recovery code, ask an Administrator to reset your MFA.
5. If the account status is **Invited**, ask an Administrator to resend the invitation.

> [!WARNING]
> Do not share MFA recovery codes or reset links. An Administrator never needs your password or your MFA code.

### Verification

1. Sign in with your password and a new MFA code.
2. Confirm that the dashboard opens.

### Escalation

Escalate if the Administrator account is locked out and no recovery code is available, or if you cannot sign in after you reset the password and the MFA. Escalate suspected unauthorized access to your security team immediately.

## Escalation process

1. Complete the diagnostic and resolution steps for the problem.
2. Collect the information in [Before you begin](#before-you-begin).
3. Contact your organization's CloudVault Administrator.
4. If the Administrator cannot resolve the problem, the Administrator contacts CloudVault support. The support contact is in your organization's service agreement.

## Related documentation

- [Installation Guide](installation-guide.md)
- [User Guide](user-guide.md)
- [Release Notes](release-notes.md)
- [Documentation home](../README.md)
