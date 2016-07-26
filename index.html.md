---
title: Riak CS for Pivotal Cloud Foundry
owner: Riak
---

This is documentation for the [Riak CS service](https://network.pivotal.io/products/p-riakcs) for [Pivotal Cloud Foundry](https://network.pivotal.io/products/pivotal-cf) (PCF).

## <a id="product-snapshot"></a>Product Snapshot ##

Current Riak CS for Pivotal Cloud Foundry Details
<div style="line-height: 1; padding-left: 3em">

- **Version**: 1.5.13
- **Release Date**: 2016-06-15
- **Software component version**: Riak CS 1.5.4, Riak 1.4.12
- **Compatible Ops Manager Version(s)**: 1.4.x, 1.5.x, 1.6.x, 1.7.x
- **Compatible Elastic Runtime Version(s)**: 1.4.5, 1.5.x, 1.6.x, 1.7.x
- **vSphere support?** Yes
- **AWS support?** Yes
- **OpenStack support?** Yes
</div>

## <a id="upgrading"></a>Upgrading to the Latest Version ##

Consider the following compatibility information before upgrading Riak CS for Pivotal Cloud Foundry.

<p class="note"><strong>Note</strong>: Before you upgrade to Ops Manager 1.4.x or 1.5.x, you must first upgrade Riak CS for PCF to any version of 1.3. This allows Riak CS for PCF upgrades after you install OpsManager 1.4.x.</p>

For more information, refer to the full [Product Version Matrix](http://docs.pivotal.io/compatibility-matrix.pdf).

<table border="1" class="nice">
<tr>
	<th rowspan="2">Ops Manager Version</th>
	<th colspan="2">Supported Upgrades from Imported Riak CS Installation</th>
</tr>

<tr>
	<th>From</th>
	<th>To</th>
</tr>

<tr>
	<th rowspan="2">1.3.x</th>
	<td>1.2.1</td>
	<td>1.3.2</td>
</tr>

<tr>
	<td>1.3.2</td>
	<td>1.3.3</td>
</tr>

<tr>
	<th>1.4.x and 1.5.x</th>
	<td>1.3.2, 1.3.3</td>
	<td>1.4.0</td>
</tr>

<tr>
	<th rowspan="2">1.4.x, 1.5.x, and 1.6.x</th>
	<td>1.4.0</td>
	<td>1.5.1 - 1.5.13</td>
</tr>

<tr>
	<td>1.5.1 - 1.5.12</td>
	<td>Next 1.5.x release - 1.5.13</td>
</tr>

</table>

## <a id="release-notes"></a>Release Notes ##

Consult the [Release Notes](release-notes.html) for important tips and information about changes for this product.

## <a id="known-issues"></a>Known Issues ##

- Upgrading from v1.3.2 to v1.3.3 requires a resource configuration change in Operations Manager. See [Release Notes](release-notes.html) for details.
- **Note**: This product should be considered for development use only and is not intended for production use.

## <a id="installation"></a>Installation ##

To install Riak CS for PCF, follow the procedure for installing Pivotal Operations Manager tiles:

1. Download the product file from [Pivotal Network](https://network.pivotal.io/).
1. Upload the product file to your Ops Manager installation.
1. Click **Add** next to the uploaded product description in the Available Products view to add this product to your staging area.
1. Click the newly added tile to review any configurable options.
1. Click **Apply Changes** to install the service.

This product requires PCF version 1.2 or greater.

## <a id="settings"></a>Settings ##

### <a id="cluster-components"></a>Understanding Cluster Components ###

See [Managing Your Cluster](managing_your_cluster.html) for a description of the different components deployed by this product, and how they can be scaled up and down depending on your requirements.

### <a id="service-plan"></a>Service Plan ###

The service offers a single service plan called **developer** suitable for development workloads. Provisioning a service instance creates a private Riak CS bucket. Binding applications to the instance creates unique credentials for each application to access the bucket.

There are no storage quotas on individual buckets. Service capacity is limited by persistent storage allocated to the Riak CS nodes; this defaults to 10GB but can be configured by the Operations Manager user.

## <a id="provision-and-bind"></a>Provisioning and Binding via Cloud Foundry ##

Once you have installed the product, it automatically registers itself with your Elastic Runtime. At this point, the product is available to your application developers, either in the Marketplace in the web based console, or via `cf marketplace`. They can add, provision, and bind the service to their applications like any other CF service:

<pre class="terminal">
$ cf create-service p-riakcs developer mybucket
$ cf bind-service myapp mybucket
$ cf restart myapp
</pre>

## <a id="using-the-service"></a>Using the Riak CS service ##

### <a id="clients"></a>S3 Clients ###

See [Clients for Riak CS](https://github.com/cloudfoundry/cf-riak-cs-release/blob/master/docs/clients.md) for a list of clients that have been validated to work with the service.

### <a id="example-app"></a>Example Application ###

An example application, written in Ruby and using the Fog library, can be [downloaded here](riakcs-example-app.tgz). Once you've downloaded and unpacked the tarball, see the included README for instructions.

<a id="backing-up"></a>

## Backing Up and Restoring

Using [riak-backup](https://github.com/cloudfoundry/cf-riak-cs-release/tree/master/scripts/riak-backup/src/riak_backup), administrators can back up data from all buckets as a batch process. This tool is distributed as [compiled binaries for linux and OSX](https://github.com/cloudfoundry/cf-riak-cs-release/tree/master/scripts/riak-backup/bin) (not available for Windows at this time), and requires the `cf` CLI and [s3cmd](https://github.com/cloudfoundry/cf-riak-cs-release/blob/master/docs/clients.md#s3cmd) to be independently installed.

Data can be restored one bucket at a time, by admins or end users themselves, using [s3cmd](https://github.com/cloudfoundry/cf-riak-cs-release/blob/master/docs/clients.md#s3cmd). Backup data is stored in a directory tree organized by organization, spaces, and service instances. A metadata file for each instance includes which applications were bound to it.


## Further Reading

* [Official Riak CS Documentation](http://basho.com/riak-cloud-storage/)
