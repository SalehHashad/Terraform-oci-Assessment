# terraform-oci-arch-web-ha

[![License: UPL](https://img.shields.io/badge/license-UPL-green)](https://img.shields.io/badge/license-UPL-green) [![Quality gate](https://sonarcloud.io/api/project_badges/quality_gate?project=oracle-devrel_terraform-oci-arch-web-ha)](https://sonarcloud.io/dashboard?id=oracle-devrel_terraform-oci-arch-web-ha)

Web applications typically include a web client, servers, and a datastore. Flask is a popular Python framework for developing web applications quickly and easily.

This reference architecture uses a sample Flask application that interacts with an Oracle Database on the backend. The application is containerized using Docker, and the reference architecture is deployed to Oracle Cloud Infrastructure.

## Prerequisites

- Permission to `manage` the following types of resources in your Oracle Cloud Infrastructure tenancy: `vcns`, `internet-gateways`, `route-tables`, `network-security-groups`, `subnets`, `autonomous-database-family`, and `instances`.

- Quota to create the following resources: 1 VCN, 2 subnets, 1 Internet Gateway, 1 NAT Gateway, 2 route rules, 1 ATP database instance, and 2 compute instances.

If you don't have the required permissions and quota, contact your tenancy administrator. See [Policy Reference](https://docs.cloud.oracle.com/en-us/iaas/Content/Identity/Reference/policyreference.htm), [Service Limits](https://docs.cloud.oracle.com/en-us/iaas/Content/General/Concepts/servicelimits.htm), [Compartment Quotas](https://docs.cloud.oracle.com/iaas/Content/General/Concepts/resourcequotas.htm).

## Deploy Using Oracle Resource Manager

1. Click [![Deploy to Oracle Cloud](https://oci-resourcemanager-plugin.plugins.oci.oraclecloud.com/latest/deploy-to-oracle-cloud.svg)](https://cloud.oracle.com/resourcemanager/stacks/create?region=home&zipUrl=https://github.com/oracle-devrel/terraform-oci-arch-web-ha/releases/latest/download/terraform-oci-arch-web-ha-stack-latest.zip)

    If you aren't already signed in, when prompted, enter the tenancy and user credentials.

2. Review and accept the terms and conditions.

3. Select the region where you want to deploy the stack.

4. Follow the on-screen prompts and instructions to create the stack.

5. After creating the stack, click **Terraform Actions**, and select **Plan**.

6. Wait for the job to be completed, and review the plan.

    To make any changes, return to the Stack Details page, click **Edit Stack**, and make the required changes. Then, run the **Plan** action again.

7. If no further changes are necessary, return to the Stack Details page, click **Terraform Actions**, and select **Apply**. 

## Deploy Using the Terraform CLI

### Clone the Repository
Now, you'll want a local copy of this repo. You can make that with the commands:

    git clone https://github.com/oracle-devrel/terraform-oci-arch-web-ha.git
    cd terraform-oci-arch-web-ha
    ls

2. Create a `terraform.tfvars` file, and specify the following variables:

```
# Authentication
tenancy_ocid         = "<tenancy_ocid>"
user_ocid            = "<user_ocid>"
fingerprint          = "<finger_print>"
private_key_path     = "<pem_private_key_path>"

# SSH Keys
ssh_public_key       = "<contents_of_public_ssh_key>"
### USE ONE ^ OR THE OTHER v
ssh_public_key_path  = "<public_ssh_key_path>"

# database
ATP_password           = "<ATP_user_password>"
ATP_data_guard_enabled = false # set the value to true only when you want to enable standby and then re-run terraform apply

# Region
region = "<oci_region>"

# Availability Domain 
availability_domain_number = "<availability_domain_number>"
### USE ONE ^ OR THE OTHER v
availability_domain_name = "<availability_domain_name>"

# Compartment
compartment_ocid = "<compartment_ocid>"
```

### Create the Resources
Run the following commands:

    terraform init
    terraform plan
    terraform apply

### Destroy the Deployment
When you no longer need the deployment, you can run this command to destroy the resources:

    terraform destroy

## Deploy as a Module
It's possible to utilize this as a module, providing the necessary inputs:

```
module "terraform-oci-arch-web-ha" {
  source                      = "github.com/oracle-devrel/terraform-oci-arch-web-ha"
  tenancy_ocid                = "<tenancy_ocid>"
  user_ocid                   = "<user_ocid>"
  fingerprint                 = "<finger_print>"
  region                      = "<oci_region>"
  availability_domain_number  = "<availability_domain_number>"
  compartment_ocid            = "<compartment_ocid>"
  private_key_path            = "<pem_private_key_path>"

  # SSH Keys
  ssh_public_key              = "<contents_of_public_ssh_key>"
  ### USE ONE ^ OR THE OTHER v
  ssh_public_key_path         = "<public_ssh_key_path>"

  ATP_password                = "<ATP_user_password>"
  ATP_data_guard_enabled      = false # set the value to true only when you want to enable standby and then re-run terraform apply
}
```

## URLs
For details of the architecture, see [_Deploy a highly available web application_](https://docs.oracle.com/en/solutions/ha-web-app/index.html)

## Contributing
This project is open source.  Please submit your contributions by forking this repository and submitting a pull request!  Oracle appreciates any contributions that are made by the open source community.

## License
Copyright (c) 2021 Oracle and/or its affiliates.

Licensed under the Universal Permissive License (UPL), Version 1.0.

See [LICENSE](LICENSE) for more details.
