# EKS Best Practices
A comprehensive list of recommendations and best practices for securing EKS clusters is published by AWS [here](https://aws.github.io/aws-eks-best-practices/security/docs/).

Kyverno being Kubernetes-native, can be extended to other resources via Custom Resource Definition (CRD) and controllers. Because EKS Best Practices includes enforcing/auditing for cloud resources, we can use the [Kyverno AWS Adapter](https://github.com/nirmata/kyverno-aws-adapter) by Nirmata. See detailed blog post [here](https://nirmata.com/2023/04/04/enforcing-security-best-practices-for-amazon-eks-using-kyverno/).

## Installing the Kyverno AWS Adapter
Refer to the installation guide [here](https://github.com/nirmata/kyverno-aws-adapter/blob/main/docs/getting_started.md).

## Deploy EKS Best Practices policies
We will use the Nirmata-curated policies for the controls specified by AWS.
**Note:** The policies map to a subset of controls and not the entirety of the best practices recommended.

```sh
git clone git@github.com:nirmata/kyverno-policies.git
cd kyverno-policies
kubectl apply -f eks-best-practices/
```
