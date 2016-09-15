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

```bash
$ kubectl port-forward registry-1685615563-m469g 5000:5000
  docker pull alpine
  docker tag alpine localhost:5000/alpine
  docker push localhost:5000/alpine

$ minikube ssh
  docker pull localhost:5000/alpine
```
