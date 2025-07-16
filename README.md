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

- **Resource Group**: By default, the identity is created in a resource group named "RG-ReportingFoundation-Temp" (customizable via parameter)
- **Identity Name**: By default, named "mi-reportingfoundation" (customizable via parameter)
- **Permissions**: The managed identity is granted the "Key Vault Data Administrator" role at the subscription level
- **Purpose**: This identity can be used by deployment scripts to manage Key Vault secrets, keys, and certificates

### What This Deployment Creates
When you deploy this template, it will:

1. Create a temporary resource group and deploy a user-assigned managed identity in it
2. Assign the Key Vault Data Administrator role to the managed identity at the subscription scope
3. Register your subscription with Azure Lighthouse for GK management after the managed identity is configured
4. Output the resource IDs and principal IDs for future reference

## Parameters

| Parameter Name | Description |
|---------------|------------|
| managedByTenantId | GK's Tenant ID (provided by GK) |
| OwnerPrincipalID | Object ID of GK's Contributor Group (provided by GK) |
| resourceGroupName | Name of the temporary resource group where the managed identity will be deployed |
| managedIdentityName | Name of the user-assigned managed identity |

## Fixed Values (Not Configurable)

The following values are fixed in the template and cannot be changed:

| Name | Value |
|---------------|------------|
| mspOfferName | GK Reporting Foundation Service - Access privileges |
| mspOfferDescription | The solution will grant glueckkanja AG the Contributor role to your subscription |
| location | Uses the location of the selected resource group |
| reportingFoundationSubscriptionId | Uses the current subscription ID |

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