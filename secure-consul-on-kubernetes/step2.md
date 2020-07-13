To verify that network traffic is in cleartext you will inspect it.

### View server traffic

To view network traffic, connect to the consul server container, and observe
its traffic using the `tcpdump` program. You should now have a container running named
`katacoda-consul-server-0`. Connect to it with the following command:

`kubectl exec -it katacoda-consul-server-0 -- /bin/sh`{{execute T1}}

The container images used by this hands-on lab are lightweight alpine images. They ship with
limited tools. Run the following commands to install `tcpdump`:

`apk update`{{execute T1}}

`apk add tcpdump`{{execute T1}}

Now start `tcpdump` to view traffic to the server container. This script limits the port range
to the range of ports used by Consul.

`tcpdump -an portrange 8300-8700 -A`{{execute T1}}

If you inspect the output you will observe that the traffic is in cleartext.
Note the UDP operations. These entries are the gossip protocol at work.
This proves that gossip encryption is not enabled.
