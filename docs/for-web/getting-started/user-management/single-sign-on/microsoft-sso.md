# Microsoft Single Sign On (Azure SSO)

This document will guide you through changing from Webtrends Optimize authentication to using Microsoft Azure SSO.

!!! info "Enabling Microsoft SSO on your account"

    The Webtrends Optimize support team will need to enable this for your user, and for any of your colleagues who you wish to authenticate with Microsoft SSO. This typically takes 24-48 hours.
    
    You will first need to perform steps listed below, and then contact the Support Team for enablement at [Contact Support](mailto:support@webtrends-optimize.com){target=_blank}

We have built this to specifically have Microsoft handle the authentication, but Rights in the UI managed in the traditional way (e.g. User can view a report, can view a test, can create/edit/delete, etc.).

## Step 1: Get Tenant ID from Azure AD

As described in: [https://learn.microsoft.com/en-us/azure/active-directory/fundamentals/active-directory-how-to-find-tenant](https://learn.microsoft.com/en-us/azure/active-directory/fundamentals/active-directory-how-to-find-tenant)

1. Go to the Azure portal
2. Navigate to the service Azure Active Directory
3. Scroll down the blade on the left to Properties

    Note: You can go straight to this URL, but we recommend browsing there yourself for security/reassurance: [https://portal.azure.com/#view/Microsoft_AAD_IAM/ActiveDirectoryMenuBlade/~/Properties](https://portal.azure.com/#view/Microsoft_AAD_IAM/ActiveDirectoryMenuBlade/~/Properties)

    ![Azure Portal](/assets/portal-tenant-id.png)

5. Copy your Tenant ID and share this with your Account Manager.
6. We will then enable Microsoft/Azure SSO for your account. This may take 1-2 days

## Step 2: First login with Microsoft SSO

1. Click the Sign in with Microsoft SSO button on our login screen:

    ![WTO Login Form](/assets/login-form.png)

2. You will be taken to Microsoft. For security, we recommend making sure the domain says microsoftonline.com.

    Please log in, using MFA if it asks you to. If you select the “Keep me signed in” option, most/all of these steps won’t need to be repeated until you discard cookies, and you’ll have 1-click login for subsequent UI sessions.

3. After following these steps, you will be taken into our UI. This will be the Account Selection screen if you have access to multiple accounts, or the Dashboard if you have access to one.

![Login with Microsoft screen 1](/assets/login with microsoft screen 1.png)

![Login with Microsoft screen 2](/assets/login with microsoft screen 2.png)

![Login with Microsoft screen 3](/assets/login with microsoft screen 3.png)

![Login with Microsoft screen 4](/assets/login with microsoft screen 4.png)

![Login with Microsoft screen 5](/assets/login with microsoft screen 5.png)

![Login with Microsoft screen 6](/assets/login with microsoft screen 6.png)

## For subsequent logins:

You will no longer need to approve use of Webtrends Optimize, and so your login will consist of simply authenticating and then being taken straight into the Webtrends Optimize UI.