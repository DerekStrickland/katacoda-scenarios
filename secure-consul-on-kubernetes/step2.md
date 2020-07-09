To verify that network traffic is in cleartext you should inspect it.

### View server traffic

To view network traffic, you can Connect to the consul server container, and observe
its traffic using the `tcpdump` program. You should now have a container running named
`katacoda-consul-server-0`. Connect to it with the following command:

`kubectl exec -it katacoda-consul-server-0 -- /bin/sh`{{execute T1}}

The container images used by this hands-on lab are lightweight alpine images. They ship with
limited tools.  Install `tcpdump` to the image run the following commands:

`apk update`{{execute T1}}

`apk add tcpdump`{{execute T1}}

Now start a `tcpdump` capture on the server container. This script limits the port range
to the range of ports used by Consul, so as to filter out unrelated traffic. Traffic
occurs quickly and frequently.

`tcpdump -an portrange 8300-8700 -A`{{execute T1}}

Inspect the output, and you will notice that the traffic is in cleartext.
The majority of the traffic you are obvserving at this point is gossip traffic, specifically
the entries tagged as UDP traffic. This proves that gossip encryption is not enabled.

### Log server traffic

This slightly modified version of the tcpdump command writes results to a log file.
Run it now so that you can grep for specific, interesting log entries.

`tcpdump -an portrange 8300-8700 -A > /tmp/tcpdump.log`{{execute interrupt T1}}

Now, let's generate some HTTP API traffic in a different terminal. This traffic will come
orginate from a client machine to the server and will prove that RPC traffic is also in cleartext.
In this situation, the script sets a Key-Value store entry which, in a production scenario, may
very well contain sensitive or secret information.

`kubectl exec $(kubectl get pods -l component=client -o jsonpath='{.items[0].metadata.name}') -- consul kv put foo=bar`{{execute T2}}

### View the log file

Now, from the original terminal which is currently attached to the server container shell, you can
search the log file with the following command:

`grep 'foo' /tmp/tcpdump.log`{{execute interrupt T1}}

Note that you are able to see the Key-Value store entry in cleartext. This proves that RPC traffic
is not encrypted.

Exit the server container.

`exit`{{execute T1}}