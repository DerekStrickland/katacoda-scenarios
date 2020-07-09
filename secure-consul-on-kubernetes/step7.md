# Switch back to server shell session and start tcpdump again

`kubectl exec -it katacoda-consul-server-0 -- /bin/sh`{{execute T2}}

# add tcp dump

`apk update`{{execute T1}}
`apk add tcpdump`{{execute T1}}

## IS THIS STILL REQUIRED
`rm -rf /tmp/tcpdump.log`{{execute T2}}

`tcpdump -an portrange 8300-8700 -A > /tmp/tcpdump.log`{{execute T2}}

# Switch to host and add a kv entry

`kubectl exec $(kubectl get pods -l component=client -o jsonpath='{.items[0].metadata.name}') -- consul kv put foo=bar`{{execute T1}}


# Switch back to server, stop tcpdump and grep log for entry

`grep 'foo' /tmp/tcpdump.log`{{execute interrupt T2}}


# start a capture on the server

`tcpdump -an portrange 8300-8700 -A`{{execute T1}}

note the traffic is encrypted

# exit the server container

`exit`{{execute interrupt T1}}