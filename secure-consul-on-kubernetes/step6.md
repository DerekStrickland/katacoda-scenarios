# View the Consul bootstrap token

`kubectl get secret katacoda-consul-bootstrap-acl-token -o yaml`{{execute T1}}

# Set CONSUL_HTTP_TOKEN

`export CONSUL_HTTP_TOKEN=$(kubectl get secrets/katacoda-consul-bootstrap-acl-token --template={{.data.token}} | base64 -D)`{{execute T1}}

# Try to view consul members

`consul members`{{execute T1}}

# Switch back to server shell session and start tcpdump again

`kubectl exec -it katacoda-consul-server-0 -- /bin/sh`{{execute T2}}

`rm -rf /tmp/tcpdump.log`{{execute T2}}

`tcpdump -an portrange 8300-8700 -A > /tmp/tcpdump.log`{{execute T2}}

# Switch to host and add a kv entry

`consul kv put foo=bar`{{execute T1}}


# Switch back to server, stop tcpdump and grep log for entry

`grep 'foo' /tmp/tcpdump.log`{{execute interrupt T2}}