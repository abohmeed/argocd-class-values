# argocd-class-values

Per-environment Helm values for **storefront**, owned by the Storefront team.

Companion to [`argocd-class-resources`](https://github.com/abohmeed/argocd-class-resources),
which holds the chart. The two are deliberately separate, and that separation is the
point of **S04 L07** in *ArgoCD 3 in Production: GitOps at Scale on Kubernetes*.

## Why this is its own repository

The platform team owns the chart. They do not want every application team committing
into it just to change a replica count. The Storefront team owns what is in here, and
changes it on its own schedule, without a pull request against the platform's repo.

A single Argo CD `source` block can only read files from the one repository it names —
so a `valueFiles` entry pointing here from an Application whose `repoURL` is the chart
repo simply is not found. That failure is not a bug to work around; it is the thing the
lesson exists to show. The fix is `spec.sources`: two entries, one for the chart and one
for the values, with `ref: values` on the second and `$values/…` in the first's
`valueFiles`.

## Files

| File | Environment |
|---|---|
| `values-dev.yaml` | dev |
| `values-staging.yaml` | staging |
| `values-prod.yaml` | prod |

Each sets only what differs per environment. Everything else comes from the chart's own
`values.yaml`, which lives in the chart repository where it belongs.
