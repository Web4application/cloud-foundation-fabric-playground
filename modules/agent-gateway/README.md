# Agent Gateway

The module facilitates the deployments of Agent Gateways.

<!-- BEGIN TOC -->
- [Minimal Gateway deployment](#minimal-gateway-deployment)
- [PSC-I: attach to an existing service attachment](#psc-i-attach-to-an-existing-service-attachment)
- [Connect to self-managed proxies](#connect-to-self-managed-proxies)
- [Variables](#variables)
- [Outputs](#outputs)
<!-- END TOC -->

## Minimal Gateway deployment

In order to deploy a gateway, you need to specify a name, a region and the direction it needs to apply to.

```hcl
module "agent-gateway" {
  source      = "./fabric/modules/agent-gateway"
  name        = "my-gateway"
  project_id  = "my-project-id"
  region      = "europe-west1"
  access_path = "CLIENT_TO_AGENT" # can be also: ingress, or egress (or AGENT_TO_ANYWHERE)
}
# tftest inventory=minimal.yaml
```

## PSC-I: attach to an existing service attachment

If it's a egress (or AGENT_TO_ANYWHERE) agent, you can attach with a PSC interface to an existing service attachment.

```hcl
module "agent-gateway" {
  source      = "./fabric/modules/agent-gateway"
  name        = "my-gateway"
  project_id  = "my-project-id"
  region      = "europe-west1"
  access_path = "AGENT_TO_ANYWHERE"
  networking_config = {
    psc_i_network_attachment_id = "projects/my-project-id/regions/europe-west1/serviceAttachments/my-sa"
  }
}
# tftest inventory=psc-i.yaml
```

## Connect to self-managed proxies

You can connect to compatible proxies you manage, by specifying the proxy uri.

```hcl
module "agent-gateway" {
  source            = "./fabric/modules/agent-gateway"
  name              = "my-gateway"
  project_id        = "my-project-id"
  region            = "europe-west1"
  is_google_managed = false
  proxy_uri         = "my-proxy-uri"
}
# tftest inventory=proxy.yaml
```
<!-- BEGIN TFDOC -->
## Variables

| name | description | type | required | default |
|---|---|:---:|:---:|:---:|
| [access_path](variables.tf#L17) | The direction the gateway applies to: ingress (CLIENT_TO_AGENT) or egress (AGENT_TO_ANYWHERE) (if var.is_google_managed = false). | <code>string</code> | ✓ |  |
| [name](variables.tf#L65) | The name of the Agent Gateway. | <code>string</code> | ✓ |  |
| [project_id](variables.tf#L81) | The ID of the project where the data stores and the agents will be created. | <code>string</code> | ✓ |  |
| [region](variables.tf#L117) | The region where the agent gateway is created. | <code>string</code> | ✓ |  |
| [description](variables.tf#L46) | The description of the Agent Gateway. | <code>string</code> |  | <code>&#34;Terraform managed.&#34;</code> |
| [is_google_managed](variables.tf#L52) | Whether the Agent Gateway is Google or self-managed. | <code>bool</code> |  | <code>true</code> |
| [labels](variables.tf#L59) | Labels to associate to the Agent Gateway. | <code>map&#40;string&#41;</code> |  | <code>null</code> |
| [networking_config](variables.tf#L72) | The Agent Gateway networking configuration. | <code>object&#40;&#123;&#8230;&#125;&#41;</code> |  | <code>&#123;&#125;</code> |
| [protocols](variables.tf#L87) | The protocols managed by the Agent Gateway. | <code>list&#40;string&#41;</code> |  | <code>&#91;&#34;MCP&#34;&#93;</code> |
| [proxy_uri](variables.tf#L103) | The uri of a compatible self-managed proxy (if var.is_google_managed = false). | <code></code> |  | <code>null</code> |
| [registries](variables.tf#L123) | A list of Agent Registries containing the agents, MCP servers and tools governed by the Agent Gateway. Note: Currently limited to project-scoped registries Must be of format //agentregistry.googleapis.com/{version}/projects/{{project}}/locations/{{location}} | <code>list&#40;string&#41;</code> |  | <code>null</code> |

## Outputs

| name | description | sensitive |
|---|---|:---:|
| [agent_gateway](outputs.tf#L17) | The Agent Gateway object. |  |
| [agent_gateway_card](outputs.tf#L22) | The Agent Gateway card, including information about mTLS endpoints, root certificates and service extension service account. |  |
| [id](outputs.tf#L27) | The Agent Gateway id. |  |
<!-- END TFDOC -->
