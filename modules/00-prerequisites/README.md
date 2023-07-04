# Prerequisites

## nctl
The Nirmata CLI (nctl) can be used for registering clusters, image and YAML manifests signing, and for installing and managing Kyverno and policies.

### Install nctl
```sh
curl -sL https://raw.githubusercontent.com/nirmata/kyverno-charts/main/scripts/n4k/nctl-install.sh | sh
```
Verify installation using,
```sh
nctl version
```

## Generate keypair
For the image signing and YAML signing modules, we will need a public-private keypair. Create a new directory called `keys`.
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

## Create Cluster
Next, we need a Kubernetes cluster before we can install Kyverno. Ensure that your kubectl context points to the correct Kubernetes cluster. For most modules, any cluster will suffice (even local [kind](https://kind.sigs.k8s.io/) cluster). But for [04-eks-best-practices](./../04-eks-best-practices/README.md), we will need an [EKS cluster](https://docs.aws.amazon.com/eks/latest/userguide/create-cluster.html).

## Nirmata Policy Manager
To login to the Nirmata Policy Manager (NPM), sign-up for a free trial [here](https://www.nirmata.io/security/signup.html?product=NPMK)

Once you setup username and password, login to the NPM portal to onboard your clusters and view/manage policies and compliance across all your clusters.