Hello! My name is Michał Dulko and I'm an engineer in the OpenShift on OpenStack
team at Red Hat.

We've spent considerable amount of time integrating and validating
cloud-provider-openstack and LoadBalancer Services with OpenShfit and we've
discovered quite a few things in Octavia that affect that integration. This
presentation will share these experiences.

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

Let's talk about Octavia itself. So I think the main theme on the "hate" side
of Octavia-Kubernetes relationship is that Octavia is not a perfect abstraction
layer. The default and reference implementation of an Octavia backend (called
"provider") is Amphora. Amphora backs each LB with a VM with HAProxy. This is
simple, but resource-consuming and time to create an LB is also significant.
Moreover due to design decisions there's no source IP preservation.

We also have OVN Octavia Provider which uses Neutron's OVN instance to
configure the LBs. This is fast, this has source IP preservation, but there
are functional gaps in older versions that affect K8s. To present them we need
to understand how LoadBalancer Services work in K8s.


