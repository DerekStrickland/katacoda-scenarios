Next, you will execute a Consul CLI command to prove two things:

- RPC traffic is currently unencrypted
- ACLs are not enabled, since you are able to interact with Consul without providing a token

### Log server traffic

This slightly modified version of the `tcpdump` command writes results to a log file.
Run it now so that you can grep for interesting log entries later.

`tcpdump -an portrange 8300-8700 -A > /tmp/tcpdump.log`{{execute interrupt T1}}

Next, generate some Consul traffic using the CLI. This traffic will
orginate from a client machine to the server, and will prove that RPC traffic is in cleartext.
The script sets a Key-Value store entry. This simulates a user setting a value that
may contain sensitive or secret information. This command will execute in a different terminal.

`kubectl exec $(kubectl get pods -l component=client -o jsonpath='{.items[0].metadata.name}') -- consul kv put password=B@n@n@!!`{{execute T2}}

The command succeeds, but notice that you did not pass a `-token` option nor did you set the
`CONSUL_HTTP_TOKEN` environment variable. One or the other is required when ACLs are enabled.
This proves that ACLs are not enabled.

### View the log file

Now, from the original terminal you can search the log file for the CLI operation with the following command:

`grep 'password' /tmp/tcpdump.log`{{execute interrupt T1}}

You are able to see the Key-Value store entry in cleartext. This proves that RPC traffic
is not encrypted.

```shell
....Seqr.ServiceMethod.KVS.Apply..Datacenter.dc1.DirEnt..CreateIndex..Flags..Key.password=B@n@n@!!.LockIndex..ModifyIndex..Session..Value..Op.set.Token.
```

Exit the server container.

`exit`{{execute T1}}
