---
title: Release Notes
---

## 1.3.0

- **Syslog forwarding**: Syslogs are now streamed to the same host and port configured in Elastic Runtime settings
- **Trusty stemcells**: Server and broker are now deployed on Ubuntu “Trusty” 14.04 LTS stemcells, providing improved security, performance, and a smaller resource footprint.
- **Precompiled packages**: Most packages have been precompiled for the targeted stemcell. This will lower initial deployment times, at the cost of a larger download.

### Upgrading from 1.2.1 to 1.3.0

Version 1.2.1 had a minimum setting for ephemeral storage on the Riak CS and Stanchion VMs which was too low to accomodate the additional packages installed for 1.3.0. This minimum has been raised, but deploying the upgrade without the following steps will result in a failed deploy.

1.  Import the file `p-riak-cs-1.3.0.0.pivotal` into Pivotal Operations Manager
- Select the new version and click Upgrade
- Click on the product tile, Riak CS for Pivotal CF
- Click Resource Config in the left navigation
- Click Save
- You will be prompted that the ephemeral disk for Riak CS and Stanchion do not meet required minimums. Change those values and click Save.
- Return to the Dashboard and click Apply Changes to deploy the upgrade

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
