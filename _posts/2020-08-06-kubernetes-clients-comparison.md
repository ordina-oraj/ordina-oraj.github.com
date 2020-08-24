---
layout: post
authors: [yolan_vloeberghs,pieter_vincken]
title: 'Kubernetes clients and dashboards: a comparison'
image: /img/2020-08-06-kubernetes-clients-comparison/banner.jpg
tags: [Kubernetes,kubectl,tools]
category: Cloud
comments: true
---

## Table of Contents

* [Introduction](#introduction)
* [K9s](#k9s)
* [Octant](#octant)
* [Lens](#lens)
* [Kubenav](#kubenav)
* [Infra.App](#infra-app)
* [Conclusion](#conclusion)

## Introduction

Imagine the following scenario: you're writing code for your amazing new take-your-bike-to-work platform and you've just finished implementing a new feature to allow users to send unicorns to each other.
Your CI/CD pipeline has nicely tested, packaged and deployed the updates to your development Kubernetes cluster, you load the URL and are greeted by a very nice error page stating "Oops, my bad, we lost some unicorns".
Clearly you broke something somewhere.
Most of the time, this means you'll open up a terminal, run some commands to login into the cluster and start firing two dozen `kubectl`-commands in order to figure out which micro service broke and check the logs to figure out where your code has broken.
You make some changes to the broken service and push your code to the repository and the CI/CD flow takes over again.

This process works quite nicely, but figuring which service is broken and which logs to check can be quite a challenge.
Typing the `kubectl`-commands into the terminal probably takes half of the time you spend on debugging the issue.
As developers are always optimizing their workflow, using `kubectl` just takes to much time, even with the `k` alias for the command and perfect auto-complete features.

As the famous mantra goes anything worth doing twice is work automating.
Therefor quite some tools were created in order to make this process of navigating through a cluster easier than typing a lot of commands.

This blog post aims to provide a very brief overview a some of the more common tools that are available as replacements or additions to `kubectl` to allow developers to look into a Kubernetes cluster.
All tools can be installed locally and don't require any components to be installed in the cluster to operate.

## K9s

[K9s](https://github.com/derailed/k9s){:target="_blank" rel="noopener noreferrer"} is a Kubernetes client built by [Fernand Galiana](https://twitter.com/kitesurfer){:target="_blank" rel="noopener noreferrer"}.
The client is fully terminal based so you'll be using only your keyboard when operating it.
For those who are familiar with Vim, you'll feel right at home in K9s.
It uses similar hotkeys to the popular editor.
There is a (quite steep) learning curve when you start using this client, but once you've read the brief readme on the projects home page and memorize the commands you'll use the most, it's an absolute joy to use.
It feels like using `kubectl` without the requirement to type all commands every time you need to get a deployment.
The tool is quite feature-rich at the time of writing.
You can port-forward, view secrets in plain text, edit resources directly, and "drill-down" from deployments into the logs of a container.

`:deploy` takes you to the pod overview, `<enter>`-ing into the deployment takes you to all pods in that deployment.
`<enter>`-ing again into a pod reveals all containers, including (completed) init-containers.
`<enter>`-ing a final time into a pod takes you straight into a view with live logs.
This hierarchical approach feels very natural and follows the architectural design of Kubernetes.
A similar approach can be used for service (`:svc`), statefulsets (`:sts`) and deamonsets(`:ds`).

Another very familiar shortcut is the usage of `/` to filter on the context you're currently in.
This works on basically any screen where you'd expect it, even in the logs view!

It feels very natural using the tool after a few days of use.
However, for those of us who rather use their mouse to navigate through resources and hate memorizing commands, this tool is not for you.

The project is still under very active development and quite some people are contributing to the codebase.
Since the project doesn't seem to be backed by a company directly, there are no real support guarantees nor is there a fixed release schedule.
The maintainer however accepts fixes quite fast and releases are very frequent, sometimes multiple a day.

Demo: go from deployment to all the way into pod logs
<img alt="k9s from deployment to pod logs" src="{{ '/img/2020-08-06-kubernetes-clients-comparison/k9s-deploy.gif' | prepend: site.baseurl }}" class="image fit" style="margin:0px auto; max-width: 1000px;">

Demo: Switch between 2 Kubernetes contexts
<img alt="k9s context switch" src="{{ '/img/2020-08-06-kubernetes-clients-comparison/k9s-ctx.gif' | prepend: site.baseurl }}" class="image fit" style="margin:0px auto; max-width: 1000px;">

Demo: Find out where a configmap is used
<img alt="k9s config map usage" src="{{ '/img/2020-08-06-kubernetes-clients-comparison/k9s-cm.gif' | prepend: site.baseurl }}" class="image fit" style="margin:0px auto; max-width: 1000px;">

## Octant
[Octant](https://github.com/vmware-tanzu/octant){:target="_blank" rel="noopener noreferrer"} is an open-source Kubernetes web dashboard built by VMWare Tanzu.
It is written in Go, which is a trending programming language made by Google. 
It has lots of features, a clean user interface and has the support of a big company in the IT industry behind it.
This dashboard runs locally, which means you do not need to install it on your cluster, unlike the official Kubernetes dashboard. 

Installing and accessing the dashboard is very straight-forward; it is as easy as running an install command (on Mac, I used Homebrew) and then running the `octant` command from your terminal.
Once it's running, you will find that the navigation is simple. I didn't need to look around and search a lot in order to find what I need.

You are greeted by the appplication with a nice and well-ordered summary of your cluster resources.
Navigating through the dashboard feels very comfortable and swift. They offer both a dark and light theme, which you will be able to see down below.

This client offers features that other traditional clients have aswell such as log streams, a graphical display of all resources, switch between clusters, ... .
It does have unique features aswell, such as in-app port forwarding (say goodbye to `kubectl port-forward`!).

<img alt="Octant port forwarding" src="{{ '/img/2020-08-06-kubernetes-clients-comparison/octant-pf.gif' | prepend: site.baseurl }}" class="image fit" style="margin:0px auto; max-width: 1000px;">
<p style="text-align: center; font-style: italic;">Octant port forwarding</p>

The most singular feature they have is their plugin system, which allows you to design plugins based on the information that you want on your interface. 
Although this requires knowledge of Go and their plugin system, you can customize your experience entirely to the way that you want it to be.
For example, there is a [Helm plugin](https://github.com/bloodorangeio/octant-helm){:target="_blank" rel="noopener noreferrer"} available and Jenkins X has a repository of Octant [plugins](https://github.com/jenkins-x/octant-jx){:target="_blank" rel="noopener noreferrer"}.

If you are a fan of web dashboards, then you should definitely give Octant a try. Once you start to work with it, you will feel that it has a lot of potential. 
If you do prefer something more natively and working in a seperate application window, then I suggest you look further and read more in this blog post.

<img alt="Octant slideshow" src="{{ '/img/2020-08-06-kubernetes-clients-comparison/octant.gif' | prepend: site.baseurl }}" class="image fit" style="margin:0px auto; max-width: 1000px;">
<p style="text-align: center; font-style: italic;">Octant slideshow</p>

## Lens

[Lens app](https://github.com/lensapp/lens){:target="_blank" rel="noopener noreferrer"} is a Kubernetes client with a proper GUI.
It was created by Kontena Inc and later [sold to Mirantis](https://techcrunch.com/2020/08/13/mirantis-acquires-lens-an-ide-for-kubernetes/){:target="_blank" rel="noopener noreferrer"}, the owners of Docker Enterprise.

When first starting Lens, it immediately feel very easy to use.
Adding a cluster can be done by hitting the `+` and selecting a cluster from the dropdown.
Lens leverages the contents of the Kubeconfig it finds on the system to discover and authenticate with clusters.
This way no additional magic is needed to get started.

After connecting to a cluster, you're dropped into the cluster overview (see screenshot).
This view provides you with an easy overview of the resources within the cluster and (super useful) provides a list of the last seen error events in the cluster.

Navigating to the list of pods provides an overview of all pods in the cluster.
By clicking on a pod you're provided with the details of that pod (`kubectl describe`).
From here, you can directly dive into the pod logs, shell into the pod, make edits or remove the pod from the cluster.

Similar support is available for most common resources within the cluster: Statefulsets, deployment, configmaps, secrets, ...
The workflow is always the following: open the type in the sidebar on the left, click on an object to get details.
From that details view, certain actions can be performed on the object.

As with most of the tools in this comparison, Lens is quite feature-rich.
Most common types are support and common actions are available.
But Lens has another trick up it's sleeve which makes it different from the other tools: Metrics/Prometheus integration.

The integration relies on a Prometheus instance being installed in the cluster that exposes the supported metrics.
You can opt for Lens to install Prometheus (and other required components) for you, but in real scenarios you either don't have those rights or you'll already have a Prometheus installed in the cluster.
In those cases, you can just [add some configuration](https://github.com/lensapp/lens/blob/master/troubleshooting/custom-prometheus.md){:target="_blank" rel="noopener noreferrer"} to that instance and point the Lens app to that Prometheus instance.
It will use port-forwarding under the hood, so no need to expose the Prometheus instance to the outside.
The integration works nicely and instantly provides some metrics about your cluster and deployed components.
This provides good insights for developers to figure out their resource consumption without leaving their Kubernetes client.
The charts and data seem to be very rudimentary, but improvement are expected to arrive over time.

As most of the client described in this post, Lens app is an open-source project.
There seems to be a group / company behind the development, but no paid / supported version is available at this time.
There is very active development happing on the app and releases are about one month apart, so bugs and new features should be available regularly.

Screenshot: List of pods in Lens
<img alt="Lens pod list" src="{{ '/img/2020-08-06-kubernetes-clients-comparison/lens-pod-list.png' | prepend: site.baseurl }}" class="image fit" style="margin:0px auto; max-width: 1000px;">

Screenshot: Details about a pod in Lens, including Prometheus supplied metrics
<img alt="Lens pod details" src="{{ '/img/2020-08-06-kubernetes-clients-comparison/lens-pod-details.png' | prepend: site.baseurl }}" class="image fit" style="margin:0px auto; max-width: 1000px;">

Screenshot: Overview of a cluster in Lens, including the last error events
<img alt="Lens cluster overview" src="{{ '/img/2020-08-06-kubernetes-clients-comparison/lens-cluster-overview.png' | prepend: site.baseurl }}" class="image fit" style="margin:0px auto; max-width: 1000px;">

## Kubenav
[Kubenav](https://github.com/kubenav/kubenav){:target="_blank" rel="noopener noreferrer"} is a rich featured, open source Kubernetes client created in early 2020 to manage your cluster(s) with. 
The application is under active development by the open source community on their GitHub repository.
There is cross-platform functionality, which means they provide you a desktop AND mobile client, which is a unique feature in the world of Kubernetes clients. 
They share the same codebase, so the navigation should be similar on all platforms.

The navigation menu is self-explanatory as it categorizes every Kubernetes resource, which you can then filter by namespace in the top-right corner.
They support all widely used Kubernetes resources and the status / configuration of those resources. 
You can easily switch between Kubernetes clusters by using the drop-down menu available in the menu.

There is a window which is available at all times where you can consult different deployment logs and a terminal. Each window is categorized under a tab.
When you minimize this window, you will notice a blue shell icon in the right bottom corner of Kubenav, which allows to open your current logs or terminal again.

The app is made in the Ionic Framework using Capacitator as cross-platform runtime, which is something you notice when you actually start using the app.
I don't really like this feeling, as I prefer my clients to have a more native feeling, although I understand why the developers chose for this approach.

The primary factor of why I would use this application is because they support almost every resource combined with a GUI if you don't like using the terminal.
However, I'm not really a fan of the interface in general, as it seems to be messy and overwhelming at times when a resource provides you with a great deal of information in an uncategorized way.
If you want to know every detail about your resource at all times, then this application is definitely for you. I didn't have a reason yet to use the mobile client, but I'm sure that depends on your own use case.

<img alt="Kubenav slideshow" src="{{ '/img/2020-08-06-kubernetes-clients-comparison/kubenav.gif' | prepend: site.baseurl }}" class="image fit" style="margin:0px auto; max-width: 1000px;">
<p style="text-align: center; font-style: italic;">Kubenav slideshow</p>

## Infra App
[Infra App](https://infra.app/){:target="_blank" rel="noopener noreferrer"} is a new addition to the list of Kubernetes clients.
It is made by the people over at Docker Desktop & [Kitematic](https://kitematic.com){:target="_blank" rel="noopener noreferrer"} and is being developed behind closed doors, which has been addressed as unpleasant within the Kubernetes community.
It provides you with a clean, simplistic user interface which groups everything you need to know about a single resource together.
Everything speaks for itself and all the information you need is within the reach of a few simple clicks.

When you open the application for the first time, they greet you by asking you to fill in your e-mail address.
This is probably for newsletters and updates, but I wish this step was optional.

You quickly notice that there is only basic functionality available in the application, which makes sense as the client is still in early access at the time of writing.
You can browse resources per namespace, go through application logs, read and edit YAML configuration and check the current resources used by your deployment.
There is a metrics interface for the whole cluster aswell, which supplies you with a structured and detailed view about your nodes.

The application is still very young and right now, it is lacking some functionality that you might expect or find in other clients.
If you want something with more than basic functionality right now, this might not be the application you are looking for.
However, if it has what you need, you will find that it will be very easy and straight-forward to manage your Kubernetes cluster with this client.


<img alt="Kubenav slideshow" src="{{ '/img/2020-08-06-kubernetes-clients-comparison/infra-app.gif' | prepend: site.baseurl }}" class="image fit" style="margin:0px auto; max-width: 1000px;">
<p style="text-align: center; font-style: italic;">Infra slideshow</p>

## Conclusion

Now the real question, which client should you use.
As with any question about software, it depends.

If you are using some software that has plugins available for Octant, definitely give it a try.
The [plugins](https://github.com/topics/octant-plugin){:target="_blank" rel="noopener noreferrer"} add a lot of value to the tool and might make it a very compelling option for your use-case.

If you like to be lightning fast and don't mind struggling though a steep learning curve, K9s might be a tool for you.
It's the personal favorite of the authors this post, meanly because of it's shortcuts and lightning fast load times.

If you often need to optimize your resource usage, want a client that just works and is easy to use, go for Lens.
This is definitely hits the sweet spot between ease of use, stability and available feature set.

// Kubenav
// Infra app

But our final advice really is: just try them out yourself and see which fits your workflow best.
Most of them share the same basic functionality and it just depends on your use-cases and workflow which one fits best.