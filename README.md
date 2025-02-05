# AWS EKS Cert Manager Terraform module

[![Labyrinth Labs logo](ll-logo.png)](https://www.lablabs.io)

We help companies build, run, deploy and scale software and infrastructure by embracing the right technologies and principles. Check out our website at https://lablabs.io/

---

![Terraform validation](https://github.com/lablabs/terraform-aws-eks-cert-manager/workflows/Terraform%20validation/badge.svg?branch=master)
[![pre-commit](https://img.shields.io/badge/pre--commit-enabled-success?logo=pre-commit&logoColor=white)](https://github.com/pre-commit/pre-commit)

## Description

A terraform module to deploy an Cert Manager on Amazon EKS cluster.

## Related Projects

Check out other [terraform kubernetes addons](https://github.com/lablabs?q=terraform-eks).

## Examples

See [Basic example](examples/basic/README.md) for further information.

## Potential issues with running terraform plan

When deploying with ArgoCD application, Kubernetes terraform provider requires access to Kubernetes cluster API during plan time. This introduces potential issue when you want to deploy the cluster with this addon at the same time, during the same Terraform run.

To overcome this issue, the module deploys the ArgoCD application object using the Helm provider, which does not require API access during plan. If you want to deploy the application using this workaround, you can set the `argo_application_use_helm` variable to `true`.

<!-- BEGINNING OF PRE-COMMIT-TERRAFORM DOCS HOOK -->
## Requirements

| Name | Version |
|------|---------|
| <a name="requirement_terraform"></a> [terraform](#requirement\_terraform) | >= 0.14 |
| <a name="requirement_aws"></a> [aws](#requirement\_aws) | >= 2.0 |
| <a name="requirement_helm"></a> [helm](#requirement\_helm) | >= 1.0.0 |
| <a name="requirement_kubernetes"></a> [kubernetes](#requirement\_kubernetes) | >= 2.6 |
| <a name="requirement_time"></a> [time](#requirement\_time) | >= 0.6 |
| <a name="requirement_utils"></a> [utils](#requirement\_utils) | >= 0.14.0 |

## Modules

No modules.

## Resources

| Name | Type |
|------|------|
| [aws_iam_policy.this](https://registry.terraform.io/providers/hashicorp/aws/latest/docs/resources/iam_policy) | resource |
| [aws_iam_role.this](https://registry.terraform.io/providers/hashicorp/aws/latest/docs/resources/iam_role) | resource |
| [aws_iam_role_policy_attachment.this](https://registry.terraform.io/providers/hashicorp/aws/latest/docs/resources/iam_role_policy_attachment) | resource |
| [aws_iam_role_policy_attachment.this_additional](https://registry.terraform.io/providers/hashicorp/aws/latest/docs/resources/iam_role_policy_attachment) | resource |
| [helm_release.argocd_application](https://registry.terraform.io/providers/hashicorp/helm/latest/docs/resources/release) | resource |
| [helm_release.default_cluster_issuer](https://registry.terraform.io/providers/hashicorp/helm/latest/docs/resources/release) | resource |
| [helm_release.this](https://registry.terraform.io/providers/hashicorp/helm/latest/docs/resources/release) | resource |
| [kubernetes_manifest.this](https://registry.terraform.io/providers/hashicorp/kubernetes/latest/docs/resources/manifest) | resource |
| [time_sleep.default_cluster_issuer](https://registry.terraform.io/providers/hashicorp/time/latest/docs/resources/sleep) | resource |
| [aws_iam_policy_document.this](https://registry.terraform.io/providers/hashicorp/aws/latest/docs/data-sources/iam_policy_document) | data source |
| [aws_iam_policy_document.this_irsa](https://registry.terraform.io/providers/hashicorp/aws/latest/docs/data-sources/iam_policy_document) | data source |
| [utils_deep_merge_yaml.argo_application_values](https://registry.terraform.io/providers/cloudposse/utils/latest/docs/data-sources/deep_merge_yaml) | data source |
| [utils_deep_merge_yaml.default_cluster_issuer_values](https://registry.terraform.io/providers/cloudposse/utils/latest/docs/data-sources/deep_merge_yaml) | data source |
| [utils_deep_merge_yaml.values](https://registry.terraform.io/providers/cloudposse/utils/latest/docs/data-sources/deep_merge_yaml) | data source |

## Inputs

| Name | Description | Type | Default | Required |
|------|-------------|------|---------|:--------:|
| <a name="input_cluster_identity_oidc_issuer"></a> [cluster\_identity\_oidc\_issuer](#input\_cluster\_identity\_oidc\_issuer) | The OIDC Identity issuer for the cluster | `string` | n/a | yes |
| <a name="input_cluster_identity_oidc_issuer_arn"></a> [cluster\_identity\_oidc\_issuer\_arn](#input\_cluster\_identity\_oidc\_issuer\_arn) | The OIDC Identity issuer ARN for the cluster that can be used to associate IAM roles with a service account | `string` | n/a | yes |
| <a name="input_argo_application_enabled"></a> [argo\_application\_enabled](#input\_argo\_application\_enabled) | If set to true, the module will be deployed as ArgoCD application, otherwise it will be deployed as a Helm release | `bool` | `false` | no |
| <a name="input_argo_application_use_helm"></a> [argo\_application\_use\_helm](#input\_argo\_application\_use\_helm) | If set to true, the ArgoCD Application manifest will be deployed using Kubernetes provider as a Helm release. Otherwise it'll be deployed as a Kubernetes manifest. See Readme for more info | `bool` | `false` | no |
| <a name="input_argo_application_values"></a> [argo\_application\_values](#input\_argo\_application\_values) | Value overrides to use when deploying argo application object with helm | `string` | `""` | no |
| <a name="input_argo_destionation_server"></a> [argo\_destionation\_server](#input\_argo\_destionation\_server) | Destination server for ArgoCD Application | `string` | `"https://kubernetes.default.svc"` | no |
| <a name="input_argo_info"></a> [argo\_info](#input\_argo\_info) | ArgoCD info manifest parameter | `list` | <pre>[<br>  {<br>    "name": "terraform",<br>    "value": "true"<br>  }<br>]</pre> | no |
| <a name="input_argo_namespace"></a> [argo\_namespace](#input\_argo\_namespace) | Namespace to deploy ArgoCD application CRD to | `string` | `"argo"` | no |
| <a name="input_argo_project"></a> [argo\_project](#input\_argo\_project) | ArgoCD Application project | `string` | `"default"` | no |
| <a name="input_argo_sync_policy"></a> [argo\_sync\_policy](#input\_argo\_sync\_policy) | ArgoCD syncPolicy manifest parameter | `map` | `{}` | no |
| <a name="input_cluster_issuer_enabled"></a> [cluster\_issuer\_enabled](#input\_cluster\_issuer\_enabled) | Variable indicating whether default ClusterIssuer CRD is enabled | `bool` | `false` | no |
| <a name="input_cluster_issuer_settings"></a> [cluster\_issuer\_settings](#input\_cluster\_issuer\_settings) | Additional settings which will be passed to the Helm chart cluster\_issuer values, see https://github.com/lablabs/terraform-aws-eks-cert-manager/blob/main/helm/defaultClusterIssuer/values.yaml | `map(any)` | `{}` | no |
| <a name="input_cluster_issuers_values"></a> [cluster\_issuers\_values](#input\_cluster\_issuers\_values) | Additional values for cert manager cluster issuers helm chart. Values will be merged, in order, as Helm does with multiple -f options | `string` | `""` | no |
| <a name="input_enabled"></a> [enabled](#input\_enabled) | Variable indicating whether deployment is enabled | `bool` | `true` | no |
| <a name="input_helm_chart_name"></a> [helm\_chart\_name](#input\_helm\_chart\_name) | Helm chart name to be installed | `string` | `"cert-manager"` | no |
| <a name="input_helm_chart_version"></a> [helm\_chart\_version](#input\_helm\_chart\_version) | Version of the Helm chart | `string` | `"1.5.3"` | no |
| <a name="input_helm_create_namespace"></a> [helm\_create\_namespace](#input\_helm\_create\_namespace) | Whether to create k8s namespace with name defined by `k8s_namespace` | `bool` | `true` | no |
| <a name="input_helm_release_name"></a> [helm\_release\_name](#input\_helm\_release\_name) | Helm release name | `string` | `"cert-manager"` | no |
| <a name="input_helm_repo_url"></a> [helm\_repo\_url](#input\_helm\_repo\_url) | Helm repository | `string` | `"https://charts.jetstack.io"` | no |
| <a name="input_k8s_assume_role_arns"></a> [k8s\_assume\_role\_arns](#input\_k8s\_assume\_role\_arns) | Allow IRSA to assume specified role arns. Assume role must be enabled. | `list(string)` | `[]` | no |
| <a name="input_k8s_assume_role_enabled"></a> [k8s\_assume\_role\_enabled](#input\_k8s\_assume\_role\_enabled) | Whether IRSA is allowed to assume role defined by k8s\_assume\_role\_arn. Useful for hosted zones in another AWS account. | `bool` | `false` | no |
| <a name="input_k8s_irsa_additional_policies"></a> [k8s\_irsa\_additional\_policies](#input\_k8s\_irsa\_additional\_policies) | Map of the additional policies to be attached to default role. Where key is arbiraty id and value is policy arn. | `map(string)` | `{}` | no |
| <a name="input_k8s_irsa_policy_enabled"></a> [k8s\_irsa\_policy\_enabled](#input\_k8s\_irsa\_policy\_enabled) | Whether to create opinionated policy to allow operations on specified zones in `policy_allowed_zone_ids`. | `bool` | `true` | no |
| <a name="input_k8s_irsa_role_create"></a> [k8s\_irsa\_role\_create](#input\_k8s\_irsa\_role\_create) | Whether to create IRSA role and annotate service account | `bool` | `true` | no |
| <a name="input_k8s_irsa_role_name_prefix"></a> [k8s\_irsa\_role\_name\_prefix](#input\_k8s\_irsa\_role\_name\_prefix) | The IRSA role name prefix for prometheus | `string` | `"cert-manager-irsa"` | no |
| <a name="input_k8s_namespace"></a> [k8s\_namespace](#input\_k8s\_namespace) | The K8s namespace in which the external-dns will be installed | `string` | `"kube-system"` | no |
| <a name="input_k8s_rbac_create"></a> [k8s\_rbac\_create](#input\_k8s\_rbac\_create) | Whether to create and use RBAC resources | `bool` | `true` | no |
| <a name="input_k8s_service_account_create"></a> [k8s\_service\_account\_create](#input\_k8s\_service\_account\_create) | Whether to create Service Account | `bool` | `true` | no |
| <a name="input_k8s_service_account_name"></a> [k8s\_service\_account\_name](#input\_k8s\_service\_account\_name) | The k8s cert-manager service account name | `string` | `"cert-manager"` | no |
| <a name="input_policy_allowed_zone_ids"></a> [policy\_allowed\_zone\_ids](#input\_policy\_allowed\_zone\_ids) | List of the Route53 zone ids for service account IAM role access | `list(string)` | <pre>[<br>  "*"<br>]</pre> | no |
| <a name="input_settings"></a> [settings](#input\_settings) | Additional settings which will be passed to the Helm chart values, see https://artifacthub.io/packages/helm/cert-manager/cert-manager | `map(any)` | `{}` | no |
| <a name="input_tags"></a> [tags](#input\_tags) | AWS resources tags | `map(string)` | `{}` | no |
| <a name="input_values"></a> [values](#input\_values) | Additional values for cert manager helm chart. Values will be merged, in order, as Helm does with multiple -f options | `string` | `""` | no |

## Outputs

| Name | Description |
|------|-------------|
| <a name="output_helm_release_application_metadata"></a> [helm\_release\_application\_metadata](#output\_helm\_release\_application\_metadata) | Argo application helm release attributes |
| <a name="output_helm_release_metadata"></a> [helm\_release\_metadata](#output\_helm\_release\_metadata) | Helm release attributes |
| <a name="output_iam_role_attributes"></a> [iam\_role\_attributes](#output\_iam\_role\_attributes) | Prometheus IAM role atributes |
| <a name="output_kubernetes_application_attributes"></a> [kubernetes\_application\_attributes](#output\_kubernetes\_application\_attributes) | Argo kubernetes manifest attributes |
<!-- END OF PRE-COMMIT-TERRAFORM DOCS HOOK -->

## Contributing and reporting issues

Feel free to create an issue in this repository if you have questions, suggestions or feature requests.

### Validation, linters and pull-requests

We want to provide high quality code and modules. For this reason we are using
several [pre-commit hooks](.pre-commit-config.yaml) and
[GitHub Actions workflow](.github/workflows/main.yml). A pull-request to the
master branch will trigger these validations and lints automatically. Please
check your code before you will create pull-requests. See
[pre-commit documentation](https://pre-commit.com/) and
[GitHub Actions documentation](https://docs.github.com/en/actions) for further
details.


## License

[![License](https://img.shields.io/badge/License-Apache%202.0-blue.svg)](https://opensource.org/licenses/Apache-2.0)

See [LICENSE](LICENSE) for full details.

    Licensed to the Apache Software Foundation (ASF) under one
    or more contributor license agreements.  See the NOTICE file
    distributed with this work for additional information
    regarding copyright ownership.  The ASF licenses this file
    to you under the Apache License, Version 2.0 (the
    "License"); you may not use this file except in compliance
    with the License.  You may obtain a copy of the License at

      https://www.apache.org/licenses/LICENSE-2.0

    Unless required by applicable law or agreed to in writing,
    software distributed under the License is distributed on an
    "AS IS" BASIS, WITHOUT WARRANTIES OR CONDITIONS OF ANY
    KIND, either express or implied.  See the License for the
    specific language governing permissions and limitations
    under the License.
