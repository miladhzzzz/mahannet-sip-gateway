# Solution Proposal for Mahan Net to Move VoIP Traffic to Kubernetes
# Infrastructure
To move the VoIP traffic of Mahan Net to Kubernetes, we will use Terraform with vSphere provider to store the infrastructure code in Github. We will also use Github Actions to build the infrastructure as PRs come in. We will have two clusters - one for Dev/Test and QA and another for Production. Both the clusters will have vSphere cluster autoscaler on them since we use vSphere infrastructure in the underlying layer.
# CI/CD Pipeline
To build the SIP servers that are asterisk, we will use Ansible as the entry point of Docker files to build the asterisk servers. We will use ArgoCD for our CI/CD pipeline, and we will decide on the CI platform.
# Load Balancing
Since load balancers that don't have SIP channel signaling affinity cannot be used, we will use a Go program that's affinity aware. This program can be horizontally scaled as a gateway for our VoIP traffic and makes tracing each connection on packet level available.
# Networking
Kubernetes has a few requirements for any networking implementation. It imposes the following fundamental requirements on any networking implementation, barring any intentional network segmentation policies:
Make your HTTP (or HTTPS) network service available using a protocol-aware configuration mechanism, that understands web concepts like URIs, hostnames, paths, and more.
The Ingress concept lets you map traffic to different backends based on rules you define via the Kubernetes API.
The EndpointSlice API is the mechanism that Kubernetes uses to let your Service scale to handle large numbers of backends, and allows the cluster to update its list of healthy backends efficiently.
If you want to control traffic flow at the IP address or port level (OSI layer 3 or 4), NetworkPolicies allow you to specify rules for traffic flow within your cluster, and also between Pods and the outside.
# Security
We recommend securing the Kubernetes cluster to protect it from accidental or malicious access. Kubernetes expects that all API communication in the cluster is encrypted by default with TLS. We also recommend enabling Kubelet authentication and authorization, especially on Production clusters.
# Multi-Tenancy
We recommend using Kubernetes constructs like network QoS, storage classes, and pod priority and preemption to provide tenants with the quality of service that they paid for. The Kubernetes bandwidth plugin creates an environment where all pods on a node share a network interface.
# Add-ons
We can use add-ons to extend the functionality of Kubernetes. For networking and network policy, we can use CNI-Genie, which enables Kubernetes to seamlessly connect to a choice of CNI plugins, such as Calico, Canal, Flannel, or Weave. We can also use OVN-Kubernetes, which is a networking provider for Kubernetes based on OVN (Open Virtual Network).
By following these recommendations, we can successfully move the VoIP traffic of Mahan Net to Kubernetes.