# azurerm_backend

A role to ensure that the necessary AWS infrastructure is present/absent for an S3 remote backend for Terraform.
When creating, the S3 bucket will be created with the required permissions for Terraform. See U(https://developer.hashicorp.com/terraform/language/settings/backends/s3#s3-bucket-permissions).
The role also allow for optionally creating a DynamoDB table with the required permissions for state locking and with a partition key named LockID with type of String.

## Requirements

Azure Account with permission to create Resource group, Storage account and container, and eventually assign role to service principal.

## Role Variables

- **azurerm_backend_operation**: Whether to create or delete the infrastructure for the azurerm Terraform backend.. Choices: 'create', 'delete'. Default: 'create'.
- **azurerm_backend_resource_group_name**: The name of the resource group to create/delete. **Required**
- **azurerm_backend_location**: Azure location for the resource group. Required when creating a new resource group.
- **azurerm_backend_storage_account_name**: The name of the storage account name to be created. When not specified and if none is existing from the resource group, the role will create a new one with generated name.
- **azurerm_backend_storage_account_type**: Type of storage account. Required when creating a storage account. Valid values are: 'Premium_LRS', 'Standard_GRS', 'Standard_LRS', 'Standard_RAGRS', 'Standard_ZRS', 'Premium_ZRS', 'Standard_RAGZRS', 'Standard_GZRS'. __Default__: 'Standard_LRS'
- **azurerm_backend_container_name**: The Name of the Storage Container to create within the Storage Account. Required when __azurerm_backend_operation=create__.
- **azurerm_backend_service_principal_id**: The service principal Id used by Terraform to push Terraform state to the container. The role will assign the __Storage Blob Data Contributor__ role for the specified container to this service principal.

## Example Playbook

    - hosts: localhost
      roles:
        - role: cloud.terraform_ops.azurerm_backend
          azurerm_backend_operation: create
          azurerm_backend_resource_group_name: "StorageAccount-ResourceGroup"
          azurerm_backend_location: eastus
          azurerm_backend_storage_account_name: ansible123
          azurerm_backend_storage_account_type: Premium_LRS
          azurerm_backend_container_name: tfstate
          azurerm_backend_service_principal_id: abcdef12-123a-456b-789c-12345abcde6e

## License

GNU General Public License v3.0 or later

See [LICENCE](https://github.com/ansible-collections/cloud.terraform_ops/blob/main/LICENSE) to see the full text.

## Author Information

- Ansible Cloud Content Team
