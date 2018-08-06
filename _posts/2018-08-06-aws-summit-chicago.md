---
title:  "AWS Summit Chicago Recap"
date:   2018-08-06 12:28:00
category: post
tags: [aws, summit, chicago, kubernetes, eks, istio, canary, deployments, sagemaker]
---

Last Thursday was the AWS Summit Chicago. I attended 3 sessions on AWS Fargate, Canary Deployments with Istio, and AWS Sagemaker.

## Session 1: AWS Fargate

[Fargate][fargate]{:target="_blank"} is a relatively new mode you can choose when you're deploying containers to ECS or EKS. It removes the necessity to specify and configure server specifications, by creating pre-packaged configurations for you that are optimized for most workloads. This allows you to focus purely on your application code instead of on the infrastructure your containers will run on.

Contrast this with EC2 mode, which requires that you specify server types, scaling options, and provision these in a way that you are not wasting money. Most applications do not require this level of control, though, and for teams that are just starting out or are validating prototypes with users, Fargate mode seems to be a simple way to get your application running in an efficient pre-configured way.

## Session 2: Canary Deployments with Istio

[Istio][istio]{:target="_blank"} is a service mesh that is complimentary to kubernetes. It provides additional routing control, security standardization, and telemetry than kubernetes provides out of the box.

A `canary deployment` is a way of verifying that a new version of your application will perform well in a production environment, by directing a small amount of production traffic at the new version and collecting metrics on how it is performing compared to the old version. If anything goes wrong, traffic can be directed back to old versions that are still deployed to prod and customers shouldn't experience any outages.

This session covered how one might perform canary deployments with vanilla kubernetes, and then additionally with istio:

With `kubernetes` alone, **it is possible to achieve a percentage redirect of traffic to a new version by scaling the old and new application containers independently in your cluster**. For example, if I wanted to redirect 20% of traffic to v2 and leave the remaining 80% directed to v1, I could scale my v1 containers to `8` and my v2 containers to `2`. This would effectively achieve an 80/20 split between the two versions.

Where this falls apart, though, is when you want to achieve something more fine grained (say 1% to a new version and 99% to another version). This would require you to spin up 99 containers of the old version, and one container of the new version in your cluster, which would likely result in a ton of idle compute time if your containers do not normally hover around this scale.

With `Istio`, which has more telemetry and capability around routing in your cluster, it is possible to specify percentage splits of traffic in the service mesh configuration. This allows your cluster to scale container instances as needed, instead of creating potentially unnecessary copies just to achieve a certain probability of routing to it.

A final thought from this session: it is important when designing a canary deployment, that you are casting a wide enough net over your traffic to ensure that all of your user types are contained in the canary group. For example, if you perform a canary deployment at midnight CST, but most of your users are sleeping at that time, you might not get a representative sample of your traffic directed at your canary. This can lead to a green light to fully roll out the canary in prod, only to realize after the fact that a serious performance issue appeared for a critical user type.

Ensuring that all user types are contained in a canary deploy test seems similar to designing an A/B test for a UI. Similarly, if you are designing a canary deploy for a UI related change, you would likely need to get more sophisticated than just a pure percentage traffic redirect to ensure users receive a consistent experience and are not randomly swapped between the old and new versions. This type of sophistication might not be necessary for backend changes.

## Session 3: AWS Sagemaker

[Sagemaker][sagemaker]{:target="_blank"} is an Amazon offering that allows you to build,  train, and deploy machine learning models in the AWS cloud. Sagemaker is framework agnostic, allowing you to build your models using any number of popular machine learning frameworks (Tensorflow, SKlearn, R, etc...).

Sagemaker offers a jupyter notebook environment for the development of models and scripts, that are hooked up to AWS cloud compute resources to get quick feedback as you build.

It also offers custom implementations of common machine learning algorithms that are optimized to run in the amazon cloud. Current offerings include: K-means clustering, PCA, LDA, and more... These custom implementations maximize the cost to performance ratio for your model training, allowing your models to train faster for less cost.

Sagemaker seems like an interesting way to build high performing machine learning algorithms, as it abstracts away much of the engineering challenges of the Build/Test/Deploy process, allowing you to focus more on model analysis where the bulk of the value for your customers lies.

[fargate]: https://aws.amazon.com/fargate/
[istio]: https://istio.io/
[sagemaker]: http://aws.amazon.com/sagemaker/

