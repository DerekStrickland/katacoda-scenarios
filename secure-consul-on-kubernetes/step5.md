Next, you will set necessary configuration options now that TLS is enabled.

### TLS Changes

Configure Kubernetes to forward the HTTP port (8500) to your development host so that
you can interract with the Consul CLI from the development host. Note port 8501 is being
forwarded. With TLS enabled, Consul uses port 8501 for HTTPS traffic.

`kubectl port-forward --address 0.0.0.0 katacoda-consul-server-0 8500:8501`{{execute T3}}

You must also set the CONSUL_HTTP_SSL environment variable to true on any host attempting
to execute Consul CLI commands. Do that now with the following command:

`export CONSUL_HTTP_SSL=true`{{execute T1}}

Now, execute `consul members`. Note you must provide Consul with a way to verify TLS
connections. In this example, you are providing the CA as a CLI option.

`consul members -ca-file consul-agent-ca.pem`{{execute T1}}

You now observe a list of all members of the service mesh. This
proves that TLS is being enforced.
