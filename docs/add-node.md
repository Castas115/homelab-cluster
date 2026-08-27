# Adding a node

pc1 runs `services.k3s.role = "server"`, which is both control plane and
kubelet. Everything below is about adding a *second* machine — the NAS.

## On the server, once

The join token lives at `/var/lib/rancher/k3s/server/node-token` and is
root-only. Treat it like a root password for the cluster: anything holding it
can register a node and schedule pods.

```sh
ssh pc1 sudo cat /var/lib/rancher/k3s/server/node-token
```

## On the new node

In its `configuration.nix`, alongside whatever else it runs:

```nix
services.k3s = {
  enable = true;
  role = "agent";
  serverAddr = "https://<ip>:6443";
  tokenFile = "/etc/k3s-token";
};
```

`tokenFile` rather than `token`, so the value never lands in the world-readable
Nix store. Put the file there out of band:

```sh
sudo install -m 0600 /dev/stdin /etc/k3s-token <<<'K10...'
```

The server needs 6443 reachable from the new node. It is already open, and the
`--tls-san` entries in pc1's config already cover the Tailscale address.

## After it joins

```sh
kubectl get nodes
kubectl label node <name> node-role.kubernetes.io/storage=true
```

## What has to change in the app manifests

Single-node hides two things that a second node exposes immediately.

**`ReadWriteOnce` means one node, not one pod.** Navidrome and the importer
share the music volume today only because both land on pc1. Once they can be
scheduled apart, that PVC has to become `ReadWriteMany`, which hostPath cannot
do — it needs NFS or another shared backend.

**The `nodeAffinity` on every PV in `navidrome/k8s/10-storage.yaml` pins it to
pc1.** That is correct for hostPath and wrong the moment the data moves to the
NAS. Swap the PV body for an `nfs:` source and drop the affinity block in the
same change; the PVCs and the pods do not move.

The order that avoids downtime: get the NAS joined and exporting NFS, mount and
copy the library onto it, then switch the PVs. Not the other way round.
