# Multi-tenancy with Flux

This repo configures a multi-tenant flux setup with the following directory structure:

```
.
├── README.md
├── clusters
│   └── kind
│       ├── flux-system
│       └── tenants
└── tenants
    ├── team-a
    │   └── app.yaml
    └── team-b
        └── app.yaml
```
Tenants are **onboarded** via `clusters/kind/tenants`.

Onboarding creates the following resources for each tenant:
- GitRepository (using public http endpoint, for private repo secrets would be provided also)
- Kustomization (pointing at `./tenants/${team-name}`)
- Namespace
- ServiceAccount
- RoleBinding

To illustrate that multi-tenancy is properly enforced fork this repo and run `flux bootstrap --path ./clusters/kind` with the appropriate details for your fork.

We will see that flux creates our tenant resources. The `podinfo` application will be deployed successfully to `team-a` namespace.

However because `team-b` attempts to deploy their application into the `team-a` namespace the Kubernetes API will refuse to deploy their application and the `HelmRelease` for `team-b` will fail.
