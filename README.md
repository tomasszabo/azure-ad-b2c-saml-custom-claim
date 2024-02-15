# Azure AD B2C custom policy with SAML2 token and custom claims

This is an example of Azure AD B2C custom policy with SAML2 token and custom claims. The custom policy is based on the starter pack `SocialAndLocalAccounts` and the custom policy is extended to include SAML2 token and custom claim.

## Deployment

To create the custom policy, you need to follow the steps below:

1. [Register a SAML application in Azure AD B2C](https://learn.microsoft.com/en-us/azure/active-directory-b2c/saml-service-provider?tabs=macos&pivots=b2c-custom-policy)
2. [Add custom claims and customize user input](https://learn.microsoft.com/en-us/azure/active-directory-b2c/configure-user-input?pivots=b2c-custom-policy#include-a-claim-in-the-token)
3. [Enable custom claims by enabling Azure AD B2C extensions app](https://learn.microsoft.com/en-us/azure/active-directory-b2c/user-flow-custom-attributes?pivots=b2c-custom-policy#azure-ad-b2c-extensions-app)

After custom policy was deployed, you can test the custom policy with the [SAML Test Service](https://samltestapp2.azurewebsites.net/).

In the [policy](policy) folder are the final custom policy files created following the steps above. In order to use them, following placeholders needs to be replaced with actual values:

- `yourtenant.onmicrosoft.com` needs to be replaced with your actual tenant name
- `B2CExtensionsAppAppId` and `B2CExtensionsAppObjectId` with values of b2c-extensions-app from App Registrations in Azure AD B2C
- `ProxyIdentityExperienceFrameworkAppId` and `IdentityExperienceFrameworkAppId` with application ids of IdentityExperienceFramework and ProxyIdentityExperienceFramework applications from App Registrations in Azure AD B2C

> **Note**: Custom claim (a.k.a. extension attribute) is not created in the directory when it is defined. The first time the AAD technical profile persists the claim to the directory, it checks whether the custom claim exists. If it doesn't, it creates the custom claim.

> **Note**: Custom claims (a.k.a. extension attributes) are not visible within user properties in Azure AD B2C in Azure Portal. You can use [Microsoft Graph](https://learn.microsoft.com/en-us/graph/api/user-get?view=graph-rest-1.0&tabs=http) or [.NET Core console app](https://github.com/Azure-Samples/ms-identity-dotnetcore-b2c-account-management) to manage custom claims.

> **Note**: Existing user profile attributes (e.g. `city`) can be used only if they are listed in the [documentation](https://learn.microsoft.com/en-us/azure/active-directory-b2c/user-profile-attributes#microsoft-entra-user-resource-type). Other attributes (e.g. `employeeId`) visible in user properties in Azure AD B2C user blade but not listen in [documentation](https://learn.microsoft.com/en-us/azure/active-directory-b2c/user-profile-attributes#microsoft-entra-user-resource-type) are not supported.

## Manage custom claims with Microsoft Graph

To get user custom claims with Microsoft Graph, you can use the following endpoint:

```http
GET https://graph.microsoft.com/beta/users/{id | userPrincipalName}
```

To modify user custom claims with Microsoft Graph, you can use the following endpoint:

```http
PATCH https://graph.microsoft.com/beta/users/{id | userPrincipalName}
```

where body is following JSON:

```json
{
	"extension_{appId}_customClaim": "value"
}
```
> **Note**: Replace `{appId}` with the application id of the b2c-extensions-app from App Registrations in Azure AD B2C. Application id must not contain `-` in extension name.

## Resources

- [SAML Test Service](https://samltestapp2.azurewebsites.net/)
- [Claim names for different protocols](https://learn.microsoft.com/en-us/entra/identity-platform/optional-claims#directory-extension-formatting)
- [Manage Azure AD B2C with Microsoft Graph](https://learn.microsoft.com/en-gb/azure/active-directory-b2c/microsoft-graph-operations)
- [Azure AD B2C user account management with .NET Core and Microsoft Graph](https://github.com/Azure-Samples/ms-identity-dotnetcore-b2c-account-management)
- [Azure AD B2C User profile attributes](https://learn.microsoft.com/en-us/azure/active-directory-b2c/user-profile-attributes#microsoft-entra-user-resource-type)

## License

Distributed under MIT License. See [LICENSE](LICENSE) for more details.