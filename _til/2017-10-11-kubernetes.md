---
title:  "Kubernetes 101"
date:   2017-10-11 13:05:00
category: til
tags: ["kubernetes", "docker", "orchestration", "container", "containers", "devops", "deployment"]
---

TIL about Kubernetes, Google's open source container orchestration system.

I spent a half-day at Google's office space today learning about [Kubernetes][k]{:target="_blank"} from the folks at Apprenda. Going in to the session, I thought I had a grasp on what Kubernetes was, but quickly realized I had many misconceptions.

We worked through the following repository, provided by Apprenda, for learning some of the basics of Kubernetes: [https://github.com/apprenda/hands-on-with-kubernetes-gke][app]{:target="_blank"}.

Here are some things that I learned from the session:

## Kubernettes !== Docker

I thought Kubernetes was just a different way to build container images like you can with Docker... not so!

Kubernetes is a **container orchestration system**: in other words, it provides the necessary components for configuring, deploying and scaling images that are built with Docker.

**You still need to use a system like Docker or rkt to build images of your application, before you can deploy them to a kubernetes cluster**. Kubernetes is only responsible for the orchestration of containers (running images) not the building of those images themselves.

<figure>
	<img src="/assets/images/k8-architecture.jpg">
	<figcaption>Kubernetes block diagram (provided by Apprenda)</figcaption>
</figure>

Some of the active components in a kubernetes cluster are:
  - etcd - a distributed key-value store for cluster config
  - Master node - contains common orchestration
  - Worker node - where your images are run.
  - Pods - A running image / set of images
  - API server - processes all REST based requests to the cluster (runs on master node)
  - Scheduler - Spins up new pods on available infrastructure, as requested
  - Controller manager - ensures that the desired number of nodes in your cluster matches the actual active number
  - Kubelet - Reports back health of pods to master node (runs on worker node)
  - Kube proxy - network proxy and load balancer for services on a worker

An analogue to the Docker world is [docker swarm][swarm]{:target="_blank"}, which is docker's container orchestration solution. Apache offers [Mesos][mesos]{:target="_blank"}, which is yet another container orchestration solution.

The session today focused on Kubernetes, and the folks from Apprenda who were leading the class were very bullish on it compared to other available options. In addition to being backed by the likes of Google and Microsoft, since it is an open sourced solution it is possible for you to **deploy a kubernetes cluster outside of a kubernetes compatible cloud environment**.

This means if you have security requirements that prevent you from deploying in the cloud, or you are deployed in the cloud and want to transition to an in-house solution, **it is easier to do so than if you are working with a non-open-sourced orchestration solution like Amazon ECS.

## Google Cloud Platform

We also used Google Cloud Platform (GCP) for the hands on portion of the kubernetes session. **GCP offers kubernetes cluster deployment solutions right out of the box**, so it's insanely easy to get applications deployed and running.

If you wanted to do the same on Amazon infrastructure, you would need to spin up and host your own kubernetes components manually, which is clearly more work upfront. You could also use Amazon ECS, but that comes with its own set of tradeoffs...

## Amazon ECS

Amazon offers its own container orchestration solution with its [ECS][ecs]{:target="_blank"} offering. We used ECS at Expedia to deploy our application images to the public-facing internet.

I learned through the session today that **the architecture of ECS is different than that of Kubernetes**. It is a closed-source, proprietary solution that Amazon offers you to run containers on. Since it is closed-source, it would be more of a pain to operate a similar container cluster locally if you needed to do so.

**You are locked in to the Amazon ecosystem if you go this route**, which might be ok if the rest of your stack runs on Amazon infrastructure.

## Conclusion

Overall it was a well spent morning learning about Kubernetes today. The session definitely helped clear up some misconceptions I had about orchestration solutions available. I'm excited to bring this knowledge with me as I begin a new role at Uptake on the 23rd.

[k]: https://kubernetes.io/
[app]: https://github.com/apprenda/hands-on-with-kubernetes-gke
[swarm]: https://docs.docker.com/engine/swarm/
[mesos]: http://mesos.apache.org/
[ecs]: https://aws.amazon.com/ecs/
