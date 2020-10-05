+++
title = "Creating a Kubernetes cluster"
weight = "2"
+++


Along with this handful of tools, we need a Kubernetes cluster on which to deploy cf-for-k8s. Here are the steps I took with Google Cloud to create a GKE cluster. I am using three nodes with 4vCPU each and 16 GB of RAM.

![gke: cluster basics](cf4k8s/img/gke-1.png "Creating a Kubernetes Cluster on GKE: Cluster Basics")

![gke: node pool](cf4k8s/img/gke-2.png "Creating a Kubernetes Cluster on GKE: Node Pool")

> Please note: The cluster version should be any Kubernetes cluster in the version range 1.16.n to 1.18.n.

> The installation has worked with both 2 and 3 nodes in a cluster. The default docs prescribe a minimum of 5 nodes.

Now that we have all the tools and the cluster on which to deploy, we can begin the actual installation.

## Preparing for the Installation

### Set the kubectl context

In preparation for the installation, please ensure that the context of your kubectl command is set correctly.

![gke: setting kubectl context](cf4k8s/img/gke-3.png "Setting the kubectl context")

You can verify this by using: 

```
kubectl config current-context
```

If you don’t find the cluster name on the console matching your target cluster, please change it. When using Kubernetes with Google Cloud, this command will help you configure the connection:

```
gcloud container clusters get-credentials <CLUSTER-NAME> -—zone <GCP-ZONE> --project <YOUR-PROJECT>
```

#### Why is this needed? 

To be able to direct all the installation steps (and subsequently any governance/instructions that will be needed later). 

### Static IPs

Next, create a static IP.

Let’s answer the why first: this static IP will be externally visible and will serve as the ingress to the Kubernetes cluster. We will use this IP address to refer to the Cloud Foundry endpoint. We will include this IP address as part of the installation process so that the mapping is available as soon as the installation finishes.

On Google Cloud, indicate that you want to create an address along with the region in which you want it to be present. You can also specify a label with which you can identify the address. The exact command is:

```
gcloud compute addresses create cf4k8sdemo --region us-central1
```

Once the static IP address is generated, create a local variable called IP. Assign the value to the local variable. This is for use later when generating deployment YAML files:

```
IP=$(gcloud compute addresses describe cf4k8sdemo --region us-central1 --format=’value(address)’)
```

There are a couple of alternative approaches as well. You can always modify the property of the ingress service post-installation. You can set the static IP as the parameter for the load balancer. The command for doing that is:

```
kubectl patch svc istio-ingressgateway --namespace istio-system --patch ‘{“spec”: { “loadBalancerIP”: “$IP” }}’
```

The other approach is to use hostnames. Obviously, using static IP addresses is not desirable. For the same reason as websites having domain names and not IPs. So, you can use a hostname and configure an ‘A’ record that redirects incoming requests to the ingress/load balancer that manages the Kubernetes cluster. (There will be a second tutorial that will provide instructions on using hostnames. Stay tuned!)