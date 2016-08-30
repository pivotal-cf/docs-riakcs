---
title: Release Notes
owner: Riak
---

## <a id="1-5-10"></a>1.5.11

Release date: 16 March 2016

- Updated stemcell to 3146.10. This is a security upgrade that resolves the following:
  - [USN-2929-1](http://www.ubuntu.com/usn/usn-2929-1/)


Additional information can be found at https://pivotal.io/security.

## <a id="1-5-10"></a>1.5.10

Release date: 24 February 2016

- Updated stemcell to 3146.9. This is a security upgrade that resolves the following:
  - [USN-2910-1](http://www.ubuntu.com/usn/usn-2910-1/)

  This release also includes the security updates included in stemcell 3146.8:
  - [USN-2900-1](http://www.ubuntu.com/usn/usn-2900-1/), a critical GNU C lib (glibc) CVE
  - [USN-2897-1](http://www.ubuntu.com/usn/usn-2897-1/)
  - [USN-2896-1](http://www.ubuntu.com/usn/usn-2896-1/)

Additional information can be found at https://pivotal.io/security.

## <a id="1-5-9"></a> 1.5.9 ##

Release date: 2 February 2016

- Updated stemcell to 3146.6. This is a security upgrade that resolves the following:
  - [USN-2882-1](http://www.ubuntu.com/usn/usn-2882-1/)
  - [USN-2879-1](http://www.ubuntu.com/usn/usn-2879-1/)
  - [USN-2875-1](http://www.ubuntu.com/usn/usn-2875-1/)
  - [USN-2871-1](http://www.ubuntu.com/usn/usn-2871-1/)
  - [USN-2868-1](http://www.ubuntu.com/usn/usn-2868-1/)
  - [USN-2865-1](http://www.ubuntu.com/usn/usn-2865-1/)
  - [USN-2861-1](http://www.ubuntu.com/usn/usn-2861-1/)

Additional information can be found at https://pivotal.io/security.

## <a id="1-5-8"></a>1.5.8 ##

Release date: 18 January 2016

- Updated stemcell to 3146.3. This is a security upgrade that resolves [CVE-2016-0715](https://pivotal.io/security/cve-2016-0715). Additional information can be found at https://pivotal.io/security.

## <a id="1-5-7"></a>1.5.7 ##

Release date: 04 December 2015

- Updated stemcell to 3146. This is a security upgrade that resolves the following Ubuntu Security Notices:
  - [[USN-2821-1](http://www.ubuntu.com/usn/usn-2821-1/)] GnuTLS vulnerability

## <a id="1-5-6"></a>1.5.6 ##

Release date: 03 December 2015

- Updated stemcell to 3144. This is a regular security upgrade that resolves the following Ubuntu Security Notices:
  - [[USN-2815-1](http://www.ubuntu.com/usn/usn-2815-1/)] libpng vulnerabilities
  - [[USN-2812-1](http://www.ubuntu.com/usn/usn-2812-1/)] libxml2 vulnerabilities
  - [[USN-2810-1](http://www.ubuntu.com/usn/usn-2810-1/)] Kerberos vulnerabilities

## <a id="1-5-5"></a>1.5.5 ##

Release date: 12 November 2015

Updated stemcell to 3130. This is a regular security upgrade that resolves the following issues:

- [[USN-2806-1](http://www.ubuntu.com/usn/usn-2806-1/)] Linux kernel (Vivid HWE) vulnerability
- [[USN-2798-1](http://www.ubuntu.com/usn/usn-2798-1/)] Linux kernel (Vivid HWE) vulnerabilities

## <a id="1-5-4"></a>1.5.4 ##

Release date: 3 November 2015

Updated stemcell to 3112. This is a regular security upgrade that resolves the following issues:

- [[USN-2778-1](http://people.canonical.com/~ubuntu-security/cve/2015/CVE-2015-5156.html)] Linux kernel (Vivid HWE) vulnerabilities

## <a id="1-5-3"></a>1.5.3 ##

- Updated stemcell to 3094 and ruby to v2.2.3 to resolve CVE-2015-3900, a man-in-the-middle rubygems vulnerability.
- A minor fix for HTTPS-only mode, now acceptance tests will successfully pass as well.

## <a id="1-5-2"></a>1.5.2 ##

- Updated stemcell to 3062 to resolve CVE-2015-3290

## <a id="1-5-1"></a>1.5.1 ##

A minor release.

- Updates to support the experimental HTTPS-only feature in Elastic Runtime 1.5.
- Documentation now includes a new section, [Managing Your Cluster](managing_your_cluster.html).

* **Note:** BOSH Stemcell 3026 is required; this stemcell is provided by Ops Manager 1.5.1.


## <a id="1-4-0"></a>1.4.0 ##

- **AWS support:** The Riak CS service can now be deployed on Amazon Web Services from the Operations Manager Web UI.
  - Deployment is limited to a single Availability Zone. Look for multi-AZ in future releases.
  - Single availability zone is a limitation on AWS only. Operations Manager on vSphere continues to support deployment to multiple availability zones.
  - The default instance type for the cluster nodes on AWS is m3.large.
- **IaaS agnostic**
  - The same product can be deployed to both AWS and vSphere.
  - Precompiled packages are no longer included.
  - p-riak-cs 1.4.0 requires Operations Manager 1.4.0
- **Cluster node changes:** Ephemeral disk default of 1GB is now 2GB.
- **Bug Fix:** Addresses an issue when creating service instances which occasionally gives the error: `403 Forbidden ("InvalidAccessKeyId The AWS Access Key Id you provided does not exist in our records")`
- **Upgrade support:** p-riak-cs 1.4.0 can be automatically upgraded from either p-riak-cs 1.3.2 or 1.3.3

**Known issues:**

* On AWS, this version supports deployments in the US-East region. Multi-region support is coming in a future release.
* The experimental HTTPS-only feature in Elastic Runtime 1.5 may cause issues with this version of the product. Full support for HTTPS-only traffic is coming in a future release.
* Note: BOSH Stemcell 2865.1 is required for installation on Ops Manager 1.5.x and above.

## <a id="1-3-3"></a>1.3.3 ##

- **Improved garbage collection**: persistent disk is reliably reclaimed asynchronously after objects are deleted
- **Requires Operations Manager version 1.3.4 or greater**

### Upgrading from 1.3.2 to 1.3.3

Version 1.3.2 had a default setting for Ephemeral Disk for the RiakCS Broker job which was too low to support an upgrade to 1.3.3. Operations Manager (version 1.3.4 required) will not allow you to upgrade to Riak CS 1.3.3 without updating the resource configuration.

#### Disregard this issue if:

- Ephemeral Disk for RiakCS Broker was previously configured with at least 2048 MB
- You are deploying Riak CS 1.3.3 without upgrading; this issue applies to upgrades only

#### To upgrade from 1.3.2 to 1.3.3:

1.  Import the file `p-riak-cs-1.3.3.0.pivotal` into Pivotal Operations Manager version 1.3.4 or greater
- Select the version 1.3.3 and click Upgrade
- If a configuration change is required, the product tile for Riak CS will be highlighted in orange. Operations Manager will prevent deployment until these changes are addressed. Click on the product tile Riak CS for [Pivotal Cloud Foundry&reg;](https://network.pivotal.io/products/pivotal-cf) (PCF).
- If a configuration change is required, the Resource Config tab in the left navigation will be highlighted orange. Click on the Resource Config tab.
- Configuration issues will be highlighted in red. Mousing over the highlighted field will show that Ephemeral Disk for RiakCS must be at least 2048 MB. Update this values and click Save.
- Return to the Installation Dashboard and click Apply Changes to deploy the upgrade

## <a id="1-3-2"></a>1.3.2 ##

- **Syslog forwarding**: Syslogs are now streamed to the same host and port configured in Elastic Runtime settings
- **Precompiled packages**: Most packages have been precompiled for the targeted stemcell. This will lower initial deployment times, at the cost of a larger download.
- **Trusty stemcell**: Server and broker are now deployed on the Ubuntu “Trusty” 14.04 LTS stemcell, providing improved security, performance, and a smaller resource footprint. This stemcell addresses bash-shellshock vulnerabilities discussed [here](http://www.pivotal.io/security/CVE-2014-6271) and [here](http://www.pivotal.io/security/CVE-2014-7186).

### Upgrading from 1.2.1 to 1.3.2

Version 1.2.1 had a default setting for Ephemeral Disk for the Riak CS and Stanchion jobs which was too low to support an upgrade to 1.3.0. The upgrade will fail with the error `No space left on device` unless Ephemeral Disk is increased to at least 4096 MB prior to upgrading.

#### Disregard this issue if:

- Ephemeral Disk for Riak CS Node was previously configured with at least 4096 MB
- You are deploying Riak CS 1.3.2 without upgrading; this issue applies to upgrades only

#### To upgrade from 1.2.1 to 1.3.2:

1.  Import the file `p-riak-cs-1.3.2.0.pivotal` into Pivotal Operations Manager
- Select the version 1.3.2 and click Upgrade
- Click on the product tile Riak CS for PCF
- Click Resource Config in the left navigation
- Click Save
- You will be notified of configuration errors. Mousing over the highlighted fields will show that Ephemeral Disk for Riak CS Node and Stanchion jobs must be at least 4096 MB. Update these values and click Save.
- Return to the Installation Dashboard and click Apply Changes to deploy the upgrade

## <a id="1-2-1"></a>1.2.1 ##

- **Persistence**: Data is now stored on a persistent volume.

### Upgrading from 1.2.0 to 1.2.1 is not supported

Version 1.2.0 stored data files on an ephemeral volume, which resulted in data loss when cluster VMs were restarted.

Version 1.2.1 fixes this issue by storing data files on a persistent volume. However, as data could be lost in the process, we do not support upgrading from 1.2.0 to 1.2.1. Users of Operations Manager must delete the deployment of version 1.2.0 prior to deploying version 1.2.1. The following steps can be followed to migrate your bucket data, if desired:

1. Import version 1.2.1 into your Operations Manager.
1. Follow instructions in [Backing Up and Restoring](index.html#backing-up) to back up data for all buckets.
1. Delete the deployment of Riak CS for PCF by clicking the trash can icon in Operations Manager, then Apply Changes. This will delete all service instances and bindings for the Riak CS service in Cloud Foundry and then tear down the Riak cluster.
1. Add version 1.2.1 of Riak CS for PCF to Operations Manager, then Apply Changes. This will deploy a new Riak CS cluster and restore the Riak CS service to the Cloud Foundry marketplace. For applications that used the service, new instances must be created and bound.
1. Data can be restored to these buckets by users as described in [Backing Up and Restoring](index.html#backing-up).

## <a id="1-2-0"></a>1.2.0 ##

- **Initial Release**
