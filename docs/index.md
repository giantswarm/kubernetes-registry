+++
title = "Running a Private Docker Registry"
description = "Recipe to spin up a private Docker Registry on Kubernetes."
date = "2016-09-30"
type = "page"
weight = 140
categories = ["recipes"]
+++

# Running a Private Docker Registry

If you want to store truly private Docker images for your cluster, you might want to run your own private Docker registry inside of it.

Keep in mind that this recipe currently does not include SSL or authentification. To work around this, there's a proxy running on each node in the cluster, exposing a port onto the node (via `hostPort`), which Docker accepts as "secure", since it is accessed by `localhost`.

_Note: The registry set up by this recipe currently uses non-persistent storage, so images pushed to it will not survive Pod restarts._

## Deploying the registry

To deploy the registry just apply the manifests included in this recipe.

```bash
kubectl apply --filename manifests/
```

## Using the registry

First, to push images to your private registry from the outside you can set up port forwarding with `kubectl`.

```bash
$ POD=$(kubectl get pods --namespace registry -l app=registry \
    -o template --template '{{range .items}}{{.metadata.name}} {{.status.phase}}{{"\n"}}{{end}}' \
    | grep Running | head -1 | cut -f1 -d' ')
$ kubectl port-forward --namespace registry $POD 5000:5000 &
```

You can then use your local docker client to push images to `localhost:5000`.

```bash
$ docker pull alpine
$ docker tag alpine localhost:5000/user/container:tag
$ docker push localhost:5000/user/container:tag
```

To use this image in your Pods, you need to specify it in your Pod's `spec.containers[].image` field:

`image: localhost:5000/user/container`
