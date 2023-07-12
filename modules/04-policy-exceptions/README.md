# Policy Exceptions

One of Kyverno's USP is the fine grained control to resources and policies. Although Kyverno provides the `match`/`exclude` blocks to filter resources, Policy Exceptions come in handy when policies may not be directly editable, or doing so imposes an operational burden.

**Note:** Policy Exceptions are available only in Kyverno 1.9 and above.

**Note:** To enable policy exceptions, set the Kyverno container flag `enablePolicyException` to true and `exceptionNamespace` to the value of the namespace where Policy Exceptions will reside. If not set, PolicyExceptions from all namespaces will be considered

**Note:** Using the Enterprise Kyverno Operator will enable policy exceptions by default and the default exception namespace is `kyverno`.

## Creating Policy Exceptions
Consider the sample Deployment [deployment.yaml](./deployment.yaml) that is sets `spec.hostIPC` to `true`. The policy [disallow-host-namespaces](./disallow-host-namespaces.yaml) blocks such a Deployment.

Let us see the Deployment behavior befor and after deploying the policy.
```sh
kubectl apply -f deployment.yaml
```
The Deployment gets created successfully.

```sh
kubectl apply disallow-host-namespaces.yaml
kubectl apply -f deployment.yaml
```
Now the Deployment is blocked by Kyverno.

There might be numerous reasons why you would need a temporary exception to this policy and rule. Let us see how to create a PolicyException and bypass this rule for creating the Deployment.
```sh
kubectl apply -f policy-exception.yaml
kubectl apply -f deployment.yaml
```
The Deployment gets created successfully.
