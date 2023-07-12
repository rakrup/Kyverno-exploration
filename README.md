# Exploring Kyverno

Evaluating Kyverno and what is offers.

> **Note**
> If you are new to Kyverno - it would be good to go through a little introduction below. If you have already read about Kyverno - you can [read about usecases you could solve with Kyverno](./navigation.md).

## What is Kyverno ?

![kyverno_logo](./modules/00-prerequisites/images/kyverno%20logo.png)

1. a CNCF Incubating project
2. Graduation submission in 2023, planned graduation 2024
    a. Fuzzing and third-party security audit in progress
3. Backed by [Nirmata](https://nirmata.com)
4. Kyverno stats
   1.  **Image pulls**: 2.12 B
   2.  **GitHub stars**: ~4,100
   3.  **Slack community**: ~2,400 members across two channels/workspaces
   4.  **Monthly active contributors**: ~100+
   5.  **Maintainers**: 7+
   6.  **Releases**: 175 releases

#### Key Links: 

* [CNCF reference](https://www.cncf.io/projects/kyverno/) : Kyverno page on CNCF website - news, presentations and more!
* [kyverno](https://github.com/kyverno/kyverno): The main application repository.
* [website](https://github.com/kyverno/website): The source for the Kyverno website at https://kyverno.io.
* [policies](https://github.com/kyverno/policies): The Kyverno policy library and test cases (with web frontend at https://kyverno.io/policies).
* [policy-reporter](https://github.com/kyverno/policy-reporter): The Kyverno Policy Reporter sub-project which shows policy reports in a graphical web-based front end.
* [KDP](https://github.com/kyverno/KDP): The Kubernetes Design Proposals repository, where significant new enhancements and features are proposed.
* [demos](https://github.com/kyverno/demos): Any Kyverno demos that have been given usually store the sample policies and resources here.

## Why Kyverno ?

### Benefits of a CNCF project
What are the advantages of choosing any CNCF project?
1. Branding
2. Community involvement/support
3. Interoperability with other CNCF projects
a. Example (Kyverno x Crossplane, Kyverno x OpenCost)

### Notable Kyverno Adoption

Kyverno Adopters list - [https://github.com/kyverno/kyverno/blob/main/ADOPTERS.md](https://github.com/kyverno/kyverno/blob/main/ADOPTERS.md)
Department of Defense, USA - here is the [GitHub repo](https://github.com/DoD-Platform-One/big-bang/blob/master/docs/understanding-bigbang/package-architecture/README.md#policy-enforcement) stating Kyverno is the default policy
engine
Openshift uses Kyverno for Policy Management - [Openshift blog](https://cloud.redhat.com/blog/automate-your-security-practices-and-policies-on-openshift-with-kyverno)
[Kyverno](https://kyverno.io)/[Nirmata](https://nirmata.com) is used across the board - financial institutions, government bodies, retail
industry, healthcare, and Service Integrators.

### Deprecation of Pod Security Policy (PSP)

[PSPs were deprecated](https://kubernetes.io/blog/2021/04/06/podsecuritypolicy-deprecation-past-present-and-future/) in Kubernetes 1.21 and completely removed in Kubernetes 1.25.
1. Pod Security Standards (PSS) is the replacement for PSPs
2. Pod Security Admission (PSA) is an implementation of PSS (introduced in Kubernetes
1.25)
3. PSA has two profiles - baseline and restricted. It does not offer granular control
4. Kyverno can be used either standalone to implement all PSS standards or alongside
PSA to provide granular control to the profiles
Detailed blog post here - [Using Kyveno with Pod Security Admission](https://kyverno.io/blog/2023/06/12/using-kyverno-with-pod-security-admission/)

## Potential value for companies adopting Kyverno/Nirmata
1. Implement best practices for Kubernetes configurations
2. Consistent approach to perform security audits for multi-cluster and multi-cloud
3. Interoperability with existing open source tooling - Crossplane, Open Cost any other CNCF projects;
4. Teams using OPA can either continue to leverage existing knowledge base or move to Kyverno or continue with co-existence of both tools & its [possible integration](https://kyverno.io/blog/2023/05/30/kyverno-1.10-released/#extensibility-via-external-service-calls) 

### OPA vs Kyverno
Detailed comparison - [Kubernetes Policy Comparison: OPA Gatekeeper vs Kyverno](https://neonmirrors.net/post/2021-02/kubernetes-policy-comparison-opa-gatekeeper-vs-kyverno/)
Performance tests - [Scaling with Kyverno](https://kyverno.io/docs/installation/scaling/)

Policies Complexity in Kyverno - While authoring Kyverno policies is simpler compared to OPA Gatekeeper, one can get really creative and write complex policies.
1. JMESPath
   1.  config-syncer-secret-generation-from-rancher-capi
   2.  argo-cluster-generation-from-rancher-capi
   3.  Require-unique-uid-per-workload
2. [Make external service API calls](https://kyverno.io/blog/2023/05/30/kyverno-1.10-released/#extensibility-via-external-service-calls)

## Kyverno Open Source vs Nirmata Policy Manager
[Nirmata Policy Manager (NPM)](https://nirmata.com/nirmata-cloud-native-policy-manager/) is a SaaS offering by Nirmata. While it is powered by Kyverno,
NPM has additional capabilities for -
1. Enterprise Kyverno distribution
   1.  Long Term Support (LTS) for Kyverno and Kubernetes versions
   2.  Backport of fixes to all LTS versions (open-source supports only n-2 versions)
   3.  Assistance in curating policies relevant to various groups within the firm
   4.  Kyverno Operator for lifecycle management of Kyverno and policies
   5.  Variety of adapter integrations for AWS, Image scanning, Venafi, etc.
   6.  Tamper prevention and self-healing for policies and policy engine
   7.  Secure distribution of policies via OCI (roadmap)
2. Advanced dashboarding to provide quick summary of the security posture across your fleet of clusters
3. Built-in compliance standards for industry best practices (CIS, NIST, PCI, etc.)
4. Support for custom compliance standards mapping to policies
5. Collaboration workflows across dev, sec, and ops teams
   1. Violations assignment and namespace ownership
   2. Time based policy exceptions
   3. Policy tamper detection and prevention
6. Nirmata CLI integration in CI/CD pipelines
   1. Example - GitHub actions, Tekton CD etc.
7. Remediation suggestions for policy violations
8. Integration with service management tools like JIRA, Slack, GitHub, etc.
9.  Identity and Access Management
10. Alerting and monitoring capabilities

### NPM Roadmap
Some of the advanced feature highlights in the roadmap are -
1. Evidence gathering support for compliance failures
2. Support for IAC pipelines i.e. Terraform
3. Integration with non-Kubernetes-native use-cases
   1. Example - ECS, Fargate

4. Automated Remediation of violations
5. Integrations with image scan reports, SBOMs etc.

### Kyverno - Technical Overview
* All documentation can be found on the [official site](https://kyverno.io/docs/).
* [Latest release highlights](https://kyverno.io/blog/2023/05/30/kyverno-1.10-released/)