# Local Docker Registry within a Kubernetes cluster

See:
  - [kubernetes/cluster/addons/registry](https://github.com/kubernetes/kubernetes/tree/master/cluster/addons/registry)

```bash
$ kubectl apply --filename manifests/

$ kubectl run -ti alpine --image webwurst/curl-utils sh
  curl http://registry:5000

$ minikube ssh
  curl http://localhost:5000
```
