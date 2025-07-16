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
3. Register your subscription with Azure Lighthouse for GK management only after the managed identity is configured
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

## Template Structure and Implementation

The template has been structured to ensure reliable deployment across different environments. Here's how it works:

1. **Resource Group Creation**:
   - Creates a temporary resource group to contain the managed identity

2. **Three-Phase Deployment Approach**:
   - First deployment (`identityDeployment`): Creates the user-assigned managed identity in the resource group
   - Second deployment (`roleAssignment`): Assigns the Key Vault Data Administrator role to the managed identity at the subscription level
   - Third deployment (`lighthouseDeployment`): Registers your subscription with Azure Lighthouse for GK management only after the managed identity has been properly configured

3. **Dependency Chain**:
   - The Lighthouse registration is deliberately made dependent on the role assignment deployment
   - This ensures that the managed identity is fully configured before granting GK access to your subscription
   - This sequence provides assurance that all required resources are properly set up before external access is granted

4. **Properly Scoped References**:
   - Uses nested deployments to handle resource group and subscription-level resources
   - Implements proper expression evaluation scope for nested templates
   - Maintains proper dependencies between resources

This approach ensures that resources are created in the correct order and that references between resources are properly resolved during deployment.

## Important Note on Deployment Sequence

This template has been structured to make the Azure Lighthouse onboarding dependent on the successful deployment of the managed identity and its role assignment. This means:

1. **Benefits**:
   - Ensures that all resources are properly deployed and configured before granting GK access
   - Provides a checkpoint that verifies all permissions are correctly set up
   - Follows a principle of setting up internal resources before enabling external access

2. **Potential Considerations**:
   - If the managed identity creation or role assignment fails, the Lighthouse onboarding will not proceed
   - This is a deliberate safeguard to ensure that deployments are all-or-nothing
   - For troubleshooting, check deployment logs for the identity and role assignment steps first

This sequence was chosen to ensure that your environment is fully prepared before external management access is granted.
