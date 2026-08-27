# cluster

Cluster-wide configuration for the k3s cluster on `pc1`.

## Layout

```
base/       namespaces, storage classes, per-namespace limits
traefik/    HelmChartConfig for the k3s-bundled ingress controller
docs/       runbooks
scripts/    kubeconfig fetch
apply.sh    apply everything, idempotent
```

## Use

```sh
./apply.sh
```

Runs kubectl on the server over ssh, so it needs no local setup and cannot be
aimed at the wrong cluster. Apply this **before** any app repo — they assume
their namespace exists.

## kubectl from your workstation

```sh
./scripts/kubeconfig.sh --merge
```

Adds a `pc1` context to `~/.kube/config`, backing the file up first and leaving
your current context alone. Then:


