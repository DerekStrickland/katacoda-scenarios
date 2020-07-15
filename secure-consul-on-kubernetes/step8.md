### Deploy sample services

Now that the network communications have been secureed, and ACLs have been applied,
you will configure zero-trust networking using Consul intentions. Run the following
command to deploy a sample backend service to the cluster.

`kubectl apply -f server.yaml`{{execute T1}}

Now, deploy a downstream client.

`kubectl apply -f client.yaml`{{execute T1}}

Next, make sure all pods are running before proceeding to the next section.

`watch kubectl get pods`

### Create a deny all intention

TODO:  Verify this isn't already the case - don't see it in the UI
Next, create a deny all intention so that no services can communicate without
explicit permission.

`consul intention create -ca-file consul-agent-ca.pem -deny "*" "*"`{{execute interrupt T1}}

Run the following command to validate that the deny all intention is enforced

`kubectl exec static-client static-client -- curl -s http://127.0.0.1:1234/ `{{execute T1}}

Observe that the command exits with a non-zero exit code.

<code class="execute">
Defaulting container name to static-client.
Use 'kubectl describe pod/static-client -n default' to see all of the containers in this pod.
command terminated with exit code 7
</code>

Next, create an allow intention for the client to server traffic.

`consul intention create -ca-file consul-agent-ca.pem -allow static-client static-server`{{execute T1}}

Finally, validate the intention allows traffic to from the client to the server.

`kubectl exec static-client static-client -- curl -s http://127.0.0.1:1234/ `{{execute T1}}

Notice the output now includes "hello world". This proves the intention is now allowing
traffic between the client and the server.

<code class="execute">
Defaulting container name to static-client.
Use 'kubectl describe pod/static-client -n default' to see all of the containers in this pod.
"hello world"
</code>
