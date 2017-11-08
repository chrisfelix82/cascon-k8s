# Steps:

1. Connect to your cluster

```
bx cs cluster-config cascon-2017-cluster
```

1. View cluster details with 
```
bx cs cluster-get cascon-2017-cluster
```
1. You can view details in the UI as well. For example. https://console.bluemix.net/containers-kubernetes/clusters/1283cb87a6364855ad8985640505f8cd/nodes?env_id=ibm%3Ayp%3Aus-south

1. Create a simple micro-service from nginx and deploy to Private registry

```
bx cr login
bx cr namespace-add cascon2017
bx cr image-list
cd my-nginx
docker build -t registry.ng.bluemix.net/cascon2017/my-nginx:1.0.0 .
docker push registry.ng.bluemix.net/cascon2017/my-nginx:1.0.0
bx cr image-list
cd ..
```

1. You can create a Helm release for your project's stack.  For example:

```
helm create sample-stack-2
```

1. Deploy the stack with the following command

```
helm lint ./sample-stack --strict
helm install ./sample-stack --name sample-stack-2
```

1. Edit the my-nginx service and rebuild/push to registry

```
cd my-nginx
docker build -t registry.ng.bluemix.net/cascon2017/my-nginx:1.0.1 .
docker push registry.ng.bluemix.net/cascon2017/my-nginx:1.0.1
bx cr image-list
cd ..
```

1. Edit service and deploy a new version of the chart

```
helm list
helm upgrade sample-stack-2 ./sample-stack
helm list
helm history sample-stack-2
```

1. Rollback to previous version

```
helm rollback sample-stack-2 1
```