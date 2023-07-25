# Prerequisites

[Go back of Main Menu](../../navigation.md)

## Create Kubernetes Cluster
Next, we need a Kubernetes cluster before we can install Kyverno. Ensure that your kubectl context points to the correct Kubernetes cluster. For most modules, any cluster will suffice (even local [kind](https://kind.sigs.k8s.io/) cluster). But for [04-eks-best-practices](./../04-eks-best-practices/README.md), we will need an [EKS cluster](https://docs.aws.amazon.com/eks/latest/userguide/create-cluster.html).

## Nirmata Policy Manager - Setup
To login to the Nirmata Policy Manager (NPM), sign-up for a free trial [here](https://www.nirmata.io/security/signup.html?product=NPMK)

Once you setup username and password, login to the NPM portal to onboard your clusters and view/manage policies and compliance across all your clusters.

## nctl - Nirmata CLI
The Nirmata CLI (nctl) can be used for registering clusters, image and YAML manifests signing, and for installing and managing Kyverno and policies.
For YAML & Image signing alternately cosign or notary could be used, however, if you use Nirmata CLI you get enterprise support over what the opensource bits provide.
For instance k8-manifest project used for signing YAMLs still considers itself not ready for production. However, when wrapped under ntcl - Nirmata can offer support on the utility.

### Install nctl
```sh
curl -sL https://raw.githubusercontent.com/nirmata/kyverno-charts/main/scripts/n4k/nctl-install.sh | sh
```
Verify installation using,
```sh
nctl version
```

## Generate keypair
For the [image signing](./../01-image-signing/README.md) and [YAML signing](./../02-yaml-signing/README.md) modules, we will need a public-private keypair. 
Follow the steps below to creata a set of keys that would be used in both these signing related excercises. 
Create a new directory called `keys`.

```sh
mkdir keys
cd keys/
```

Now we will generate the keypair. Enter the password for private key and it will be created in the `keys` directory.

```sh
nctl images cosign generate-key-pair
```

Verify that keys are generated.
```sh
ls
cosign.key cosign.pub
```
We shall later refer this key in susequent modules.


[Go back of Main Menu](../../navigation.md)