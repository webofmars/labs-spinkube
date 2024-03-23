# SpinKube

## install cert-manager

```sh
# Install cert-manager CRDs
kubectl apply -f https://github.com/cert-manager/cert-manager/releases/download/v1.14.3/cert-manager.crds.yaml

# Add and update Jetstack repository
helm repo add jetstack https://charts.jetstack.io
helm repo update

# Install the cert-manager Helm chart
helm install \
  cert-manager jetstack/cert-manager \
  --namespace cert-manager \
  --create-namespace \
  --version v1.14.3
```

## install kwasm-operator

```sh
helm repo add kwasm http://kwasm.sh/kwasm-operator/
helm repo update

helm install \
  kwasm-operator kwasm/kwasm-operator \
  --namespace kwasm \
  --create-namespace \
  --set kwasmOperator.installerImage=ghcr.io/spinkube/containerd-shim-spin/node-installer:v0.13.1

kubectl annotate node --all kwasm.sh/kwasm-node=true
```

## install Spin Operator with Helm

```sh
kubectl apply -f https://github.com/spinkube/spin-operator/releases/download/v0.1.0/spin-operator.crds.yaml

helm install spin-operator \
  --namespace spin-operator \
  --create-namespace \
  --version 0.1.0 \
  --wait \
  oci://ghcr.io/spinkube/charts/spin-operator
```

## deploy the spin operator shim

```sh
kubectl apply -f https://github.com/spinkube/spin-operator/releases/download/v0.1.0/spin-operator.shim-executor.yaml
```

## deploy spin runtime-class

```sh
kubectl apply -f https://github.com/spinkube/spin-operator/releases/download/v0.1.0/spin-operator.runtime-class.yaml
```

## install spin locally

```sh
curl -fsSL https://developer.fermyon.com/downloads/install.sh | bash
mv spin ~/bin/spin # or anywhere in your PATH
```

## create a new app

```sh
spin new
# choose from one template
# name it "demo"
```

## build your app

```sh
cd demo
spin build
spin up
```

## deploy your wasm app to k8s

```sh
spin registry push --build ttls.sh/demo-spin:1h
```
