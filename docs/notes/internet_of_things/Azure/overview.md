# Azure Cloud Notes

## Azure Active Directory

Azure Active Directory (Azure AD) is Microsoft’s cloud-based identity and access management service, which helps your employees sign in and access resources in:

* External resources, such as Microsoft 365, the Azure portal, and thousands of other SaaS applications.

* Internal resources, such as apps on your corporate network and intranet, along with any cloud apps developed by your own organization. For more information about creating a tenant for your organization, see Quickstart: Create a new tenant in Azure Active Directory.

## Azure Tenant

A dedicated and trusted instance of Azure AD that's automatically created when your organization signs up for a Microsoft cloud service subscription, such as Microsoft Azure, Microsoft Intune, or Microsoft 365. An Azure tenant represents a single organization.

## Azure Subscription

Used to pay for Azure cloud services. You can have many subscriptions and they're linked to a credit card.

> More terms [here](https://docs.microsoft.com/en-us/azure/active-directory/fundamentals/active-directory-whatis#:~:text=Azure%20tenant,tenant%20represents%20a%20single%20organization)

## Service Principal Object

To access resources that are secured by an Azure AD tenant, the entity that requires access must be represented by a security principal. This requirement is true for both **users** (user principal) and **applications** (service principal). The security principal defines the access policy and permissions for the user/application in the Azure AD tenant. This enables core features such as authentication of the user/application during sign-in, and authorization during resource access.

There are three types of service principal: **application**, **managed identity**, and **legacy**.

* Application: The first type of service principal is the local representation, or application instance, of a global application object in a single tenant or directory. In this case, a service principal is a concrete instance created from the application object and inherits certain properties from that application object. A service principal is created in each tenant where the application is used and references the globally unique app object. The service principal object defines what the app can actually do in the specific tenant, who can access the app, and what resources the app can access. When an application is given permission to access resources in a tenant (upon registration or consent), a service principal object is created.

* Managed Identity: The second type of service principal is used to represent a managed identity. Managed identities eliminate the need for developers to manage credentials. Managed identities provide an identity for applications to use when connecting to resources that support Azure AD authentication. When a managed identity is enabled, a service principal representing that managed identity is created in your tenant. **Service principals representing managed identities can be granted access and permissions, but cannot be updated or modified directly.**

* Legacy: The third type of service principal represents a legacy app (an app created before app registrations were introduced or created through legacy experiences). A legacy service principal can have credentials, service principal names, reply URLs, and other properties which are editable by an authorized user, but does not have an associated app registration. **The service principal can only be used in the tenant where it was created.**