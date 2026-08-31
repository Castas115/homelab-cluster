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
- [ ] Add 2 worker nodes
- [ ] Create NAS
- [ ] Deploy with Argo CD
- [ ] Improve networking switch 1 to 2.5 gigabit 

## projects
- [/] navidrome
- [ ] AdGuard Home
- [ ] Actual Budget
- [ ] Tailscale alternative
- [ ] Jellyfin
