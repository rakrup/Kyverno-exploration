# Validation using Kyverno

**Documentation** : [https://kyverno.io/docs/writing-policies/validate/](https://kyverno.io/docs/writing-policies/validate/)

## Interesting Usecases

1. Disallow MTLS disabling in Istio : [Code](./istio-disable-mtls-restricted.yaml)
2. Prevent disabling Istio Sidecar injecting : [Code](./istio-prevent-disable-sidecar-inject.yaml)
3. Verified Registries Check : [Code](./verified-registries-only.yaml)