# Switch back to server shell session and start tcpdump again

`kubectl exec -it katacoda-consul-server-0 -- /bin/sh`{{execute T2}}

# add tcp dump

`apk update`{{execute T2}}

`apk add tcpdump`{{execute T2}}


## IS THIS STILL REQUIRED

`tcpdump -an portrange 8300-8700 -A > /tmp/tcpdump.log`{{execute T2}}

# Switch to host and add a kv entry

`kubectl exec $(kubectl get pods -l component=client -o jsonpath='{.items[0].metadata.name}') -- consul kv put apple=banana`{{execute T1}}

FAILS CAUSE NO TOKEN

`kubectl exec $(kubectl get pods -l component=client -o jsonpath='{.items[0].metadata.name}') -- consul kv put -token $(kubectl get secrets/katacoda-consul-bootstrap-acl-token --template={{.data.token}} | base64 -d) foo=bar`{{execute T1}}

Works cause we passed the secret

# Switch back to server, stop tcpdump and grep log for entry

`grep 'foo' /tmp/tcpdump.log`{{execute interrupt T2}}


# start a capture on the server

`tcpdump -an portrange 8300-8700 -A`{{execute T2}}

note the traffic is encrypted

# exit the server container

`exit`{{execute interrupt T1}}