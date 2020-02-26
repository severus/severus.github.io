---
layout: default
title: Restrict access to services
parent: App Engine
grand_parent: Google Cloud Platform
nav_order: 1
---

# Restrict access to App Engine Services with Identity-Aware Proxy
{: .no_toc }

Identity-Aware Proxy (IAP) lets you manage who has access to services hosted on [App Engine](/google-cloud-platform/appengine/), Compute Engine, or an HTTPS Load Balancer.
{: .fs-6 .fw-300 }

[Learn more](https://cloud.google.com/iap/docs){: .btn .fs-5 .mb-4 .mb-md-0 }{:target="_blank"}

---

## Table of contents
{: .no_toc .text-delta }

1. TOC
{:toc}

---

## Requirements

For example, we have three App Engine services deployed — `default`, `admin` and `api`.

`default` is our landing page, `admin` is a [React application](/google-cloud-platform/appengine/react-app), and `api` is a RESTful API which provides data to the React app.

Our access restrictions should be as following:

- Access to `default` service should not be restricted. We want our landing page to be accessible by everyone.
- Access to `admin` should be restricted to a list of specified users. We want to keep our administrative area secure.
- Since `admin` communicates with `api`, the access to the latter should also be restricted to the same list of users as for `admin`.

## Implementation

Enable Identity-Aware Proxy as described in the [documentation](https://cloud.google.com/iap/docs/app-engine-quickstart#enabling_iap){:target="_blank"}:

### Selecting a project

1. Go to the [Identity-Aware Proxy page](https://console.cloud.google.com/security/iap){:target="_blank"}.
2. If you don't already have an active project, you'll be prompted to select the project you want to secure with IAP. Select the project to which you deployed your application.

### Configuring the OAuth consent screen

If you haven't configured your project's OAuth consent screen, you'll be prompted to do so. An email address and product name are required for the OAuth consent screen.

1. Go to the [OAuth consent screen](https://console.cloud.google.com/apis/credentials/consent){:target="_blank"}.
2. Under **Support email**, select the email address you want to display as a public contact. This email address must be your email address, or a Google Group you own.
3. Enter the **Application name** you want to display.
4. Add any optional details you'd like.
5. Click **Save**.

To change information on the OAuth consent screen later, such as the product name or email address, repeat the preceding steps to configure the consent screen.

### Setting up IAP access

Go to the [Identity-Aware Proxy page](https://console.cloud.google.com/security/iap){:target="_blank"}.

Under **HTTPS Resources**, select **All Web Service/App Engine app/default** resource by checking the box to its left. On the right side panel, click **Add Member**.

In the **Add members** dialog, add **allUsers** and grant the **IAP-secured Web App User** role for the project.

When you're finished, click **Add**.

Select **All Web Service/App Engine app/admin** and **All Web Service/App Engine app/api** resources by checking the boxes to its left. On the right side panel, click **Add Member**.

In the **Add members** dialog, add the email addresses of groups or individuals to whom you want to grant the **IAP-secured Web App User** role for the project.

The following kinds of accounts can be members:

- **Google Account**: user@gmail.com
- **Google Group**: admins@googlegroups.com
- **Service account**: server@example.gserviceaccount.com
- **G Suite domain**: example.com

Make sure to add a Google Account that you have access to.</li>

When you're finished adding members, click Add.

### Turning on IAP

1. On the **Identity-Aware Proxy** page, under **HTTPS Resources**, find the App Engine app you want to restrict access to. The **Published** column shows the URL of the app. To turn on IAP for the app, toggle the on/off switch in the **IAP** column.
  - To enable IAP, you need the `appengine.applications.update`, `clientauthconfig.clients.create`, and `clientauthconfig.clients.getWithSecret` permissions. These permissions are granted by roles, such as the Project Editor role. To learn more, see [Managing access to IAP-secured resources](https://cloud.google.com/iap/docs/managing-access#turning_on_and_off){:target="_blank"}.
2. To confirm that you want IAP to secure the application, click **Turn On** in the **Turn on IAP** window that appears. After you turn it on, IAP requires login credentials for all connections to your application. Only accounts with the **IAP-secured Web App User** role on this project will be given access.

### Test Access

1. Access the app URL from the Google Account that you added to IAP. You should have unrestricted access to the app.
2. Use an incognito window or private mode in your browser to access the app and sign in when prompted. If you try to access the app with an account that isn't authorized, you'll see a message saying that you don't have access.
