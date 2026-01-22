# MariaDB on Kubernetes (Kustomize)

This repository provides a base Kustomize overlay for running MariaDB as a StatefulSet.

## Prerequisites

- Kubernetes cluster with a default StorageClass (or set one explicitly).
- `kubectl` and `kustomize` (or `kubectl kustomize`).

## Configure

Edit `base/kustomization.yaml` as needed:

- Secret values:
  - `MARIADB_ROOT_PASSWORD` is set by default.
  - Optional: uncomment `MARIADB_USER`, `MARIADB_PASSWORD`, and `MARIADB_DATABASE`.
- Image tag: update the `images` section (`mariadb:11.4` by default).
- Namespace: default is `default`.

If you want a different storage class or size, edit the PVC template in `base/mariadb-stateful.yaml`:

- `spec.volumeClaimTemplates[0].spec.resources.requests.storage`
- `spec.volumeClaimTemplates[0].spec.storageClassName` (add if needed)

## Overlays

Use overlays to customize the base for each environment. Create a new directory like `example`:


Deploy an overlay:

```bash
kubectl apply -k example/
```

## Verify

```bash
kubectl -n default get pods,svc
```

## Connect

Inside the cluster, use the service name `mariadb` on port 3306.

## Remove

```bash
kustomize delete -k example/
```

Note: deleting the StatefulSet does not delete the PVC by default. Remove PVCs manually if you want to delete data.
