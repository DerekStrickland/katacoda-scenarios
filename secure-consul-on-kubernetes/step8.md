### Deploy a backend service

`kubectl apply -f server.yaml`{{execute T1}}

### Deploy a client service

`kubectl apply -f client.yaml`{{execute T1}}

### Create a deny all intention

`consul intention create -ca-file consul-agent-ca.pem -deny "*" "*"`

### Validate intention enforced

`kubectl exec static-client static-client -- curl -s http://127.0.0.1:1234/ `{{execute T1}}

```plaintext
Defaulting container name to static-client.
Use 'kubectl describe pod/static-client -n default' to see all of the containers in this pod.
command terminated with exit code 52
```

### Create an allow intention for the client to server traffic

`consul intention create -ca-file consul-agent-ca.pem -allow static-client static-server`{{execute T1}}

### Validate intention enforced

`kubectl exec static-client static-client -- curl -s http://127.0.0.1:1234/ `{{execute T1}}

```plaintext
Defaulting container name to static-client.
Use 'kubectl describe pod/static-client -n default' to see all of the containers in this pod.
"hello world"
```
