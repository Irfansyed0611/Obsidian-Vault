
# Detailed Survey Note on Setting Up SSO in LogicMonitor Using Azure AD

This comprehensive guide outlines the process for setting up Single Sign-On (SSO) in LogicMonitor using Azure Active Directory (Azure AD), based on official documentation from LogicMonitor and Microsoft. The integration leverages SAML 2.0 for secure authentication, allowing users to log into LogicMonitor using their Azure AD credentials. This survey note provides a detailed, step-by-step procedure, addressing both technical and organizational considerations, and includes tables for clarity.

## Background and Importance
SSO simplifies user authentication by enabling access to multiple applications with a single set of credentials, enhancing security and user experience. LogicMonitor, a cloud-based IT infrastructure monitoring platform, supports SSO with Azure AD, which is Microsoft's identity and access management service. The integration is particularly beneficial for organizations using Azure AD for identity management, as it reduces password fatigue and improves compliance with security policies.

## Prerequisites
Before beginning, ensure the following are in place:
- An active LogicMonitor account with administrator privileges.
- A valid Azure AD subscription.
- Users and groups configured in Azure AD for SSO access.

## Step-by-Step Procedure

### Step 1: Adding the LogicMonitor App to Azure AD
The first step involves integrating LogicMonitor into Azure AD to enable SSO. This is done by adding LogicMonitor as an enterprise application:

1. Sign in to the Azure portal at [https://portal.azure.com](https://portal.azure.com) and navigate to **Azure Active Directory**.
2. Select **Enterprise applications** and then **All applications**.
3. Click **New application** and search for "LogicMonitor" in the Azure AD Gallery.
4. Select **LogicMonitor** from the results and click **Create**.

This step typically takes a few minutes, and the application will be added to your tenant.

### Step 2: Configuring Azure AD SSO for LogicMonitor
Once the app is added, configure the SSO settings using SAML. This involves setting up the necessary URLs and downloading metadata for LogicMonitor:

1. Navigate to the LogicMonitor app in the Azure portal and click **Set up single sign on**.
2. From the **Users and groups** page, add the users or user groups that will use SSO to log into LogicMonitor.
3. Select **SAML** as the single sign-on method.
4. Click **Edit** in the **Basic SAML Configuration** pane and fill in:
   - **Identifier (Entity ID)**: The URL of your LogicMonitor portal (e.g., `https://yourportalname.logicmonitor.com`).
   - **Reply URL (Assertion Consumer Service URL)**: `https://yourportalname.logicmonitor.com/santaba/saml/SSO/` (replace "yourportalname" with your actual portal name).
   - **Sign on URL**: 
     - If **Multi Idp Support** is disabled, use one of:
       - `https://COMPANYNAME.logicmonitor.com/santaba/saml/login?userDomain=&url=`
       - `https://COMPANYNAME.logicmonitor.com/santaba/saml/login`
       - `https://COMPANYNAME.logicmonitor.com/santaba/saml/login?userDomain=USERDOMAIN&url=`
     - If **Multi Idp Support** is enabled, use `https://COMPANYNAME.logicmonitor.com/santaba/saml/login?userDomain=USERDOMAIN&url=` (ensure `userDomain` matches LogicMonitor SSO configuration).
5. Click **Save**.
6. Select **User Attribute & Claims** and add a group claim if necessary:
   - For on-premise environments with Azure AD Connect, rename the outgoing group claim attribute from `http://schemas.microsoft.com/ws/2008/06/identity/claims/groups` to `groups`.
   - Configure group claim settings, considering the SAML assertion limit (150 groups). Use the option **Groups assigned to the application** and assign groups under the **Users and Groups** section to avoid token size issues.
7. Click **Save**.
8. Download the **Federation Metadata XML** file from the **SAML Signing Certificate** section.

This step is critical, as incorrect URLs or group configurations can lead to authentication failures. The metadata file is essential for the next step in LogicMonitor.

| **Field**              | **Description**                                           | **Example**                                              |
| ---------------------- | --------------------------------------------------------- | -------------------------------------------------------- |
| Identifier (Entity ID) | URL of your LogicMonitor portal                           | `https://companyname.logicmonitor.com`                   |
| Reply URL              | Assertion Consumer Service URL for SAML responses         | `https://companyname.logicmonitor.com/santaba/saml/SSO/` |
| Sign on URL            | URL for initiating SSO, varies based on Multi Idp Support | See above for options                                    |

### Step 3: Configuring LogicMonitor SSO for Azure AD
With the metadata file ready, configure SSO in LogicMonitor to accept authentication from Azure AD:

1. Log in to your LogicMonitor portal as an administrator.
2. Navigate to the **Settings** menu and select **Single Sign On**.
3. Enable the **Single Sign On** checkbox.
4. Select a role from the **Default Role Assignment** dropdown (e.g., `readonly`).
5. Upload the downloaded **Federation Metadata XML** file into the **Identity Provider Metadata** field.
6. (Optional) Enable **Restrict Single Sign On** to force SSO authentication, ensuring users cannot log in without Azure AD.
7. (Optional) Set the number of days for user sign-in duration to control session length.
8. (Optional) Enable **Single Logout (SLO)** for coordinated logout across services.
9. Click **Save**.

This step ensures LogicMonitor trusts Azure AD as the identity provider and maps users to appropriate roles.

### Step 4: Creating a LogicMonitor Test User
To test the SSO, create a test user in LogicMonitor:

1. In LogicMonitor, navigate to **Settings > Roles and Users > Add**.
2. Fill in the following details:
   - Username
   - Email
   - Password
   - Retype password
3. Select the appropriate **Roles**, **View Permissions**, and **Status**.
4. Click **Submit**.

This test user will be used to verify the SSO flow, ensuring the integration works as expected.

### Step 5: Testing the SSO Integration
Finally, test the SSO to confirm users can authenticate seamlessly:

1. In the Azure portal, select **Single sign on** for the LogicMonitor app.
2. Click **Test** from the **Test single sign on with LogicMonitor** panel.
3. Select **Sign in as someone else**.
4. Log in as a user who has been assigned the LogicMonitor app in Azure AD.
5. If successful, the user should be able to log into LogicMonitor with the default role selected.

Testing is crucial to identify any configuration errors, such as mismatched URLs or group claim issues.

| **Test Option**                    | **Description**                                                                 |
|------------------------------------|---------------------------------------------------------------------------------|
| Test this application              | Redirects to LogicMonitor Sign-on URL for testing                              |
| Sign in as someone else            | Allows testing with a specific user assigned to the app                        |
| Use Microsoft My Apps              | Access via [My Apps portal](https://support.microsoft.com/account-billing/sign-in-and-start-apps-from-the-my-apps-portal-2f3b1bae-0e5a-4a86-a33e-876fbd2a4510) for broader testing |

## Additional Considerations
Several factors can impact the success of the integration:
- **Group Claim Attribute**: For on-premise environments using Azure AD Connect, ensure the group claim attribute is renamed to `groups` in Azure AD for LogicMonitor, as specified in the documentation at [Configuring Microsoft Azure AD Idp](https://www.logicmonitor.com/support/security/single-sign-on#h-configuring-microsoft-azure-ad-idp).
- **SAML Assertion Limit**: Azure AD limits the number of groups in a token to 150, including nested groups. To mitigate, use **Groups assigned to the application** and assign groups under **Users and Groups**, as detailed in [Microsoft Azure documentation](https://learn.microsoft.com/en-us/azure/active-directory/hybrid/connect/how-to-connect-fed-group-claims).
- **Multi Idp Support**: If enabled, ensure the `userDomain` in the Sign on URL matches the LogicMonitor configuration to avoid authentication errors.
- **Role Assignment**: The default role in LogicMonitor (e.g., `readonly`) should align with organizational needs, and administrators can adjust this during setup.

## Time and Complexity
The process is likely to take 30-60 minutes, depending on familiarity with Azure AD and LogicMonitor. For large organizations with complex group structures, additional time may be needed to manage group claims and test thoroughly.

## Unexpected Details
One unexpected aspect is the need to handle group claim attributes, particularly for on-premise environments, which requires renaming to "groups" in Azure AD. This step is not always highlighted in basic guides but is crucial for ensuring proper role assignment and avoiding token size issues with large group numbers.

## Conclusion
This detailed procedure ensures a secure and efficient SSO integration between LogicMonitor and Azure AD, leveraging SAML for authentication. By following these steps and considering the additional factors, organizations can streamline user access while maintaining robust security.

---

## Key Citations
- [Configuring the Azure Active Directory SSO Integration LogicMonitor](https://www.logicmonitor.com/support/configuring-azure-active-directory-sso)
- [Microsoft Entra SSO integration with LogicMonitor Microsoft Learn](https://learn.microsoft.com/en-us/entra/identity/saas-apps/logicmonitor-tutorial)
- [Single Sign On LogicMonitor](https://www.logicmonitor.com/support/security/single-sign-on)