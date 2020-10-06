+++
title = "Prerequisites"
weight = "2"
+++
 
There are some prerequisites to installing cf-for-k8s. You will need a handful of CLI tools to help with various parts of the installation. Let’s quickly get into each of them. 

## Required tools

We provide installation links for each tool below. If you are comfortable with docker, you can use the docker image maintained by the cf-for-k8s release engineering which has all of the tools listed below. If you chose to use the docker image, you can skip each of the `Installation` links below.

If you are comfortable with docker, you can use the `relintdockerhubpushbot/cf-for-k8s-ci` docker image as it contains all of the below tools. _Use of this image is beyond the scope of this tutorial._

### cf

The Cloud Foundry CLI is the tool that manages your local machine and helps connect to the remote Cloud Foundry endpoint. It is the official command-line client for Cloud Foundry.

Installation: https://github.com/cloudfoundry/cli/wiki/V7-CLI-Installation-Guide.

### kubectl

Technically speaking, kubectl is the client for the remote Kubernetes API. It acts as the “control center” for your Kubernetes installation. You can conduct all control operations on your remote cluster by using kubectl.

Installation: https://kubernetes.io/docs/tasks/tools/install-kubectl/

### gcloud

This tutorial assumes that you’re going to be using Google Cloud as your IaaS provider. When using Google Cloud, the gcloud command-line interface helps you create and manage all your resources on Google Cloud. 

Installation: https://cloud.google.com/sdk/docs/install

### bosh

One step in the install process requires you to generate a YAML file. In case you would like to use a script to create this file with some defaults, you will need the BOSH CLI.

The BOSH CLI is the command-line client for the core Cloud Foundry operations and releases management tool. In this installation process, it has only a small role to play.

Installation: https://bosh.io/docs/cli-v2-install/

## ytt and kapp

Finally, you will need 2 important tools from the k14s toolchain. These are `ytt` and `kapp`.

- `ytt` is a YAML templating tool that is used to create the full YAML file needed for the cf-for-k8s installation. It is used to create a configuration file that parameterizes all the values and variables needed for installing cf-for-k8s.

  Installation (select your binary): https://github.com/k14s/ytt/releases

-  `kapp` is a deployment tool that performs the actual installation of cf-for-k8s on the Kubernetes cluster. There are many advantages that the usage of kapp brings such as exempting custom resources, automatically ascertain create/update/delete operations, and providing deployment progress.

  Installation (select your binary): https://github.com/k14s/kapp/releases
