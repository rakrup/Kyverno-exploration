# Pod Security Admission
**Note:** A detailed explanation can be found on the official [Kyverno blog](https://kyverno.io/blog/2023/06/12/using-kyverno-with-pod-security-admission/). This README extracts some of the highlights mentioned in the blog.

[Pod Security Admission](https://kubernetes.io/docs/concepts/security/pod-security-admission/) (PSA) is the built-in successor to Kubernetes [PodSecurityPolicy](https://kubernetes.io/docs/concepts/security/pod-security-policy/) (PSP). Kyverno is highly complementary and deliver increased value through tighter security guarantees while catering to the flexibility demanded by modern Kubernetes operations.

PSA is the technology which implements the Pod Security Standards (PSS). Those standards are broken up into three profiles: [privileged](https://kubernetes.io/docs/concepts/security/pod-security-standards/#privileged), which is effectively the lack of any security, [baseline](https://kubernetes.io/docs/concepts/security/pod-security-standards/#baseline), and [restricted](https://kubernetes.io/docs/concepts/security/pod-security-standards/#restricted).

## Using Nirmata Kyverno with PSA

### Kyverno to require PSA namespace label
* Validate that labels are assigned to namespaces
* Automatically assign the labels

### Granular PSA controls
With PSA, it is all or nothing. Either you apply a profile (baseline or restricted), or you do not. Kyverno integrates with PSA and allows you finer access to controls (via `subrule`). While you can specify baseline control using PSA, you can specify any particular control(s) that need to be excluded, instead of disabling the entire profile itself.
[Nirmata's Implementation of PSA](https://github.com/nirmata/kyverno-policies/tree/main/pod-security)

### Reporting with Nirmata
Policy Reports in Nirmata powered by the [PolicyReport CRD](https://kyverno.io/docs/policy-reports/), a Kubernetes-native resource which is an open standard developed by the upstream [Kubernetes policy working group](https://github.com/kubernetes-sigs/wg-policy-prototypes/tree/master/policy-report) and adopted by many other tools.

### Nirmata CLI in pipelines
All the above checks can be done in CI pipelines as well. Nirmata offers a comprehensive CLI (`nctl`) that can be used to validate resources against Pod Security Standard Baseline and Restricted profiles enforced by PSA. You do not need a working cluster for using in the CI pipelines (unlike PSA, which requires a functional cluster)

## Comparison

| Pod Security Policy	| Pod Security Admission	| Kyverno |
|-----------------------|---------------------------|-----------|
|Pods only	|Pods only	|Anything|
|Limited options	|Only 2 options (PSS only, gaps*)	|Anything
|Limited mutation	|No mutation	|Mutate anything
|Requires RBAC modifications	| Does not require RBAC	| Does not require RBAC
|Limited to User, Group, ServiceAccount	| Limited to cluster, Namespace	| Any association
|No support for Pod controllers	| No support for Pod controllers**	| Automatic support for Pod controllers
|No auditing	| Audits in audit log	| Audits in Policy Reports
|No message customization	| No message customization	| Fully custom messages
|No exclusions	| Limited exclusions	| Flexible exclusions
|Integrated	| Integrated	| External

\* No readOnlyRootFilesystem, runtimeClass (excludes deprecated options)__
\** Audit support only