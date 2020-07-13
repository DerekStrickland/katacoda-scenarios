Now, you will verify that gossip encrytion and TLS are enabled, and that
ACLs are being enforced.

First, configure Kubernetes to forward the HTTP port (8500) to your development host.

`kubectl port-forward --address 0.0.0.0 katacoda-consul-server-0 8500:8500`{{execute T3}}

Now run consul members to retrieve a list of service mesh members.

`consul members`{{execute T1}}

The command fails with the following message.

```plaintext
Error retrieving members: Get "http://127.0.0.1:8500/v1/agent/members?segment=_all": EOF
```

This is the first indicator that TLS is being enforced. Once TLS is enabled, Consul uses port
8501 for communications. Change your port forward command to forward from 8501 in cluster
to 8500 on host:

`kubectl port-forward --address 0.0.0.0 katacoda-consul-server-0 8500:8501`{{execute interrupt T3}}

Now, try to list consul members again.

`consul members`{{execute T1}}

You should receive an error in output as follows:

```shell
Unexpected response code: 400 (Client sent an HTTP request to an HTTPS server.)`
```

This is another indicator that TLS is being enforced. You must set the CONSUL_HTTP_SSL
environment variable to true on any host attempting to execute Consul cli commands.
Do that now.

`export CONSUL_HTTP_SSL=true`{{execute T1}}

Now, try to list Consul members again.

`consul members`{{execute T1}}

You should receive a new error in output as follows:

```shell
Error retrieving members: Get "https://127.0.0.1:8500/v1/agent/members?segment=_all": x509: certificate signed by unknown authority
```

This error message indicates that connectivity is now being established, but
communication is being rejected because the TLS handshake can't be negotiated.
In order to convince the server that the client communication is authorized,
the client needs to supply a ca-file cli option. Run the following command,
and supply the consul-agent-ca.pem file you created earlier in the lab.

`consul members -ca-file consul-agent-ca.pem`{{execute T1}}

You should now receive a list of all members of the service mesh. The
actions you performed in this section of the lab proved that TLS is now
being enforced on a connectivity level.
