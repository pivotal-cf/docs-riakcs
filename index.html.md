---
title: Riak CS for Pivotal CF
---

This is documentation for the Riak CS service for Pivotal CF.

### Install via Pivotal Operations Manager

To install Riak CS for Pivotal CF, follow the procedure for installing Pivotal Ops Manager tiles:

1. Download the product file from [Pivotal Network](https://network.gopivotal.com/).
1. Upload the product file to your Ops Manager installation.
1. Click **Add** next to the uploaded product description in the Available Products view
   to add this product to your staging area.
1. Click the newly added tile to review any configurable options.
1. Click **Apply Changes** to install the service.

This product requires Pivotal CF version 1.2 or greater.

### Provisioning and Binding via Cloud Foundry

Once you have installed the product, it automatically registers itself with your Elastic Runtime. At this point, the product is available to your application developers, either in the Marketplace in the web based console, or via `cf marketplace`. They can add, provision, and bind the service to their applications like any other CF service:

```
$ cf create-service p-riakcs developer mybucket
$ cf bind-service myapp mybucket
$ cf restart myapp
```

### Example Application

To help your application developers get started with Riak CS for Pivotal CF, we have provided an example application, which can be [downloaded here][example-app]. Once you've downloaded and unpacked the tarball, see the included README for instructions on using the example application.

[example-app]:riakcs-example-app.tgz

### Using the Riak CS service

Documentation about connecting to your Riak CS service instances can be found on the [Getting Started](getting-started.html) page.

<a id='backing-up'></a>
### Backing Up and Restoring

1. An administrator can use the [riak-backup tool](https://github.com/cloudfoundry/cf-riak-cs-release/tree/master/scripts/riak-backup/src/riak_backup) to back up all the data in the Riak cluster.
1. The administrator can provide the data to users when they request it, after verifying that they are a member of the space that owned the service instance containing the data. (The backup data is stored in a way that allows the administrator to identify the space that owned each service instance.)
1. Users can create new service instances and import data from their backup. Usage of the s3cmd tool for uploading data to a bucket is described on the [Getting Started](getting-started.html#s3cmd) page.

### Available Plans

The service offers a single service plan called **developer** suitable for development workloads. Provisioning a service instance creates a private Riak CS bucket. Binding applications to the instance creates unique credentials for each application to access the bucket.

There are no storage quotas on individual buckets. Service capacity is limited by persistent storage allocated to the Riak CS nodes; this defaults to 10GB but can be configured by the Operations Manager user.  

### Version

Versions 1.2 and 1.2.1 of this product are based on Riak CS version 1.4.4 and Riak version 1.4.7.

### Known Limitations

The product should be considered a public Beta and is not for production use.

**Version 1.2 has a known issue**: the data files are stored on the ephemeral disk of the VM. If the Riak VMs are re-deployed due to a configuration change for this release in Operations Manager, data will be lost.

Version 1.2.1 fixes this issue. The following migration steps must be taken to ensure that data is not lost when upgrading from 1.2 to 1.2.1:

1. The administrator should follow the steps in [Backing Up and Restoring](#backing-up) to back up the data.
1. The Riak CS 1.2 Operations Manager tile must be removed. It is not possible to upgrade to 1.2.1 as this would result in data loss.
1. The Riak CS 1.2.1 Operations Manager tile should be added.
1. Data can be restored by users as described in [Backing Up and Restoring](#backing-up).


### Further Reading

* [Official Riak CS Documentation](http://basho.com/riak-cloud-storage/)
