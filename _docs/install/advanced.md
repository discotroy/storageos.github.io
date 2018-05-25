---
layout: guide
title: StorageOS Docs - Docker (advanced)
anchor: install
module: install/advanced
---

# Docker Advanced Installation

If you require greater control, StorageOS can be deployed in other ways.

## Overview

By default, StorageOS provides all of its services from a single container.  It
is also possible to split services into multiple container instances.

This allows some critical components which are updated infrequently to remain
running while other components are being upgraded.  Our aim is to reduce and
eventually eliminate any loss of service during an upgrade.

StorageOS currently supports running the Control Plane and Data Plane in
separate containers.  We expect the Data Plane to stabilise over time and to
require fewer updates than the Control Plane.

### Requirements

The split-container installation method works on Docker 1.10+.  It is not
compatible with Docker managed plugins, which are limited to a single container.

Docker Compose is recommended.

## Manual Installation

StorageOS shares volumes via the `/var/lib/storageos` directory.  This must be
present on each node where StorageOS runs.  Prior to installation, create it:

```bash
sudo mkdir /var/lib/storageos
```

Run the Control Plane container:

```bash
docker run -d --name controlplane \
    -e HOSTNAME \
    --net=host \
    --pid=host \
    --privileged \
    --cap-add SYS_ADMIN \
    -v /var/lib/storageos:/var/lib/storageos:rshared \
    -v /run/docker/plugins:/run/docker/plugins \
    storageos/node controlplane
```

Run the Data Plane container:

```bash
docker run -d --name dataplane \
    -e HOSTNAME \
    --net=host \
    --pid=host \
    --privileged \
    --cap-add SYS_ADMIN \
    --device /dev/fuse \
    -v /sys:/sys \
    -v /var/lib/storageos:/var/lib/storageos:rshared \
    storageos/node dataplane
```

## Docker Compose

Docker Compose can be used to start separate Control Plane and Data Plane
containers.

Write the compose file (below) to a file, e.g. `/etc/storageos/docker-compose.yml`.

```yaml
version: '2'
services:
  control:
    image: storageos/node:latest
    command: "controlplane"
    restart: always
    environment:
      - HOSTNAME
    privileged: true
    cap_add:
      - "SYS_ADMIN"
    ports:
      - "5705:5705"
      - "4222:4222"
      - "8222:8222"
      - "13700:13700/tcp"
      - "13700:13700/udp"
    volumes:
      - "/var/lib/storageos:/var/lib/storageos:rshared"
      - "/run/docker/plugins:/run/docker/plugins"
      - "/sys:/sys"
  data:
    image: storageos/node:latest
    command: "dataplane"
    restart: always
    network_mode: host
    privileged: true
    cap_add:
      - "SYS_ADMIN"
    pid: host
    environment:
      - HOSTNAME
    devices:
      - /dev/fuse
    volumes:
      - "/var/lib/storageos:/var/lib/storageos:rshared"
      - "/sys:/sys"
    ports:
      - "8999:8999"
```

Start the containers with:

```bash
docker compose -f /etc/storageos/docker-compose.yml up
```

## Environment variables

For most environments, the default settings should work. If Consul is not
running locally on the node, you will need to set `KV_ADDR`.

* `HOSTNAME`: Hostname of the Docker node, only if you wish to override it.
* `ADVERTISE_IP`: IP address of the Docker node, for incoming connections.  Defaults to first non-loopback address.
* `USERNAME`: Username to authenticate to the API with.  Defaults to `storageos`.
* `PASSWORD`: Password to authenticate to the API with.  Defaults to `storageos`.
* `KV_ADDR`: IP address/port of the Key/Vaue store.  Defaults to `127.0.0.1:8500`
* `KV_BACKEND`: Type of KV store to use.  Defaults to `consul`. `boltdb` can be used for single node testing.
* `API_PORT`: Port for the API to listen on.  Defaults to `5705` ([IANA Registered](https://www.iana.org/assignments/service-names-port-numbers/service-names-port-numbers.xhtml?search=5705)).
* `NATS_PORT`: Port for NATS messaging to listen on.  Defaults to `4222`.
* `NATS_CLUSTER_PORT`: Port for the NATS cluster service to listen on.  Defaults to `8222`.
* `SERF_PORT`: Port for the Serf protocol to listen on.  Defaults to `13700`.
* `DFS_PORT`: Port for DirectFS to listen on.  Defaults to `17100`.
* `LOG_LEVEL`: One of `debug`, `info`, `warning` or `error`.  Defaults to `info`.
* `LOG_FORMAT`: Logging output format, one of `text` or `json`.  Defaults to `json`.
