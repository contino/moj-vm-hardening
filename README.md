# moj-vm-hardening

## Description

This repo contains the scripts and config files to create a hardened CentOS image.
It applies the hardening standards as described here: (https://docs.openstack.org/ansible-hardening/latest/).


## Requirements

* Packer 1.1.0
* Ansible 2.4.0

## Variables

| Name           | Default Value | Description                        | Required |
| -------------- | ------------- | -----------------------------------| -------- |
| `basename`| centos-7-4-x86-64 | the base name for the created image| no |
| `version`| {{isotime \"2006-01-02\"}} | the current date, appended to the image name | no |
| `revision`| 1 | default revision number, can be incremented | no |
| `size`| 8 | required volume size (currently unused) | no |
| `azure_subscription_id`| null | Azure subscription ID | yes |
| `azure_tenant_id`| null | Azure tenant ID | yes |
| `azure_object_id`| null | Azure object ID | no |
| `azure_resource_group_name`| null | Azure resource group name | yes |
| `azure_storage_account`| null | Azure storage account name | yes |
| `azure_client_id`| null | Azure client ID | yes |
| `azure_client_secret`| null | Azure cleint secret | yes |
| `azure_location`| uksouth | Azure location | yes |
| `ssh_user`| centos | SSH username | yes |
| `ssh_password`| I@mTh3L@w! | SSH password | yes |

## Example Usage

NOTE: Before running the packer command, check out the latest `ansible-hardening` playbook with the following command:
```
git clone https://github.com/openstack/ansible-hardening
```

Once the ansible-hardening repo has been checked out, run packer with the following arguments:
```
packer build -var 'azure_client_id=<client-id>' \
             -var 'azure_client_secret=<client-secret>' \
             -var 'azure_tenant_id=<tenant-id>' \
             -var 'azure_subscription_id=<subscription-id>' \
             -var 'azure_resource_group_name=<resource-name>' \
             -var 'azure_storage_account=<storage-account>' \
             os-centos-7.4-x86_64.json
```

By default, this will update all packges on the image, install `clound-init` and then run the `ansible-hardening` role.
The resultant VHD file will be stored in the defined storage account.