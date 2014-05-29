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

### Available Plans

The service offers a single service plan called **developer** suitable for development workloads. Provisioning a service instance creates a private Riak CS bucket. Binding applications to the instance creates unique credentials for each application to access the bucket.

There are no storage quotas on individual buckets. Service capacity is limited by persistent storage allocated to the Riak CS nodes; this defaults to 10GB but can be configured by the Operations Manager user.  

### Version

Version 1.2 of this product is based on Riak CS version 1.4.4 and Riak version 1.4.7.

### Known Limitations

The product should be considered a public Beta and is not for production use.

### Further Reading

* [Official Riak CS Documentation](http://basho.com/riak-cloud-storage/)

