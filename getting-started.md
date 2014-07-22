#Getting Started with Riak CS

###Prerequisites:

You have created a service instance, bound it to an application, and have binding credentials from the VCAP_SERVICES environment variable.

```
"VCAP_SERVICES":
{
  "p-riakcs": [
    {
      "name": "mybucket",
      "label": "p-riakcs",
      "tags": [
        "riak-cs",
        "s3"
      ],
      "plan": "developer",
      "credentials": {
        "uri": "https://my-access-key-id:url-encoded-my-secret-access-key@p-riakcs.mydomain/service-instance-id",
        "access_key_id": "my-access-key-id",
        "secret_access_key": "my-secret-access-key"
      }
    }
  ]
}
```

As Riak CS is API-compliant with Amazon S3, any Amazon s3 client will allow you to communicate with your Riak CS instance. One that we have found simple to use is [s3curl](https://github.com/rtdp/s3curl), although others include [s3cmd](http://s3tools.org/s3cmd), the ruby library [fog](http://fog.io/) and a [java client](https://github.com/cloudfoundry-incubator/riakcs-java-client).

<a id='s3curl'></a>
##s3curl

s3curl is a Perl script. Clone it from github:

`git clone https://github.com/rtdp/s3curl`

Add credentials to `~/.s3curl`:

```
%awsSecretAccessKeys = (
    myuser => {
        id => 'my-access-key-id',
        key => 'my-secret-access-key'
    }
);
```

Edit `s3curl.pl` to add `p-riakcs.myhostname` to the known endpoints:

```
...
my @endpoints = ( 's3.amazonaws.com',
                  's3-us-west-1.amazonaws.com',
                  's3-us-west-2.amazonaws.com',
                  's3-us-gov-west-1.amazonaws.com',
                  's3-eu-west-1.amazonaws.com',
                  's3-ap-southeast-1.amazonaws.com',
                  's3-ap-northeast-1.amazonaws.com',
                  's3-sa-east-1.amazonaws.com',
                  'p-riakcs.mydomain');
...
```
*Note: If you never intend on communicating with any of the amazon services, then you can delete the existing entries (the ones beginning with 's3').*

To list bucket contents at service-instance-location:

`./s3curl.pl --id myuser -- http://p-riakcs.mydomain/service-instance-id`

To put contents file to bucket with key `mykey`:

`./s3curl.pl --id myuser --put filename -- http://p-riakcs.mydomain/service-instance-id/mykey`

*Note: curl requires you to escape any special characters in filenames - e.g. filename\\.txt*

To get file with key `mykey` from bucket:

`./s3curl.pl --id myuser -- http://p-riakcs.mydomain/service-instance-id/mykey`

<a id='s3cmd'></a>
##s3cmd

[s3cmd](http://s3tools.org/s3cmd) is a commandline client for connecting to S3 compatible blobstores.

A `.s3cfg` file is needed to configure s3cmd to talk to your Riak CS cluster.

An example is provided below. Please adjust the `access_key`, `secret_key`, `host_base`, `host_bucket`, and `proxy_host` parameters to match your Riak CS cluster config.

```
[default]
access_key = admin-key
bucket_location = US
cloudfront_host = cloudfront.amazonaws.com
cloudfront_resource = /2010-07-15/distribution
default_mime_type = binary/octet-stream
delete_removed = False
dry_run = False
encoding = UTF-8
encrypt = False
follow_symlinks = False
force = False
get_continue = False
gpg_command = None
gpg_decrypt = %(gpg_command)s -d --verbose --no-use-agent --batch --yes --passphrase-fd %(passphrase_fd)s -o %(output_file)s %(input_file)s
gpg_encrypt = %(gpg_command)s -c --verbose --no-use-agent --batch --yes --passphrase-fd %(passphrase_fd)s -o %(output_file)s %(input_file)s
gpg_passphrase =
guess_mime_type = True
host_base = p-riakcs.10.244.0.34.xip.io
host_bucket = p-riakcs.10.244.0.34.xip.io/%(bucket)s
human_readable_sizes = False
list_md5 = False
log_target_prefix =
preserve_attrs = True
progress_meter = True
proxy_host = p-riakcs.10.244.0.34.xip.io
proxy_port = 80
recursive = False
recv_chunk = 4096
reduced_redundancy = False
secret_key = admin-secret
send_chunk = 4096
simpledb_host = sdb.amazonaws.com
skip_existing = False
socket_timeout = 300
urlencoding_mode = normal
use_https = False
verbosity = WARNING
```

Bucket contents can be downloaded with the following command:

```
s3cmd -c .s3cfg sync s3://bucket-name /destination/directory
```

Uploading data to a bucket can be done like this:

```
s3cmd -c .s3cfg sync /source/directory s3://bucket-name
```

<a id='fog'></a>
##fog

*Note: requires ruby to be installed.*

Install the fog gem:

`gem install fog`

Create a ruby client object (requires fog):

```
require 'fog'

basic_client = Fog::Storage.new(
  provider: 'AWS',
  path_style: true,
  host: 'p-riakcs.mydomain',
  port: 80,
  scheme: 'http',
  aws_access_key_id: 'my-access-key-id',
  aws_secret_access_key: 'my-secret-access-key')
```
*Note: try this in irb with `irb -r 'fog'` and then create the client object.*

To list bucket contents at service-instance-location:

`basic_client.get_bucket('service-instance-id')`

To put text to bucket with key `mykey`:

`basic_client.put_object('service-instance-id','mykey','my text here')`

To get file with key `mykey` from bucket:

`basic_client.get_object('service-instance-id', 'mykey')`

<a id='java'></a>
##Java

See the [README](https://github.com/cloudfoundry-incubator/riakcs-java-client/blob/master/README.md) of our forked java-client repo.
