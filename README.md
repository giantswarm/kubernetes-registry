# Local Docker Registry within a Kubernetes cluster

See:
  - [kubernetes/cluster/addons/registry](https://github.com/kubernetes/kubernetes/tree/master/cluster/addons/registry)

```bash
$ kubectl apply --recursive --filename ./manifests

$ kubectl run -ti alpine --image giantswarm/tiny-tools sh
  curl http://registry.registry.svc:5000

$ minikube ssh
  curl http://localhost:5000
```

```bash
$ REGISTRY_POD_NAME=$(kubectl get pods --namespace registry -l app=registry,component=main \
  -o template --template '{{(index .items 0).metadata.name}}')

$ kubectl --namespace registry port-forward $REGISTRY_POD_NAME 5000:5000
```
```bash
  docker pull alpine
  docker tag alpine localhost:5000/alpine
  docker push localhost:5000/alpine

$ minikube ssh
  docker pull localhost:5000/alpine
```
