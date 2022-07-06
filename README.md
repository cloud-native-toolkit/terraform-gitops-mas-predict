#  Maximo Application Suite - MAS Predict Application Gitops terraform module

Deploys MAS Predict application as part of Maximo Application Suite via gitops.  To run, download the BOM (Bill of Materials) from the module catalog and build the terraform from there.  Specify the MAS-Core instance id - in the `instanceid` variable.  This will create a namespace of the name `mas-(instanceid)-predict`.

Note if your cluster is not setup for gitops, download the gitops bootstrap BOM from the module catalog first to setup the gitops tooling.

## Pre-Requisites
MAS-Predict requires `DB2wh`, `studio`, `health`, `monitor`, and `ML` to already be installed and configured in order to install successfully.  Both Studio and ML are part of CP4D.  Be sure to define the appropriate `scope` for those resources in the module variables if the defaults are not correct.

## Supported platforms

- OCP 4.8+

## Suggested companion modules

The module itself requires some information from the cluster and needs a
namespace to be created. The following companion
modules can help provide the required information:

- Gitops:  github.com/cloud-native-toolkit/terraform-tools-gitops
- Gitops Bootstrap: github.com/cloud-native-toolkit/terraform-util-gitops-bootstrap
- Pull Secret:  github.com/cloud-native-toolkit/terraform-gitops-pull-secret
- Catalog: github.com/cloud-native-toolkit/terraform-gitops-cp-catalogs 
- Cert:  github.com/cloud-native-toolkit/terraform-util-sealed-secret-cert
- Cluster: github.com/cloud-native-toolkit/terraform-ocp-login
- CertManager: github.com/cloud-native-toolkit/terraform-gitops-ocp-cert-manager
- JDBC: github.com/cloud-native-toolkit/terraform-gitops-mas-jdbc

## Example usage

```hcl-terraform
module "mas_predict" {
  source = "github.com/cloud-native-toolkit/terraform-gitops-mas-predict"

  gitops_config = module.gitops.gitops_config
  git_credentials = module.gitops.git_credentials
  server_name = module.gitops.server_name
  kubeseal_cert = module.gitops.sealed_secrets_cert

  instanceid = module.mas_kafka.instanceid
  workspace_id = "demo"
  entitlement_key = module.cp_catalogs.entitlement_key
  jdbc_scope = module.masjdbc.scope
 
}
```