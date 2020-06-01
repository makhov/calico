---
title: Workload Endpoint Resource (workloadEndpoint)
canonical_url: '/reference/calicoctl/resources/workloadendpoint'
---

A Workload Endpoint resource (workloadEndpoint) represents an interface 
connecting a Calico networked container or VM to its host.

Each endpoint may specify a set of labels and list of profiles that Calico will use 
to apply policy to the interface.

For `calicoctl` commands that specify a resource type on the CLI, the following
aliases are supported (all case insensitive): `workloadendpoint`, `workloadendpoints`, `wep`, `weps`.

> **Note**
>
> While `calicoctl` allows the user to fully manage Workload Endpoint resources,
the lifecylce of these resources is generally handled by an orchestrator specific 
plugin such as the Calico CNI plugin, the Calico Docker network plugin, 
or the Calico OpenStack Neutron Driver.  In general, we recommend that you only
use `calicoctl` to view this resource type.

### Sample YAML

```yaml
apiVersion: v1
kind: workloadEndpoint
metadata:
  name: eth0 
  workload: default.frontend-5gs43
  orchestrator: k8s
  node: rack1-host1
  labels:
    app: frontend
    calico/k8s_ns: default
spec:
  interfaceName: cali0ef24ba
  mac: ca:fe:1d:52:bb:e9 
  ipNetworks:
  - 192.168.0.0/32
  profiles:
  - profile1
```

### Definitions

#### Metadata

| Field       | Description                 | Accepted Values   | Schema | Default    |
|-------------|-----------------------------|-------------------|--------|------------|
| name           | The name of this endpoint resource.                     |  | string |
| workload       | The name of the workload to which this endpoint belongs. |  | string |
| orchestrator   | The orchestrator that created this endpoint.  |  | string |
| node           | The node where this endpoint resides.   |  | string |
| labels         | A set of labels to apply to this endpoint.              |  | map |

#### Spec

| Field       | Description                 | Accepted Values   | Schema | Default    |
|-------------|-----------------------------|-------------------|--------|------------|
| ipNetworks    | The CIDRs assigned to the interface. |  | List of strings |
| profiles      | List of profiles assigned to this endpoint.             | | List of strings |
| interfaceName | The name of the host-side interface attached to the workload. | | string |
| mac           | The source MAC address of traffic generated by the workload. | | IEEE 802 MAC-48, EUI-48, or EUI-64 |


### Supported operations

| Datastore type        | Create/Delete | Update | Get/List | Notes
|-----------------------|---------------|--------|----------|------
| etcdv2                | Yes           | Yes    | Yes      |
| Kubernetes API server | No            | Yes    | Yes      | WorkloadEndpoints are directly tied to a Kuberenetes Pod.