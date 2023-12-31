# Policy Reporting using Kyverno

## Policy reports
Policy reports are created based on two different triggers: an admission event (a CREATE, UPDATE, or DELETE action performed against a resource) or the result of a background scan discovering existing resources. Policy reports, like Kyverno policies, have both Namespaced and cluster-scoped variants; a PolicyReport is a Namespaced resource while a ClusterPolicyReport is a cluster-scoped resource. However, unlike Policy and ClusterPolicy resources, the PolicyReport and ClusterPolicyReport resources contain results from resources which are at the same scope and not what is determined by the Kyverno policy. For example, a ClusterPolicy (a cluster-scoped policy) contains a rule which matches on Pods (a Namespaced resource). Results generated from this policy and rule are written to a PolicyReport in the Namespace where the Pod exists.



## Reporting format
Kyverno uses a standard and open format published by the [Kubernetes Policy working group](https://github.com/kubernetes-sigs/wg-policy-prototypes/tree/master/policy-report) which proposes a common policy report format across Kubernetes tools. 

## Viewing Reports

### Command Line Output

Policy reports scoped to namespace 
![polr](../../00-prerequisites/images/polr.png)

Cluster policy report scoped to cluster
![cpolr](../../00-prerequisites/images/cpolr.png)

Example of a [Cluster Policy Report](./cluster-policy-report.yaml)
Example of a [Cluster Admission Report](./cluster-admission-report.yaml)

### Graphical UI
* Nirmata Policy manager : Offers a dashboard to view the reports

Nirmata Dashboard
![nirmata dashboard](./../../00-prerequisites/images/nirmata-dashboard.png)

Nirmata Compliance Viewer
![nirmata-compliance](./../../00-prerequisites/images/nirmata-compliance.png)


* Open Source policy reporter UI solution : [Kyverno Policy Reporter](https://github.com/kyverno/policy-reporter#readme)


**Blog References** : [Blog : Decision making for policy reporting](https://blog.webdev-jogeleit.de/blog/monitor-security-with-kyverno-and-policy-reporter)