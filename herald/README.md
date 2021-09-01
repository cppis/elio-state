# herald 
herald state repository using `helm`  

<figure>
  <div style="text-align:center">
    <img src="https://miro.medium.com/max/400/1*ANDxSZMbvvhaxwqdI-6rPw.png" style="width: 120px; max-width: 100%; height: auto" title="helm" />
  </div>
</figure>

## Installation  
### Download `elio`  
```shell
$ git clone https://github.com/cppis/elio-state
$ cd elio-state
```

> Now, **$PWD** is the root path.  

<br/>

### [Setting `Helm` on Windows](docs/setting.helm.md)  
`Helm` settings on windows for managing packages for a Kubernetes.  

<br/><br/><br/>

## Run  
Test docker image:  
```shell
$ docker run -it -p 7000:7000 -e HERALD_IN_URL=0.0.0.0:7000 cppis/herald:v0.1.6
  ...
```

> You can test a `echo` command. but can't a `pub`/`sub` 

To install a herald:  
```shell
$ helm install herald herald  
  ...
  1. Get the application URL by running these commands:
    export NODE_PORT=$(kubectl get --namespace default -o jsonpath="{.spec.ports[0].nodePort}" services herald-herald-emqx)
    export NODE_IP=$(kubectl get nodes --namespace default -o jsonpath="{.items[0].status.addresses[0].address}")
    echo http://$NODE_IP:$NODE_PORT
```

To get a exposed port of herald:  
```shell
$ kubectl get --namespace default -o jsonpath="{.spec.ports[0].nodePort}" services herald-herald-emqx
  32302
```

To uninstall a herald:  
```shell
$ helm uninstall herald
```

<br/><br/><br/>

## Tips  
### Create a *chart*  
To create chart:  
```shell
$ helm create herald
```

<br/><br/>

### Install a *release*  
To install chart as release `herald-emqx`:  
```shell
$ helm install herald-emqx helm/herald-emqx
```

<br/>

Or you can specify *value.yaml* or *namespace*:  
```shell
$ helm install 
    -f hello-world/values.yaml
    -n herald
    herald-emqx helm/herald-emqx
```

> To pass environment variable add `--set` option like `--set HERALD_IN_URL="0.0.0.0:8000"`.  

<br/>

To simulate an install for debugging:  
```shell
$ helm install herald-emqx helm/herald-emqx --dry-run --debug
```

<br/>

To find exposed service port:  
```shell
$ kubectl get --namespace default -o jsonpath="{.spec.ports[0].nodePort}" services herald-emqx
$ kubectl get nodes --namespace default -o jsonpath="{.items[0].status.addresses[0].address}"
```

<br/>

To list release:  
```shell
helm list
```

<br/>

To get status of release for debugging:  
```shell
$ helm status herald-emqx
```

<br/><br/>

### Uninstall a *release*  
To uninstall a release `herald-emqx`:  
```shell
$ helm uninstall herald-emqx
```

> To simulate an uninstall for debugging, add `--dry-run` option.

<br/>

delete namespace in the kubernetes cluster:  
```shell
$ kubectl delete ns herald
```
