
# kubernetes-sysdig-wrapper

Hello! I am a little container to run `sysdig/agent` on the nodes of a Kubernetes cluster.

## The Problem

Sysdig Cloud requires the `kubelet` binary on the nodes in your cluster to have been started with
`--host-network-sources=*`, so that it can inspect the host's network traffic.

This isn't always easy to do, though -- in [our](http://www.csats.com) case, we were using the
[CoreOS Kubernetes AWS scripts][coreos] and didn't want to mess with the internals of it. Instead,
we run this container, which manually creates a `sysdig/agent` container on the host.

## Usage

You'll want to create a DaemonSet that runs this container on all of your nodes. See
`daemonset.example.yaml` for one way this could work.

## Requirements

* The host's `/var/run/docker.sock` must be mounted in that same directory in the container.
* Your Sysdig Cloud API key must be mounted at `/key/sysdig-api-key.txt`.
* Docker 1.8.3+ on the node.

## Limitations

* Kubernetes won't actually know about the `sysdig/agent` container. You won't be able to check
  the Kubernetes API to see if it's running or anything.

* If it crashes, you may not know about it.

## License

MIT

[coreos]: https://coreos.com/kubernetes/docs/latest/kubernetes-on-aws.html
