# Image Signing
Image signing is an important step in securing the software supply chain. By signing container images, developers can verify the authenticity of the source and ensure that it has not been tampered with during transit.

## Signing container images
In the [prerequisites](./../00-prerequisites/README.md#generate-keypair) step, we have already generated the keypair locally in the [keys](./../00-prerequisites/keys/) directory.

To sign the container image,
```sh
cd keys
nctl images cosign sign --key cosign.key <image>
```

This will sign the container image and push it to the registry. Both the image and the signature are saved as OCI artifacts in the registry.

## Verify image signatures
Now that we have signed the container image, we will now verify its signature before deploying to the cluster. Refer to the policy [image-verify-policy.yaml](./image-verify-policy.yaml). Make sure to replace the public key and registry name in the policy YAML.
Apply the policy using,
```sh
kubectl apply -f image-verify-policy.yaml
```

In the [deployment.yaml](./deployment.yaml) file, replace the image reference with the one that you signed above. If you now create this deployment, only the images signed using the corresponding private key, those will get created. Any images that are unsigned or images signed with a different private key, they will be blocked by Kyverno.