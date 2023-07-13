# Value additions in Kubernetes using Kyverno

Kyverno being Kubernetes-native, provides a lot of value additions on a Kubernetes cluster. They have certain policy-sets which can be added to the clusters and directly add those features in the cluster. It can also implements the functionality of validating and mutating webhooks.

## Use cases

We have used kyverno policies to implement certain value additions in Kubernetes:
1. [Multi-tenancy](./07-multitenancy/README.md)- Kyverno has list of policies which can be deployed based on the use-case to implement multi-tenancy.
2. [PSA-migration](./03-psa/README.md)- [Pod Security Admission](https://kubernetes.io/docs/concepts/security/pod-security-admission/) (PSA) is the built-in successor to Kubernetes [PodSecurityPolicy](https://kubernetes.io/docs/concepts/security/pod-security-policy/) (PSP). Kyverno is highly complementary and deliver increased value through tighter security guarantees while catering to the flexibility demanded by modern Kubernetes operations.
Kyverno has the ability to ease the implementations of multitenancy and ease the migration to PSA. There are certain policy sets which platform team can enable on their Kubernetes cluster and implement the features easily.

3. EKS Best Practices- A comprehensive list of recommendations and best practices for securing EKS clusters is published by AWS [here](https://aws.github.io/aws-eks-best-practices/security/docs/).

**Note**: Kyverno can be extended to other resources via Custom Resource Definition (CRD) and controllers. Because EKS Best Practices includes enforcing/auditing for cloud resources, we can use the [Kyverno AWS Adapter](https://github.com/nirmata/kyverno-aws-adapter) by Nirmata. See detailed blog post [here](https://nirmata.com/2023/04/04/enforcing-security-best-practices-for-amazon-eks-using-kyverno/).