# FinOps - Continous Compliance

[Go back of Main Menu](../../navigation.md)

## Kyverno for Fintechs

Whitepaper on how is currently [solving fintech challenges in real world](https://nirmata.com/2023/07/20/strengthening-governance-and-security-in-financial-institutions-with-policy-as-code-and-kyverno/).

## Integration with Cost Management tools
Kyverno integrates with yet another CNCF project, [OpenCost](https://github.com/opencost/opencost) to enforce policies for allocating resources in Kubernetes that helps in managing costs for any organization. See detailed blog post [here](https://nirmata.com/2023/05/24/policy-based-cost-management-in-kubernetes-leveraging-opencost-and-kyverno-for-maximum-efficiency/)


## Best Practices and Governance
There are a number of Kyverno policies that can be used for ensuring proper requests and limits are set for the pods, scale deployment to zero, cleanup resources when not needed, etc. Some of these best practices are curated by Nirmata and can be applied from this repo.

[Nirmata CIS Benchmarks](https://github.com/nirmata/kyverno-charts/tree/main/charts/kube-bench-adapter)
```sh
# 1. Add Kyverno Helm Repository
helm repo add nirmata https://nirmata.github.io/kyverno-charts/
# 2. Install kube-bench adapter from nirmata helm repo with desired parameters.
helm install kube-bench-adapter nirmata/kube-bench-adapter --set kubeBench.name="test-1" --set kubeBench.yaml="job-eks.yaml"
# 3. Watch the jobs
kubectl get jobs --watch
# 4. Check policyreports created through the custom resource
kubectl get clusterpolicyreports
```

[Nirmata FinOps Policies](https://github.com/nirmata/kyverno-policies/tree/main/finops)
```sh
git clone git@github.com:nirmata/kyverno-policies.git
cd kyverno-policies
kubectl apply -f finops/
```


From a governance perspective, there are a number of compliance standards that are relevant for the finance domain. One such notable standard is the PCI/DSS guidelines. Some of the controls, if not all, can be mapped to Kyverno policies and here is such a sample list of policies.

[Nirmata PCI/DSS](https://github.com/nirmata/kyverno-policies/tree/main/pci-dss)
```sh
cd kyverno-policies
kubectl apply -f pci-dss/
```

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

[Go back of Main Menu](../../navigation.md)