# GK Azure Lighthouse onboarding templates
The provided ARM-templates will onboard your subscriptions to GK Azure Lighthouse Management. Your GK contact person will provide you with the required information (managedByTenantId, ReaderPrincipalID or ContributorPrincipalID)

## Onboard GK to your subscription with Contributor Role
[![Deploy to Azure](https://aka.ms/deploytoazurebutton)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FC4A8-CCU-Resources%2Frf-azure-lighthouse-onboarding%2Fmain%2Fgk-lighthouse-contributor.json)

## Template Features

### Azure Lighthouse Onboarding
This template onboards your subscription to Azure Lighthouse, granting the following role to GK:
- Contributor - For general management of resources

### User-Assigned Managed Identity
The template also deploys a user-assigned managed identity with the following characteristics:

- **Resource Group**: By default, the identity is created in a resource group named "RG-ReportingFoundation" (customizable via parameter)
- **Identity Name**: By default, named "mi-reportingfoundation" (customizable via parameter)
- **Permissions**: The managed identity is granted the "Key Vault Data Administrator" role at the subscription level
- **Purpose**: This identity can be used by deployment scripts to manage Key Vault secrets, keys, and certificates

### What This Deployment Creates
When you deploy this template, it will:

1. Register your subscription with Azure Lighthouse for GK management
2. Create a new resource group (if it doesn't already exist)
3. Create a user-assigned managed identity in that resource group
4. Assign the Key Vault Data Administrator role to the managed identity at the subscription scope
5. Output the resource IDs and principal IDs for future reference

## Parameters

| Parameter Name | Description |
|---------------|------------|
| mspOfferName | Name of the Lighthouse offer |
| mspOfferDescription | Description of the Lighthouse offer |
| managedByTenantId | GK's Tenant ID (provided by GK) |
| OwnerPrincipalID | Object ID of GK's Contributor Group (provided by GK) |
| resourceGroupName | Name of the resource group for the managed identity |
| location | Azure region for the resources |
| managedIdentityName | Name of the user-assigned managed identity |
| reportingFoundationSubscriptionId | Subscription ID for the Reporting Foundation |

## Prerequisites

To successfully deploy this template, the following prerequisites must be met:

### Required Roles
The user deploying this template must have the following roles:

- **Owner** or **User Access Administrator** at the subscription scope
  - Required to register the subscription with Azure Lighthouse
  - Required to create role assignments for the managed identity

### Required Information
- GK's Tenant ID (provided by your GK contact)
- GK's Contributor Group Object ID (provided by your GK contact)

### Deployment Process
1. Click the "Deploy to Azure" button above
2. Fill in the required parameters with the values provided by GK
3. Review and create the deployment
