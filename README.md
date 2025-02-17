# Openshift 3.11 on AWS via Terraform

Deploy Openshift Container Platform (OCP) v3.11 to Amazon Web Services (AWS) via Terraform

- Creates a basic OCP v3.11 cluster with a single master node, single compute node, and a bastion.

# To Use
- Install and configure awscli
  ```bash
  curl -OL https://raw.githubusercontent.com/tosin2013/openshift-4-deployment-notes/master/aws/configure-aws-cli.sh
  chmod +x configure-aws-cli.sh
  ./configure-aws-cli.sh -i 
  ```

- Install [Terraform](https://www.terraform.io/downloads.html)
  ```bash
  terraform --version
  Terraform v0.15.4
  on linux_amd64
  + provider registry.terraform.io/hashicorp/aws v3.42.0
  + provider registry.terraform.io/hashicorp/null v3.1.0
  + provider registry.terraform.io/hashicorp/template v2.2.0
  ```

- (Optional) Copy/rename `terraform.tfvars.example` to `terraform.tfvars` and fill in the information (otherwise these will be prompted on apply):
  ```bash
  mv terraform.tfvars.example terraform.tfvars
  ```

- Initialize and test the Terraform configuration.
  ```bash
  terraform init && terraform plan
  ```

- Initialize and apply the Terraform configuration. Provide verification to deploy OCP v3.11 (add `-auto-approve` to apply without user verification):
  ```bash
  terraform init && terraform apply
  ```
- The Terraform output provides access credentials for the cluster. 
  (NOTE: You can administer the cluster directly by SSH-ing to the Bastion and then SSH-ing to the Master, where `oc` is already configured and logged in with the default Cluster Administrator, `system:admin`):
  - To see all output:
    ```bash
    terraform output
    ```
  - To see only one output (handy for copy/paste or scripts):
    ```bash
    terraform output <variable>
    ```
  - Example: _SSH directly to Master node through Bastion_
    ```bash
    $(terraform output bastion_ssh) -A -t ssh $(terraform output private_dns_master)
    ```
- To destroy the cluster and its resources:
  ```bash
  terraform destroy
  ```