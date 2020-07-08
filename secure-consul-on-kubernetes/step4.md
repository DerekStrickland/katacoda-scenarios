
# connect to server container

`kubectl exec -it katacoda-consul-server-0 -- /bin/sh`{{execute T1}}

# add tcp dump

`apk update`{{execute T1}}
`apk add tcpdump`{{execute T1}}

# start a capture on the server

`tcpdump -an portrange 8300-8700 -A`{{execute T1}}

note the traffic is encrypted

# exit the server container

`exit`{{execute interrupt T1}}
