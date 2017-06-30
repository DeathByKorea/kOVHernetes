# kOVHernetes

![kOVHernetes][logo]

kOVHernetes (*`kovh`*) is a command-line utility for managing [Kubernetes][k8s] clusters on the [OVH Cloud][ovhcloud]
platform.

It deploys cloud instances running the [CoreOS Container Linux][cont-linux] operating system with Kubernetes components
pre-configured using [Ignition][ignition], including a [flannel][flannel] overlay network for containers.

#### Contents

1. [Prerequisites](#prerequisites)
2. [Installation](#installation)
3. [Documentation](#documentation)
4. [Disclaimer](#disclaimer)
5. [Roadmap](#roadmap)

## Prerequisites

### :warning: CoreOS Container Linux image

The *Container Linux* image available in the OVH image service is outdated and misses features required by the `kovh`
client (Ignition OpenStack compatibility, *rkt* wrapper scripts).

Until this problem is tackled by OVH, please follow the instructions at [Running CoreOS Container Linux on
OpenStack][coreos-ostack] to download the latest Stable release, then upload it to your OVH account using the [openstack
client][ostack-cli]:

```sh
❯ openstack image create \
    --disk-format 'qcow2' \
    --file 'coreos_production_openstack_image.img' \
    --property os-distro='container' \
    --property os-version='1409.5.0' \
    --property image_original_user='core' \
    'Container Linux Stable'
```

[coreos-ostack]: https://coreos.com/os/docs/latest/booting-on-openstack.html
[ostack-cli]: https://pypi.python.org/pypi/python-openstackclient

### Runtime

[Python][python] interpreter (version 3.2 or above) with the [`setuptools`][py-setuptools] package.

Additionally on **Linux**:

* [`cryptography`][py-cryptography] package
  * provided by the `python3-cryptography` OS package
  * installed from source, see [Building cryptography on Linux][cryp-req]

### Cloud provider

* At least one Cloud project must be created before resources can be provisioned by kOVHernetes.
* The Cloud project must be associated with a vRack in order to create Private Networks.
* At least one SSH public key must have been added to the project.

![OVH dashboard][dash]

## Installation

Run the following command inside the repository to install the project dependencies and the `kovh` executable:

```sh
❯ python3 setup.py install
```

## Documentation

* [Configuration][config]
* [Tutorial][tutorial]
* [Cluster architecture][arch]
* [Command reference][cmd-ref]

[config]: docs/configuration.md
[tutorial]: docs/tutorial.md
[arch]: docs/architecture.md
[cmd-ref]: docs/commands-reference.md

## Disclaimer

kOVHernetes is a toy project written for testing purposes. It is **not** supported by [OVH][ovh] in any manner.

Although I strive to enforce a [reasonably high level of security][security] by default, no security audit has been
performed on this software or the elements of infrastructure it helps to generate.

**Use at your own risk**.

## Roadmap

* [ ] Secure apiserver -> kubelet communications
* [ ] `list` command to display clusters
* [ ] Master HA
* [ ] CNI networking


[logo]: docs/images/logo.png
[k8s]: https://kubernetes.io/
[ovhcloud]: https://www.ovh.com/cloud/
[cont-linux]: https://coreos.com/os/
[ignition]: https://coreos.com/ignition/
[flannel]: https://coreos.com/flannel/
[python]: https://www.python.org/downloads/
[py-setuptools]: https://pypi.python.org/pypi/setuptools
[py-cryptography]: https://pypi.python.org/pypi/cryptography
[cryp-req]: https://cryptography.io/en/latest/installation/#building-cryptography-on-linux
[dash]: docs/images/project_dashboard.png
[ovh]: https://www.ovh.com/
[security]: docs/architecture.md#security
