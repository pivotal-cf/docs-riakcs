---
title: Riak CS for Pivotal CF
---

This is documentation for the Riak CS service for Pivotal CF.

### Install via Pivotal Operations Manager

To install Riak CS for Pivotal CF, follow the procedure for installing Pivotal Ops Manager tiles:

1. Download the product file from [Pivotal Network](https://network.pivotal.io/).
1. Upload the product file to your Ops Manager installation.
1. Click **Add** next to the uploaded product description in the Available Products view to add this product to your staging area.
1. Click the newly added tile to review any configurable options.
1. Click **Apply Changes** to install the service.

This product requires Pivotal CF version 1.2 or greater.

### Provisioning and Binding via Cloud Foundry

Once you have installed the product, it automatically registers itself with your Elastic Runtime. At this point, the product is available to your application developers, either in the Marketplace in the web based console, or via `cf marketplace`. They can add, provision, and bind the service to their applications like any other CF service:

<pre class="terminal">
$ cf create-service p-riakcs developer mybucket
$ cf bind-service myapp mybucket
$ cf restart myapp
</pre>

### Available Plans

The service offers a single service plan called **developer** suitable for development workloads. Provisioning a service instance creates a private Riak CS bucket. Binding applications to the instance creates unique credentials for each application to access the bucket.

There are no storage quotas on individual buckets. Service capacity is limited by persistent storage allocated to the Riak CS nodes; this defaults to 10GB but can be configured by the Operations Manager user.

### Using the Riak CS service

See [Clients for Riak CS](clients.html) for a list of clients that have been validated to work with the service.

An example application, written in Ruby and using the Fog library, can be [downloaded here](riakcs-example-app.tgz). Once you've downloaded and unpacked the tarball, see the included README for instructions.

<a id="backing-up"></a>
### Backing Up and Restoring

Using [riak-backup](https://github.com/cloudfoundry/cf-riak-cs-release/tree/master/scripts/riak-backup/) ([README](https://github.com/cloudfoundry/cf-riak-cs-release/tree/master/scripts/riak-backup/src/riak_backup)), administrators can back up data from all buckets as a batch process. This tool is distributed as [compiled binaries for linux and OSX](https://github.com/cloudfoundry/cf-riak-cs-release/tree/master/scripts/riak-backup/bin) (not available for Windows at this time), and requires the `cf` CLI and [s3cmd](clients.html#s3cmd) to be independently installed.

Data can be restored one bucket at a time, by admins or end users themselves, using [s3cmd](clients.html#s3cmd). Backup data is stored in a directory tree organized by organization, spaces, and service instances. A metadata file for each instance includes which applications were bound to it.

### Version

Versions 1.2 and 1.2.1 of this product are based on Riak CS version 1.4.4 and Riak version 1.4.7.

### Known Limitations

The product should be considered a public Beta and is not for production use.

#### Upgrading from 1.2.0 to 1.2.1

<strong>Version 1.2.0 has a known issue</strong>: the data files are stored on the ephemeral disk of the VM. If the Riak VMs are re-deployed due to a configuration change for this release in Operations Manager, data will be lost.

Version 1.2.1 fixes this issue by storing data files on a persistent volume. However, we will not support upgrading from 1.2.0 to 1.2.1, as data will be lost. Users of Operations Manager must uninstall (delete) the deployment of version 1.2.0 prior to deploying version 1.2.1. The following steps can be followed to migrate your bucket data, if desired:

1. Import version 1.2.1 into your Operations Manager.
1. Follow instructions in [Backing Up and Restoring](#backing-up) to back up data for all buckets.
1. Delete the deployment of Riak CS for Pivotal CF by clicking the trash can icon in Operations Manager, then Apply Changes. This will delete all service instances and bindings for the Riak CS service in Cloud Foundry and then tear down the Riak cluster.
1. Add version 1.2.1 of Riak CS for Pivotal CF to Operations Manager, then Apply Changes. This will deploy a new Riak CS cluster and restore the Riak CS service to the Cloud Foundry marketplace. For applications that used the service, new instances must be created and bound.
1. Data can be restored to these buckets by users as described in [Backing Up and Restoring](#backing-up).


### Further Reading

* [Official Riak CS Documentation](http://basho.com/riak-cloud-storage/)
