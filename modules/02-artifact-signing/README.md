# Signing Artifacts with Kyverno

Kyverno has the ability to verify signatures on artifacts and define audit/enforcement rules based on the signatures or attributes that have been attested by a trusted authority.

Image Signing could with use of any supported tool. Currently the most common projects used for signing are [Cosign](https://github.com/sigstore/cosign) & [Notary](https://www.cncf.io/projects/notary/).

> **Note**
> Nirmata also offers signing as part of the *ntclt* CLI. Currently supports cosign/notary underneath. Advantage of using ntctl is the enterprise support on these tools that would be offered by Nirmata.
---
# Current Scope
> **Info**
>Currently the we talk about signing Images and YAML. However, CoSign can sign much more - Tekton Pipelines, WASM, eBPF, and helm packaged into OCI complaint images.

1. [Image Signing](./01-image-signing/)
2. [Manifest(YAML) Signing](./02-yaml-signing/)
---

## Interesting UseCases

1. [Securing CICD pipelines using Kyverno](https://rcarrata.com/kubernetes/sign-images-1/)


## CoSign - A Sigstore Project

Here is a primer on Cosign capabilities as it is heavily used underneath and actually performs the signing. Remember !! Kyverno just performs validations of these artifacts that have been signed/attested in standardized formats.

### Cosign supports:

* "*Keyless signing*" with the Sigstore public good Fulcio certificate authority and Rekor transparency log (default)
* Hardware and KMS signing
* Signing with a cosign generated encrypted private/public keypair
* Container Signing, Verification and Storage in an OCI registry.
* Bring-your-own PKI

### Co-Sign Compatible Registries:

* AWS Elastic Container Registry
* GCP's Artifact Registry and Container Registry
* Docker Hub
* Azure Container Registry
* JFrog Artifactory Container Registry
* The CNCF distribution/distribution Registry
* GitLab Container Registry
* GitHub Container Registry
* The CNCF Harbor Registry
* Digital Ocean Container Registry
* Sonatype Nexus Container Registry
* Alibaba Cloud Container Registry
* Red Hat Quay Container Registry 3.6+ / Red Hat quay.io
* Elastic Container Registry
* IBM Cloud Container Registry
* Cloudsmith Container Registry
* The CNCF zot Registry

## Cosign can Sign


[Tekton](https://tekton.dev) bundles can be uploaded and managed within an OCI registry.
The specification is [here](https://tekton.dev/docs/pipelines/tekton-bundle-contracts/).
This means they can also be signed and verified with `cosign`.

Tekton Bundles can currently be uploaded with the [tkn cli](https://github.com/tektoncd/cli), but we may add this support to
`cosign` in the future.

```shell
$ tkn bundle push us.gcr.io/dlorenc-vmtest2/pipeline:latest -f task-output-image.yaml
Creating Tekton Bundle:
        - Added TaskRun:  to image

Pushed Tekton Bundle to us.gcr.io/dlorenc-vmtest2/pipeline@sha256:124e1fdee94fe5c5f902bc94da2d6e2fea243934c74e76c2368acdc8d3ac7155
$ cosign sign --key cosign.key us.gcr.io/dlorenc-vmtest2/pipeline@sha256:124e1fdee94fe5c5f902bc94da2d6e2fea243934c74e76c2368acdc8d3ac7155
Enter password for private key:
tlog entry created with index: 5086
Pushing signature to: us.gcr.io/dlorenc-vmtest2/demo:sha256-124e1fdee94fe5c5f902bc94da2d6e2fea243934c74e76c2368acdc8d3ac7155.sig
```

#### WASM

Web Assembly Modules can also be stored in an OCI registry, using this [specification](https://github.com/solo-io/wasm/tree/master/spec).

Cosign can upload these using the `cosign wasm upload` command:

```shell
$ cosign upload wasm -f hello.wasm us.gcr.io/dlorenc-vmtest2/wasm
$ cosign sign --key cosign.key us.gcr.io/dlorenc-vmtest2/wasm@sha256:9e7a511fb3130ee4641baf1adc0400bed674d4afc3f1b81bb581c3c8f613f812
Enter password for private key:
tlog entry created with index: 5198
Pushing signature to: us.gcr.io/dlorenc-vmtest2/wasm:sha256-9e7a511fb3130ee4641baf1adc0400bed674d4afc3f1b81bb581c3c8f613f812.sig
```
#### eBPF

[eBPF](https://ebpf.io) modules can also be stored in an OCI registry, using this [specification](https://github.com/solo-io/bumblebee/tree/main/spec).

The image below was built using the `bee` tool. More information can be found [here](https://github.com/solo-io/bumblebee/)

Cosign can then sign these images as they can any other OCI image.

```shell
$ bee build ./examples/tcpconnect/tcpconnect.c localhost:5000/tcpconnect:test
$ bee push localhost:5000/tcpconnect:test
$ cosign sign  --key cosign.key localhost:5000/tcpconnect@sha256:7a91c50d922925f152fec96ed1d84b7bc6b2079c169d68826f6cf307f22d40e6
Enter password for private key:
Pushing signature to: localhost:5000/tcpconnect
$ cosign verify --key cosign.pub localhost:5000/tcpconnect:test

Verification for localhost:5000/tcpconnect:test --
The following checks were performed on each of these signatures:
  - The cosign claims were validated
  - The signatures were verified against the specified public key

[{"critical":{"identity":{"docker-reference":"localhost:5000/tcpconnect"},"image":{"docker-manifest-digest":"sha256:7a91c50d922925f152fec96ed1d84b7bc6b2079c169d68826f6cf307f22d40e6"},"type":"cosign container image signature"},"optional":null}]

```

#### In-Toto Attestations

Cosign also has built-in support for [in-toto](https://in-toto.io) attestations.
The specification for these is defined [here](https://github.com/in-toto/attestation).

You can create and sign one from a local predicate file using the following commands:

```shell
$ cosign attest --predicate <file> --key cosign.key $IMAGE_URI_DIGEST
```

All of the standard key management systems are supported.
Payloads are signed using the DSSE signing spec, defined [here](https://github.com/secure-systems-lab/dsse).

To verify:

```shell
$ cosign verify-attestation --key cosign.pub $IMAGE_URI
```