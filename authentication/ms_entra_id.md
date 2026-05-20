# Configure Microsoft Entra ID (Azure AD) Single Sign-On experience

Please follow the instructions below to set up Leap Rail on your MS365 as an oauth application.

You will receive the following from LeapRail for this process:

* Your subdomain
* A Leap Rail Single Sign-On certificate
* List of Roles and their corresponding Role ids

## Register the Leap Rail application

> **Important:** Start in **App registrations**, not **Enterprise applications**. Registering the app under App registrations causes Entra ID to automatically create the matching Enterprise Application as a byproduct, with the OIDC protocol already bound. Creating the app from the Enterprise applications page instead produces a bare service principal that cannot be used for Leap Rail's OIDC sign-on flow. See the Troubleshooting section at the bottom of this page if you suspect this happened.

1. Navigate to your Azure Portal
1. Search for "Entra ID" in the top search box.

![](../assets/authentication/ms_entra_id/entra1.png)

1. Click on "Manage" and "App registrations" on the left menu.

![](../assets/authentication/ms_entra_id/entra2.png)

1. Click **New registration**
1. On the "Register an application" page, Add "Leap Rail" as a single tenant application.

![](../assets/authentication/ms_entra_id/entra3.png)

1. Click "Register" and save the Application/Client ID and Directory/Tenant ID. Send this info to your Leap Rail contact.

![](../assets/authentication/ms_entra_id/entra4.png)

1. Click on "Manage" and then "Authentication" on the left menu. Click on "Add a platform".

![](../assets/authentication/ms_entra_id/entra20.png)

1. Click on "Web" and enter "https://apps.leaprail.com/login/sso" as "Redirect Uri" and then click on "Configure" button at the bottom.

![](../assets/authentication/ms_entra_id/entra22.png)

1. Click on "Add a platform" again and then "iOS / macOS" and enter "com.leaprail.leaprailmobileapp" as "Bundle Id" and then click on "Configure".

![](../assets/authentication/ms_entra_id/entra21.png)

1. Click on "Add a platform" one more time and then "Android" and enter "com.leaprail.leaprailmobileapp" as "Package name", "Xr5Dggr2GTI8jyHIlgwgwDa5JuY=" as "Signature hash", and then click on "Configure".

1. Click on "Save" to add all three authentication methods.

![](../assets/authentication/ms_entra_id/entra23.png)

1. Click on "Manage" and then "Certificates & Secrets" on the left menu. On the tabs in the middle of the screen, click on "Certificates".

![](../assets/authentication/ms_entra_id/entra5.png)

1. Click on "Upload certificate" and select the "Leap Rail Single Sign-On certificate" you received from the Leap Rail team.

![](../assets/authentication/ms_entra_id/entra6.png)

1. Verify that the certificate description is for Leap Rail.

![](../assets/authentication/ms_entra_id/entra7.png)

1. Next, click on "Token configuration" and "Add optional claim" link.

![](../assets/authentication/ms_entra_id/entra8.png)

1. On the "Add optional claim" select "ID" as token type and select "email", "family_name", and "given_name" as "Claim".

![](../assets/authentication/ms_entra_id/entra9.png)

1. Click "Add" and on the dialog, select "Turn on the Microsoft Graph email, profile permission".

![](../assets/authentication/ms_entra_id/entra10.png)

1. Next, click on "App roles"

![](../assets/authentication/ms_entra_id/entra11.png)

1. For every role that you received from the Leap Rail team, you will repeat the steps of clicking the "Create app role" link.

![](../assets/authentication/ms_entra_id/entra12.png)

1. After you add all the roles, you should have a screen that looks like this:

![](../assets/authentication/ms_entra_id/entra13.png)

## Give users permission to Leap Rail

1. On the top search box, search for "Enterprise applications"

![](../assets/authentication/ms_entra_id/entra14.png)


1. Select the Leap Rail application.

![](../assets/authentication/ms_entra_id/entra15.png)

1. Click on "Manage" and the "Users and groups"
1. In order to give a user/group permission to one of the Leap Rail roles, click on the "Add user/group" link.

![](../assets/authentication/ms_entra_id/entra16.png)

1. Select the user and then the appropriate role.

![](../assets/authentication/ms_entra_id/entra17.png)

1. You will now see the user being assigned to the role.

![](../assets/authentication/ms_entra_id/entra18.png)

1. In order to give a user multiple roles, repeat this process as many times as necessary.

![](../assets/authentication/ms_entra_id/entra19.png)

1. Users given access to the Leap Rail roles can navigate to `https://apps.leaprail.com/login/sso` and log in by typing in the "domain" value that the Leap Rail team assigned to your organization in the beginning of this process. Alternatively, share `https://apps.leaprail.com/login/sso?domain=<your subdomain>` as a deep-linked URL — the `domain` query parameter pre-fills the subdomain so users skip directly to Microsoft sign-in.

1. Remember to send the client ID and tenant ID to the Leap Rail team so they can complete the process on their end.

## Grant admin consent

By default, each user will see a one-time Microsoft "Permissions requested" consent prompt the first time they sign in to Leap Rail, asking them to allow the app to view their basic profile. To suppress this prompt for all users in your tenant, a Global Administrator, Cloud Application Administrator, Application Administrator, or Privileged Role Administrator can grant admin consent on behalf of the organization:

1. Navigate to "Entra ID" → "App registrations" → "Leap Rail".
1. On the left menu, click "API permissions".
1. Verify that `User.Read`, `email`, and `profile` are listed under "Microsoft Graph". These were added earlier when you turned on the Microsoft Graph email and profile permission during optional claim setup.
1. Click "Grant admin consent for <your tenant>" at the top of the permissions list, then confirm.
1. The "Status" column should turn green and read "Granted for <your tenant>".

After this step, users will no longer see the consent dialog on first login.

The equivalent path under "Enterprise applications" → "Leap Rail" → "Security" → "Permissions" → "Grant admin consent" also works.

## Make Leap Rail visible in users' My Apps portal

To have Leap Rail appear as a tile in each assigned user's [My Apps portal](https://myapps.microsoft.com) and the Microsoft 365 app launcher, configure the following:

1. Navigate to "Entra ID" → "App registrations" → "Leap Rail" → "Branding & properties".
1. Set the "Home page URL" to `https://apps.leaprail.com/login/sso?domain=<your subdomain>`, replacing `<your subdomain>` with the subdomain value the Leap Rail team assigned to your organization. Including the `domain` query parameter ensures the My Apps tile launches users directly into Microsoft sign-in rather than requiring them to enter the subdomain manually.
1. (Optional) Upload a Leap Rail logo on this same page.
1. Click "Save".
1. Navigate to "Entra ID" → "Enterprise applications" → "Leap Rail" → "Properties".
1. Confirm the following settings:
   - "Enabled for users to sign-in?" → "Yes"
   - "Visible to users?" → "Yes"
   - "Assignment required?" → "Yes" (so only users assigned to a Leap Rail role under "Users and groups" see the tile)
1. (Optional) Upload the same logo on this page. Enterprise application logos are stored separately from App registration logos.
1. Click "Save".

Any user assigned to a Leap Rail role will now see a Leap Rail tile in their My Apps portal and M365 app launcher.

## Troubleshooting

### Single sign-on page shows a method picker (SAML / Password / Linked / Disabled)

The "Single sign-on" blade for the Leap Rail Enterprise Application should display a read-only "OIDC-based Sign-on" summary page. If it instead shows a picker dialog with SAML, Password-based, Linked, and Disabled tiles, the Enterprise Application was created directly from the "Enterprise applications" menu rather than as a byproduct of an App registration. None of the picker options will work for Leap Rail's OIDC flow.

To diagnose, check "Entra ID" → "App registrations" for a "Leap Rail" entry:

- If one exists, your tenant has both a correct OIDC application (auto-created from the App registration) and a stray non-gallery Enterprise Application. Delete the stray Enterprise App that shows the picker, and re-apply the user assignment and admin consent steps on the correct Enterprise App (the one that shows "OIDC-based Sign-on" with no picker).
- If no Leap Rail App registration exists, the registration step at the top of this guide was skipped. Delete the existing Enterprise App and start over from "Register the Leap Rail application". Send the new Application/Client ID and Directory/Tenant ID to your Leap Rail contact, since they will have changed.

### Users see a "Permissions requested" prompt on first sign-in

This is standard Microsoft consent behavior when an app requests delegated Graph permissions for the first time on a user account. To suppress it tenant-wide, follow the "Grant admin consent" section above.
