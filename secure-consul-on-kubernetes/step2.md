# connect to consul server container

`kubectl exec -it katacoda-consul-server-0 -- /bin/sh`{{execute T1}}

# get tcpdump

`apk update`{{execute T1}}

`apk add tcpdump`{{execute T1}}

# start a capture on the server

`tcpdump -an portrange 8300-8700 -A > /tmp/tcpdump.log`{{execute T1}}

# generate some traffic in a new terminal

`kubectl exec katacoda-consul-server-0 -- consul kv put foo=bar`{{execute T2}}

# view sensitive info in plain text

`grep 'foo' /tmp/tcpdump.log`{{execute interrupt T1}}

Note the traffic is in clear text

# prove that ACLs are not enforced

`kubectl port-forward katacoda-consul-server-0 8500:8500`{{execute T3}}

`consul members`{{execute T2}}
