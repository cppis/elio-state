# herald 
herald state repository using `helm`  

<figure>
  <div style="text-align:center">
    <img src="https://miro.medium.com/max/400/1*ANDxSZMbvvhaxwqdI-6rPw.png" style="width: 120px; max-width: 100%; height: auto" title="helm" />
  </div>
</figure>

`herald` means messanger.  
It is a sample app to test pub/sub of backing `emqx` message broker.  

Here is `herald` components:  
![docs/images/herald.skaffold.png](https://github.com/cppis/elio/blob/dev/docs/images/herald.skaffold.config.png?raw=true)  

<br/><br/>

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

<br/>

### Install  
To install a herald:  
```shell
$ helm install herald herald  
  ...
```

To test a herald, use a `telnet`:  
```shell
$ telnet localhost 33133
```

> Port number path is `.Value.service.port` in the *values.yaml*  

You can test echo easily by using telnet.  

app protocol is custom `t2p` like http.  
procotol header is separated by newline(`\n` or `\r\n`).  
And packet delimiter is double newline(`\n\n` or `\r\n\r\n`).

<br/>

### Commands  
* echo: echo message    
  ```
  echo<newline>
  {message}<newline><newline>
  ```
* sub: subcribe to topic    
  ```
  sub<newline>
  {topic}<newline><newline>
  ```
* unsub: unsubcribe from topic  
  ```
  unsub<newline>
  {topic}<newline><newline>
  ```
* pub: publish message to topic  
  ```
  pub<newline>
  {topic}<newline>
  {message}<newline><newline>
  ```

<br/>

### Uninstall  
To uninstall a herald:  
```shell
$ helm uninstall herald
```

<br/><br/><br/>

## Tips  
### Kubernetes Service Ports  
```yaml
kind: Service
apiVersion: v1

metadata:
  name: host-service

spec:
  type: NodePort      # Expose type 
  selector:
    app: echo-host    # foward requests to pods with this label
  ports:
    - nodePort: 30163 # external access port
    port: 8080        # internal access port
    targetPort: 80    # container access port
```

<br/><br/>

### Create a *chart*  
To create chart:  
```shell
$ helm create herald
```

<br/><br/>

### Install a *release*  
To install chart as release `herald`:  
```shell
$ helm install herald herald
```

<br/>

Or you can specify *value.yaml* or *namespace*:  
```shell
$ helm install 
    -f hello-world/values.yaml
    -n herald
    herald herald
```

> To pass environment variable add `--set` option like `--set HERALD_IN_URL="0.0.0.0:8000"`.  

<br/>

To simulate an install for debugging:  
```shell
$ helm install herald herald --dry-run --debug
```

<br/>

To find exposed service port:  
```shell
$ kubectl get --namespace default -o jsonpath="{.spec.ports[0].nodePort}" services herald
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
$ helm status herald
```

<br/><br/>

### Uninstall a *release*  
To uninstall a release `herald`:  
```shell
$ helm uninstall herald
```

> To simulate an uninstall for debugging, add `--dry-run` option.

<br/>

delete namespace in the kubernetes cluster:  
```shell
$ kubectl delete ns herald
```
