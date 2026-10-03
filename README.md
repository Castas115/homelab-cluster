# cluster

Cluster-wide configuration for the k3s cluster on `pc1`.

## kubectl from your workstation

```sh
./scripts/kubeconfig.sh --merge
```

Adds a `pc1` context to `~/.kube/config`, backing the file up first and leaving
your current context alone. Then:


# Roadmap

## cluster
- [x] Add 2 worker nodes
- [ ] Create NAS
- [x] Deploy with Argo CD
- [x] reverse proxy
- [ ] Improve networking switch 1 to 2.5 gigabit 

## projects
- [x] navidrome
- [x] backup system
- [x] AdGuard Home
- [x] Jellyfin
- [ ] Actual Budget
- [ ] Tailscale alternative
- [ ] Forgejo (lightweight gitlab alternative)
