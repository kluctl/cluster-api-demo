# cluster-api-demo

This is the demo project used in [Managing Cluster API with Kluctl](https://kubernetes.io/blog/). Please read this
tutorial to get a good understanding of the demo project.

If you just want to give it a try, here are some instructions:

# Trying it

First, create a kind cluster:

```bash
$ kind create cluster --config kind-config.yaml
Creating cluster "kind" ...
 ✓ Ensuring node image (kindest/node:v1.29.2) 🖼
 ✓ Preparing nodes 📦
 ✓ Writing configuration 📜
 ✓ Starting control-plane 🕹️
 ✓ Installing CNI 🔌
 ✓ Installing StorageClass 💾
Set kubectl context to "kind-kind"
You can now use your cluster with:

kubectl cluster-info --context kind-kind

Have a nice day! 👋
```

Then enter `1-initial` and run Kluctl:

```bash
$ kluctl deploy -t demo-1
```

Then, install Cluster API into the new Kind cluster:
```bash
$ clusterctl init --infrastructure docker
Fetching providers
Installing cert-manager Version="v1.13.2"
Waiting for cert-manager to be available...
Installing Provider="cluster-api" Version="v1.6.1" TargetNamespace="capi-system"
Installing Provider="bootstrap-kubeadm" Version="v1.6.1" TargetNamespace="capi-kubeadm-bootstrap-system"
Installing Provider="control-plane-kubeadm" Version="v1.6.1" TargetNamespace="capi-kubeadm-control-plane-system"
Installing Provider="infrastructure-docker" Version="v1.6.1" TargetNamespace="capd-system"

Your management cluster has been initialized successfully!

You can now create your first workload cluster by running the following:

  clusterctl generate cluster [name] --kubernetes-version [version] | kubectl apply -f -
```

Now, run `kluctl deploy` from inside 2-templating:

```bash
$ kluctl deploy -t demo-1
```

Confirm the deployment and wait for some time.

Then, get the kubeconfig:

```bash
$ kind get kubeconfig --name demo-1 > demo-1.kubeconfig
```

Access the new workload cluster:

```bash
$ kubectl --kubeconfig=demo-1.kubeconfig get node
```

Then, try whatever change in `2-templating` and re-run `kluctl deploy -t demo-1` and see for yourself how Kluctl handles these changes.