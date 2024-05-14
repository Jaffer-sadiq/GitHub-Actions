# Lab 4: Security and best practices

### Task 1: OIDC to securely connect to the cloud

OpenID Connect (OIDC) allows your GitHub Actions workflows to access resources in Azure, without needing to store the Azure credentials as long-lived GitHub secrets. This gives an overview of how to configure Azure to trust GitHub's OIDC as a federated identity, and includes a workflow example for the azure/login action that uses tokens to authenticate to Azure and access resources.

```
{
    "clientSecret": "******",
    "subscriptionId": "******",
    "tenantId": "******",
    "clientId": "******"
}
```

```
# File: .github/workflows/workflow.yml 

on: [push]

name: Run Azure Login With a Service Principal Secret

jobs:

  build-and-deploy:
    runs-on: ubuntu-latest
    steps:

    - uses: azure/login@v2
      with:
        creds: ${{ secrets.AZURE_CREDENTIALS }}
        enable-AzPSSession: true

    - name: Azure CLI script
      uses: azure/cli@v2
      with:
        azcliversion: latest
        inlineScript: |
          az account show

    - name: Azure PowerShell script
      uses: azure/powershell@v2
      with:
        azPSVersion: "latest"
        inlineScript: |
          Get-AzContext
```

### Task 2: Agent infrastructure
