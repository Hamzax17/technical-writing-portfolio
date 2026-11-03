# CloudVault Installation and Initial Configuration Guide

**Product version:** CloudVault 1.2.0  
**Document status:** Fictional example

> [!IMPORTANT]
> CloudVault is a fictional product. The screens, URLs, addresses and values in this guide are illustrative. They are not real and cannot be executed.

## Purpose and scope

This guide explains how to activate a CloudVault tenant, sign in to the CloudVault Console, and complete the initial configuration.

CloudVault is a web-based service. You do not install software on a server to use it. "Installation" in this guide means account activation, access setup and first-time configuration.

This guide covers:

- Account activation and first sign-in.
- Multi-factor authentication (MFA) setup.
- Tenant settings.
- Creating a backup vault.
- Registering a data source.
- Creating a notification profile.
- Inviting users.

This guide does not cover creating backup jobs or restoring files. See the [User Guide](user-guide.md).

## Intended audience

This guide is for IT administrators who set up CloudVault for their organization. Readers should know how to:

- Manage user accounts and file shares in a Windows or Linux environment.
- Work with network firewall rules, or contact the team that does.

## How to read this guide

This guide uses two kinds of content:

| Type | How it appears | Meaning |
|---|---|---|
| Procedure | Numbered steps | Actions you perform in the CloudVault Console. |
| Illustrative example | Block labeled **Illustrative example** | Sample values that show the expected format. Replace them with values from your environment. |

## System requirements

### Supported browsers

The CloudVault Console supports the current and previous major versions of these browsers:

- Google Chrome
- Microsoft Edge
- Mozilla Firefox
- Apple Safari

Enable JavaScript and cookies in the browser. The minimum supported screen resolution is 1366 × 768.

### Network requirements

| Requirement | Detail |
|---|---|
| Outbound access from administrator workstations | HTTPS (port 443) to the tenant URL and to `status.cloudvault.example`. |
| Access from CloudVault to data sources | The data source must accept connections from the CloudVault source addresses. These addresses appear in **Settings > Network** after you sign in. |
| Email | Notification email comes from `noreply@notifications.cloudvault.example`. Your mail system must accept it. |

### Supported data sources

| Source type | Requirement |
|---|---|
| SMB file share | A service account with read permission. Write permission is required to restore files to the original location. |
| NFS export | An export that allows access from the CloudVault source addresses, with read access. Write access is required to restore files to the original location. |

## Prerequisites

Complete these items before you begin:

1. Obtain the activation email for your tenant. CloudVault sends it to the primary administrator address that your organization provided.
2. Install an authenticator app that supports time-based one-time passwords on a mobile device.
3. Identify at least one SMB file share or NFS export to protect.
4. Create a service account for each SMB file share, with the permissions listed in [Supported data sources](#supported-data-sources).
5. Ask your network team to allow the CloudVault source addresses to reach your data sources. You can complete this step after you sign in, because the addresses appear in **Settings > Network**.
6. Collect the email addresses of the people who must receive backup notifications.

## Account requirements

CloudVault uses three roles.

| Role | Permissions |
|---|---|
| Administrator | All actions, including tenant settings, vaults, sources, users and notifications. |
| Backup Operator | Create and manage backup jobs, run backups, and restore files. |
| Viewer | View the dashboard, jobs, run history and restore points. No changes. |

The primary administrator account has the Administrator role. Use this account for the procedures in this guide.

> [!NOTE]
> The activation link expires 72 hours after CloudVault sends it. If the link expires, ask your organization's CloudVault contact to send a new activation email.

## Installation and access procedures

### Activate the administrator account

**Purpose:** Create your password and activate your account.

**Prerequisites:** The activation email. A browser that meets the [system requirements](#system-requirements).

1. Open the activation email.
2. Select **Activate account**.
3. Enter a password. The password must contain at least 12 characters.
4. Select **Activate**.

**Expected result:** The Console displays the message "Account activated" and opens the sign-in page.

### Sign in for the first time

**Purpose:** Access the CloudVault Console.

**Prerequisites:** An activated account.

1. Open the tenant URL from the activation email.

   > **Illustrative example:** `https://contoso-it.cloudvault.example`

2. Enter your email address and password.
3. Select **Sign in**.

**Expected result:** If your tenant requires MFA, the Console prompts you to set it up. Otherwise, the Console opens the dashboard.

> [!NOTE]
> The Console signs you out after 30 minutes of inactivity.

### Set up multi-factor authentication

**Purpose:** Protect the administrator account with a second authentication factor.

**Prerequisites:** An authenticator app on a mobile device.

1. On the MFA setup page, scan the QR code with the authenticator app.
2. Enter the 6-digit code that the app displays.
3. Select **Verify**.
4. Copy the recovery codes and store them in a secure location.
5. Select **Done**.

**Expected result:** The Console displays the dashboard. Future sign-ins require your password and a 6-digit code.

> [!WARNING]
> Store the recovery codes separately from your password. If you lose both your authenticator device and your recovery codes, you cannot sign in.

## Initial configuration

Complete the procedures in this order. Each procedure uses the output of the previous one.

### Configure tenant settings

**Purpose:** Set the time zone and security policy that apply to the whole tenant.

**Prerequisites:** Administrator role.

1. In the left navigation pane, select **Settings**.
2. Select **General**.
3. In **Default time zone**, select your organization's time zone.
4. Select **Security**.
5. Turn on **Require MFA for all users**.
6. Select **Save**.

**Expected result:** The Console displays "Settings saved". New backup job schedules use the default time zone. All users must set up MFA at their next sign-in.

> [!NOTE]
> Changing the default time zone does not change existing schedules. Each schedule keeps the time zone that you selected when you created it.

### Create a backup vault

**Purpose:** Create the storage location for restore points.

**Prerequisites:** Administrator role.

1. In the left navigation pane, select **Vaults**.
2. Select **Create vault**.
3. In **Vault name**, enter a name that is 3 to 64 characters long.

   > **Illustrative example:** `finance-vault`

4. In **Capacity limit**, enter the maximum storage that the vault can use.
5. Select **Create**.

**Expected result:** The vault appears in the vault list with the status **Ready** and 0% usage.

> [!WARNING]
> When a vault reaches 100% of its capacity limit, backup runs that use the vault fail with error CV-1003. Set a capacity limit that allows for data growth.

### Register a data source

**Purpose:** Tell CloudVault which file share or export to protect.

**Prerequisites:**

- Administrator role.
- The source path and service account from the [prerequisites](#prerequisites).
- Network access from the CloudVault source addresses to the data source.

1. In the left navigation pane, select **Sources**.
2. Select **Add source**.
3. In **Source type**, select **SMB file share** or **NFS export**.
4. In **Source name**, enter a name that identifies the source.
5. In **Path**, enter the path of the file share or export.

   > **Illustrative example (SMB):** `\\fileserver01\finance`
   >
   > **Illustrative example (NFS):** `fileserver02:/exports/engineering`

6. For an SMB file share, enter the service account user name and password in **Source credential**.
7. Select **Test connection**.
8. When the test succeeds, select **Save**.

**Expected result:** The test displays "Connection successful". The source appears in the source list with the status **Connected**.

> [!NOTE]
> CloudVault stores source credentials in encrypted form. The Console never displays a saved password.

### Create a notification profile

**Purpose:** Define who receives email about backup events.

**Prerequisites:** Administrator role. The recipient email addresses.

1. In the left navigation pane, select **Notifications**.
2. Select **Create profile**.
3. In **Profile name**, enter a name.
4. In **Recipients**, enter up to 20 email addresses.
5. Under **Send email when**, select the events: **Run failed**, **Run completed with warnings**, **Run completed**.
6. Select **Send test email**.
7. Select **Save**.

**Expected result:** Each recipient receives a test email within 10 minutes. The profile appears in the profile list with the status **Enabled**.

### Invite users

**Purpose:** Give other staff access to the Console.

**Prerequisites:** Administrator role.

1. In the left navigation pane, select **Settings**.
2. Select **Users**.
3. Select **Invite user**.
4. Enter the user's email address.
5. Select a role: **Administrator**, **Backup Operator** or **Viewer**.
6. Select **Send invitation**.

**Expected result:** The user appears in the user list with the status **Invited**. The status changes to **Active** after the user activates the account.

> [!IMPORTANT]
> Assign the least privileged role that lets the user do their work. Limit the Administrator role to the staff who manage tenant settings.

## Verification checklist

Confirm each item before you create your first backup job.

- [ ] You can sign in with your password and an MFA code.
- [ ] The default time zone matches your organization's time zone.
- [ ] **Require MFA for all users** is on.
- [ ] At least one vault has the status **Ready**.
- [ ] At least one data source has the status **Connected**.
- [ ] Each notification recipient received the test email.
- [ ] Each invited user has the correct role.
- [ ] Recovery codes are stored in a secure location.

## Common installation issues

| Issue | Possible cause | Resolution |
|---|---|---|
| The activation link shows "Link expired". | More than 72 hours passed since CloudVault sent the email. | Request a new activation email from your organization's CloudVault contact. |
| The MFA code is rejected. | The clock on the mobile device is not accurate. | Turn on automatic date and time on the device, then enter a new code. |
| **Test connection** fails for an SMB file share. | The service account name or password is wrong, or the account lacks read permission on the share. | Verify the credentials and permissions. Enter the credentials again, then select **Test connection**. |
| **Test connection** fails for every source. | A firewall blocks the CloudVault source addresses. | Open **Settings > Network**. Ask your network team to allow the listed addresses to reach the source. |
| The test email does not arrive. | The mail system rejects the sender address, or the email is in a spam folder. | Allow `noreply@notifications.cloudvault.example`. Check spam folders. Select **Send test email** again. |
| The sign-in page does not load. | A proxy or firewall blocks the tenant URL. | Allow HTTPS to the tenant URL. See [Dashboard not loading](troubleshooting.md#6-dashboard-not-loading). |

## Related documentation

- [User Guide](user-guide.md): Create backup jobs, monitor runs and restore files.
- [Troubleshooting Guide](troubleshooting.md): Resolve common problems.
- [Release Notes](release-notes.md): Review changes in each version.
- [Documentation home](../README.md)
