---
layout: default
title: Install and run
anchor: install

sections:
  -
    title: Quick start
    description: Quick start
    module: install/quickstart
  -
    title: Prerequisites
    description: Prerequisites
    module: install/prerequisites
    items:
      -
        title: Install CLI
        description: Install CLI
        module: install/cli
      -
        title: Cluster discovery
        description: Cluster discovery
        module: install/prerequisites/clusterdiscovery
      -
        title: Device presentation
        description: Device presentation
        module: install/prerequisites/devicepresentation
      -
        title: Ports and firewall settings
        description: Ports and firewall settings
        module: install/prerequisites/firewalls
      - 
        title: Mount Propagation
        description: Mount propagation for docker, k8s and OpenShift
        module: install/prerequisites/mountpropagation
  -
    title: Cloud
    description: Installing in Cloud Providers
    module: install/cloud
    items:
      -
        title: Overview
        description: Overview of Cloud Providers
        module: install/cloud/index
      -
        title: Azure
        description: Install in Azure
        module: install/cloud/azure
      -
        title: AWS
        description: Install in AWS
        module: install/cloud/aws
      -
        title: Google
        description: Install in Google Cloud
        module: install/cloud/googlecloud
  -
    title: Kubernetes
    description: Installing on Kubernetes
    module: install/kubernetes
    items:
      -
        title: Preprovisioned volumes
        description: Preprovisioned volumes
        module: install/kubernetes/preprovisioned
      -
        title: Dynamic provisioning
        description: Dynamic provisioning
        module: install/kubernetes/dynamic-provisioning
  -
    title: Kubernetes (CSI)
    description: Installing on Kubernetes
    module: install/kubernetes-csi
    items:
      -
        title: Dynamic provisioning (CSI)
        description: Dynamic provisioning (CSI)
        module: install/kubernetes-csi/dynamic-provisioning
  -
    title: OpenShift
    description: Installing on OpenShift
    module: install/openshift
    items:
      -
        title: Preprovisioned volumes
        description: Preprovisioned volumes
        module: install/openshift/preprovisioned
      -
        title: Dynamic provisioning
        description: Dynamic provisioning
        module: install/openshift/dynamic-provisioning
  -
    title: Docker
    description: Installing on Docker
    module: install/docker
    items:
      -
        title: Docker Swarm
        description: Installing on Docker Swarm
        module: install/docker/swarm
  -
    title: Deployment types
    description: Deployment types
    module: install/deployment
  -
    title: Cluster health
    description: Checking the status of a StorageOS cluster
    module: install/health
  -
    title: Troubleshooting
    description: Troubleshooting a Docker installation
    module: install/troubleshooting
---
