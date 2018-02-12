# Terraform with Azure Government
This repository shows you how to use Terraform to manage resources in Azure Government.

> **Important Note**
> This sample embeds the azurerm provider configuration in the terraform file itself.
> This isn't a best practice, especially if checking in these files into a source code repository.

## Instructions

1. Create your own copy of terraform-sample.tf

    ```bash
    cp terraform-sample.tf.template terraform-sample.tf
    ```

2. Log in to Azure Government with the Azure CLI 

    ```bash
    # Log in
    az cloud set -n AzureUSGovernment
    az login
    ```

3. Create an Azure AD service principal for Terraform and get all the values required for Terraform to communicate with Azure Government

    ```bash
    # Get your subscription
    SUBSCRIPTION=$(az account show --query id -o tsv)
    TENANT_ID=$(az account show --query tenantId -o tsv)

    echo "Subscription: $SUBSCRIPTION"
    echo "TenantId: $TENANT_ID"

    # Create an Azure AD Service Principal for Terraform
    # NOTE: You'll need appId and the password
    # from the output of this command.
    az ad sp create-for-rbac -n Terraform -role Contributor -scopes /subscriptions/$SUBSCRIPTION
    ```

3. Update the values in the `provider "azure"` section of `terraform-sample.tf`.

4. Deploy to Azure Government using Terraform.

    ```bash
    terraform apply
    ```

## References

https://www.terraform.io/docs/providers/azurerm/