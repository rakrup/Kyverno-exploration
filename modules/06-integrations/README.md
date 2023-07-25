# Integrations

[Go back of Main Menu](../../navigation.md)

Kyverno being a CNCF project integrates seamlessly with other CNCF projects. 

# Popular Integrations

Kyverno has started integrating with popular CNCF projects and releasing out the policy-sets used to integrate. Few of them are:
1. [Crossplane](./01-crossplane/README.md): [Crossplane](https://www.crossplane.io/) build control planes without needing to write code. Crossplane has a highly extensible backend that enables you to orchestrate applications and infrastructure no matter where they run, and a highly configurable frontend that lets you define the declarative API it offers. 

2. [OpenCost](./02-opencost/README.md): [OpenCost](https://www.opencost.io/) is a vendor-neutral open source project for measuring and allocating infrastructure and container costs in real time. Built by Kubernetes experts and supported by Kubernetes practitioners, OpenCost shines a light into the black box of Kubernetes spend.

**Note**: Kyverno at its heart is a JSON validating engine. So if we can provide terraform state or ECS task (or possibly any other config) definition as Json to our Kyverno controller, Kyverno policies can audit/enfore the presented configuration. 

For example, if a controller/adapter could export ECS task config, terraform config in YAML/JSON format and import into Kubernetes ETCd - Kyverno can audit and generate policy reports on these configuration of external resources. We believe Nirmata is already working to enable such usecases in near future.

[Go back of Main Menu](../../navigation.md)