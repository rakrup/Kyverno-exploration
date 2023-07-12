# YAML Signing

Verifying the integrity of resources is important to ensure no tampering has occurred. By ensuring the integrity of software manifests, which contain vital information about the components and dependencies of a software package, developers can verify that the package has not been modified or compromised.

## Signing YAML manifests
In the [prerequisites](./../00-prerequisites/README.md#generate-keypair) step, we have already generated the keypair locally in the [keys](./../00-prerequisites/keys/) directory.
```sh
nctl manifests cosign sign -f deployment.yaml -k cosign.key --tarball no -o signed-deployment.yaml
```

This will sign the [deployment.yaml](./deployment.yaml) and create a new [signed-deployment.yaml](./signed-deployment.yaml). It adds two new annotations to the signed YAML - `cosign.sigstore.dev/message` and `cosign.sigstore.dev/signature`. This is used for verifying the deployment manifest's integrity.

## Verify manifests integrity
Now that we have signed the deployment manifest, we will now verify its integrity by validating the signature before deploying to the cluster. Refer to the policy [verify-manifests-integrity.yaml](./verify-manifests-integrity.yaml). Make sure to replace the public key in the policy YAML.
Apply the policy using,
```sh
kubectl apply -f verify-manifests-integrity.yaml
```

We will now attempt to create the deployment. Any unsigned deployment or a deployment signed with a different private key will be blocked by Kyverno.
Try the below two commands.
```sh
kubectl apply -f deployment.yaml
kubectl apply -f signed-deployment.yaml
```