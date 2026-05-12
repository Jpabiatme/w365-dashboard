# W365 Dashboard

A single-file HTML dashboard for managing Windows 365 Cloud PCs, built on Microsoft Graph API. No backend required — runs entirely in the browser.

**Live demo:** https://jpabiatme.github.io/w365-dashboard/

---

## What it does

- **Overview** — all Cloud PCs with status, activity indicator, Intune compliance
- **Usage** — license funnel (purchased → provisioned → active), concurrency split by Enterprise/Frontline, idle machine detection
- **Licenses** — all tenant licenses with W365 highlighted, purchased vs assigned, toggle to show all
- **Users** — W365 users with MFA status, last sign-in, location, assigned Cloud PCs
- **Sessions** — connection history per Cloud PC (requires a few days of usage to generate)
- **Policies** — provisioning policies with assigned groups resolved by name
- **Apps** — discovered apps across Cloud PCs with Microsoft/third party filter
---

## Prerequisites

- A Microsoft 365 tenant with Windows 365 licenses
- An Azure App Registration (instructions below)

---

## Setup

### Step 1 — Create an App Registration

1. Go to [portal.azure.com](https://portal.azure.com)
2. Navigate to **Microsoft Entra ID → App registrations → New registration**
3. Name it anything (e.g. `W365 Dashboard`)
4. Leave everything else as default and click **Register**

---

### Step 2 — Add a redirect URI

In your App Registration → **Authentication → Add a platform → Single-page application**

Set the redirect URI to:

```
https://jpabiatme.github.io/w365-dashboard/
```

If you are hosting your own copy, use your own URL instead.

Click **Save**.

---

### Step 3 — Grant admin consent

You do not need to manually add API permissions. When you sign in for the first time, Microsoft will prompt you to grant consent on behalf of your organization for all the permissions the dashboard needs.

Click **Grant consent for [your org]** and confirm.

> You must be a Global Administrator or Privileged Role Administrator to grant consent.

For reference, the dashboard requests these delegated permissions:

| Permission | Purpose |
|---|---|
| `CloudPC.Read.All` | List and read Cloud PCs |
| `DeviceManagementManagedDevices.Read.All` | Intune compliance and managed devices |
| `User.Read` | Signed-in user profile |
| `User.Read.All` | All user profiles and sign-in activity |
| `Organization.Read.All` | Tenant organization info |
| `AuditLog.Read.All` | Sign-in logs and audit events |
| `UserAuthenticationMethod.Read.All` | MFA status per user |
| `Reports.Read.All` | W365 usage and session reports |
| `Group.Read.All` | Resolve group names in provisioning policies |
| `DeviceManagementApps.Read.All` | Discovered apps on managed devices |
| `Mail.Send` | Send nudge emails to inactive users |

---

### Step 4 — Copy your IDs

In your App Registration → **Overview**, copy:

- **Application (client) ID**
- **Directory (tenant) ID**

Both look like: `xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx`

---

### Step 5 — Open the dashboard

Go to [https://jpabiatme.github.io/w365-dashboard/](https://jpabiatme.github.io/w365-dashboard/)

Paste your **Tenant ID** and **Client ID** into the login screen and click **Sign in with Microsoft**.

---


## Security notes

- All permissions are **delegated** — the dashboard acts as the signed-in user, not as an application
- No data is stored anywhere — everything lives in the browser session
- Restrict access to the App Registration via **Enterprise Applications → Properties → Assignment required** if needed

---

## Built with

- Vanilla HTML, CSS and JavaScript — no frameworks, no build process
- [MSAL.js](https://github.com/AzureAD/microsoft-authentication-library-for-js) for Microsoft authentication
- [Microsoft Graph API](https://docs.microsoft.com/en-us/graph/) for all tenant data

