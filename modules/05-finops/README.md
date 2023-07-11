# FinOps

## Integration with Cost Management tools
Kyverno integrates with yet another CNCF project, [OpenCost](https://github.com/opencost/opencost) to enforce policies for allocating resources in Kubernetes that helps in managing costs for any organization. See detailed blog post [here](https://nirmata.com/2023/05/24/policy-based-cost-management-in-kubernetes-leveraging-opencost-and-kyverno-for-maximum-efficiency/)


## Best Practices and Governance
There are a number of Kyverno policies that can be used for ensuring proper requests and limits are set for the pods, scale deployment to zero, cleanup resources when not needed, etc. Some of these best practices are curated by Nirmata and can be applied from this repo.

```sh
git clone git@github.com:nirmata/kyverno-policies.git
cd kyverno-policies
kubectl apply -f finops/
```

From a governance perspective, there are a number of compliance standards that are relevant for the finance domain. One such notable standard is the PCI/DSS guidelines. Some of the controls, if not all, can be mapped to Kyverno policies and here is such a sample list of policies.

```sh
cd kyverno-policies
kubectl apply -f pci-dss/
```
