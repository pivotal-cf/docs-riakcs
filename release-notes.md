---
title: Release Notes
---

## 1.2.1

- **Persistence**: Data is now stored on a persistent volume.

### Upgrading from 1.2.0 to 1.2.1 is not supported

Version 1.2.0 stored data files on an ephemeral volume, which resulted in data loss when cluster VMs were restarted.

Version 1.2.1 fixes this issue by storing data files on a persistent volume. However, as data could be lost in the process, we do not support upgrading from 1.2.0 to 1.2.1. Users of Operations Manager must delete the deployment of version 1.2.0 prior to deploying version 1.2.1. The following steps can be followed to migrate your bucket data, if desired:

1. Import version 1.2.1 into your Operations Manager.
1. Follow instructions in [Backing Up and Restoring](#backing-up) to back up data for all buckets.
1. Delete the deployment of Riak CS for Pivotal CF by clicking the trash can icon in Operations Manager, then Apply Changes. This will delete all service instances and bindings for the Riak CS service in Cloud Foundry and then tear down the Riak cluster.
1. Add version 1.2.1 of Riak CS for Pivotal CF to Operations Manager, then Apply Changes. This will deploy a new Riak CS cluster and restore the Riak CS service to the Cloud Foundry marketplace. For applications that used the service, new instances must be created and bound.
1. Data can be restored to these buckets by users as described in [Backing Up and Restoring](#backing-up).

## 1.2.0

- **Initial Release**
