# Multitenancy
To setup multitenancy in EKS cluster, we need to create a lot of roles, rolebindings and restrict tenants to access other tenants resuorces. By using kyverno policy sets, certain roles and rolebindings are created as soon as developer creates his namespace. 

## Add Network policy
This policy will only be required for a calico enabled cluster.
By default, Kubernetes allows communications across all Pods within a cluster. The NetworkPolicy resource and a CNI plug-in that supports NetworkPolicy must be used to restrict communications. A default NetworkPolicy should be configured for each Namespace to default deny all ingress and egress traffic to the Pods in the Namespace. Application teams can then configure additional NetworkPolicy resources to allow desired traffic to application Pods from select sources. This policy will create a new NetworkPolicy resource named `default-deny` which will deny all traffic anytime a new Namespace is created.
Reference- [add-network-policy.yaml](./add-network-policy.yaml).

## Add RoleBinding
Typically in multi-tenancy and other use cases, when a new Namespace is created, users and other principals must be given some permissions to create and interact with resources in the Namespace. Very commonly, Roles and RoleBindings are used to grant permissions at the Namespace level. This policy generates a RoleBinding called `<userName>-admin-binding` in the new Namespace which binds to the ClusterRole `admin` as long as a `cluster-admin` did not create the Namespace. Additionally, an annotation named `kyverno.io/user` is added to the RoleBinding recording the name of the user responsible for the Namespace's creation.
Today, this is done at cluster level in single tenant cluster. But user needs to be restricted at namespace level in a multi-tenant cluster. Referance- [add-network-policy.yaml](./add-network-policy.yaml).

## Disallow Default Namespace

Kubernetes Namespaces are an optional feature that provide a way to segment and isolate cluster resources across multiple applications and users. As a best practice, workloads should be isolated with Namespaces. Namespaces should be required and the default (empty) Namespace should not be used. This policy validates that Pods specify a Namespace name other than `default`. Rule auto-generation is disabled here due to Pod controllers need to specify the `namespace` field under the top-level `metadata` object and not at the Pod template level.
This policy can be used to restrict all the platform namespaces.
Reference- [disallow-default-namespace.yaml](./disallow-default-namespace.yaml).
