

# View the Consul bootstrap token

`kubectl get secret katacoda-consul-bootstrap-acl-token -o yaml`{{execute T1}}

# Set CONSUL_HTTP_TOKEN

`export CONSUL_HTTP_TOKEN=$(kubectl get secrets/katacoda-consul-bootstrap-acl-token --template={{.data.token}} | base64 -d)`{{execute T1}}

# Try to view consul members

`consul members`{{execute T1}}
