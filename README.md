# Description

- Enables to edit Terraform file on local host and push changes to GitHub
- Azure Pipelines will trigger and automatically implement changes to Azure Cloud from Terraform file hosted at GitHub
- Secrets are stored securely in the key vault

# Steps to achieve

- Consider what permissions are required for Terraform and what scope
    - If a user has `[Contributor`permissions](https://docs.microsoft.com/en-us/azure/key-vault/general/security-features#managing-administrative-access-to-key-vault) to a key vault management plane, the user can grant themselves access to the data plane by setting a Key Vault access policy
- Create GitHub [repository](https://docs.github.com/en/get-started/quickstart/create-a-repo) and clone it locally
- Create your [Terraform file](https://registry.terraform.io/providers/hashicorp/azurerm/latest/docs) to provision Azure resources
- Create service [principal account](https://docs.microsoft.com/en-us/azure/active-directory/develop/app-objects-and-service-principals)
- Assign Azure role to [service principal](https://docs.microsoft.com/en-us/azure/role-based-access-control/role-assignments-steps)
- Create [storage account](https://docs.microsoft.com/en-us/azure/storage/common/storage-account-create?tabs=azure-portal) in Azure to store Terraform state file remotely
- Create [Azure Key Vault](https://docs.microsoft.com/en-us/azure/key-vault/general/quick-create-portal) to store your secrets
- [Store your secrets](https://docs.microsoft.com/en-us/azure/key-vault/secrets/quick-create-portal#add-a-secret-to-key-vault) in Azure Key Vault
    - [Tenant ID](https://docs.microsoft.com/en-us/azure/active-directory/fundamentals/active-directory-how-to-find-tenant), [Subscription ID, Client ID, Client Secret](https://www.cloudsnooze.com/news/view/29), [Storage Key](https://docs.microsoft.com/en-us/azure/storage/common/storage-account-keys-manage?tabs=azure-portal#view-account-access-keys)
- Create private [Azure DevOps project](https://docs.microsoft.com/en-us/azure/devops/organizations/projects/create-project?view=azure-devops&tabs=browser)
    - Initialize Azure DevOps repo from existing repository in GitHub
- Create Azure [service connection](https://docs.microsoft.com/en-us/azure/devops/pipelines/library/connect-to-azure?view=azure-devops) in Azure DevOps
- Integrate [Azure Key Vault with Azure Pipelines](https://thomasthornton.cloud/2021/06/24/storing-and-retrieving-secrets-in-azure-keyvault-with-variable-groups-in-azure-devops-pipelines/) to retrieve secrets in your pipelines
- Create your [pipeline with GitHub](https://docs.microsoft.com/en-us/azure/devops/pipelines/repos/github?view=azure-devops&tabs=yaml) to run [Terraform file](https://learn.hashicorp.com/tutorials/terraform/automate-terraform)