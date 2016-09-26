# Local Docker Registry within a Kubernetes cluster

See:
  - [kubernetes/cluster/addons/registry](https://github.com/kubernetes/kubernetes/tree/master/cluster/addons/registry)

```bash
$ kubectl create namespace registry

$ kubectl --namespace registry apply --filename manifests/
$ kubectl --namespace registry create secret generic registry-tls-secret \
  --from-file=tls.crt=server.pem \
  --from-file=tls.key=server-key.pem

$ kubectl run -ti alpine --image webwurst/curl-utils sh
  curl http://registry.registry.svc.cluster.local:5000

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

$ minikube ip
$ sudo vi /etc/hosts
  <minikube ip> registry.local.net

$ sudo mkdir --parents /etc/docker/certs.d/registry.local.net
$ sudo cp certs/ca.pem /etc/docker/certs.d/registry.local.net/ca.crt
$ sudo systemctl restart docker.service
```
