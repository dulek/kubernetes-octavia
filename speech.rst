Hello! My name is Michał Dulko and I'm an engineer in the OpenShift on OpenStack
team at Red Hat.

I was mainly responsible for the Kuryr-Kubernetes project where we used Octavia
to implement handling of Kubernetes Services. So naturally, as a seasoned
Octavia user, I was tasked with overseeing OpenShift's integration with
cloud-provider-openstack and Octavia. This presentation will share my
experiences.

I'm going to only talk about integration of Kubernetes `type=LoadBalancer`
Services with OpenStack Octavia and a bit about MetalLB later on.

I'm not going to talk about Ingress and octavia-ingress-controller even though
it's also a part of cloud-provider-openstack. cluster-api-provider-openstack
and its use of Octavia to create an API LB isn't in this presentation either.

There was a split of cloud providers and while you might be tempted to run the
in-tree one integrated in kube-controller-manager, please just don't. It's gone
in K8s 1.26 and the support for Octavia was implemented as a hack possible only
because Octavia API v2.0 is the same as Neutron LBaaS v2. This means legacy
provider is missing features that are critical for K8s. I'll highlight them
later on.
