---
layout: guide
title: StorageOS Docs - Prerequisites
anchor: install
module: install/prerequisites/index
---

# Prerequisites

The following conditions must be met before installing StorageOS:

1. Minimum one core with 2GB RAM. For replication and HA, a minimum of three nodes is required for quorum.
1. Linux with a 64-bit architecture
 * Check [OS distribution compatibility](/docs/reference/os_support).
 * For Red Hat Enterprise Linux 7, [read the release notes]({% link _docs/reference/release_notes.md %}).
1. Docker 1.10 or later, with mount propagation enabled.
1. The necessary ports should be open. See the [ports and firewall settings]({% link _docs/install/prerequisites/firewalls.md %})
1. A mechanism for [cluster
discovery]({% link _docs/install/prerequisites/clusterdiscovery.md %}), to allow
StorageOS nodes to contact each other.
1. A mechanism for [device presentation]({% link _docs/install/prerequisites/devicepresentation.md %})

This allows you to install StorageOS using a container orchestrator such as [Kubernetes]({% link _docs/install/kubernetes/index.md %}) or with [Docker only]({% link _docs/install/docker/index.md %}).
