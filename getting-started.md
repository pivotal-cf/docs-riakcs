#Getting Started with Riak CS

###Assumptions:

You have already created an instance of Riak CS via the broker, and have received VCAP_SERVICES info in the form below:
```
"VCAP_SERVICES":
{
  "user-provided": [
    {
      "name": "heyo",
      "label": "user-provided",
      "tags": [],
      "credentials": {},
      "syslog_drain_url": ""
    }
  ],
  "p-riakcs": [
    {
      "name": "asdf",
      "label": "p-riakcs",
      "tags": [
        "riak-cs",
        "s3"
      ],
      "plan": "developer",
      "credentials": {
        "uri": "https://my-access-key-id:url-encoded-my-secret-access-key@myriak.hostname/service-instance-location",
        "access_key_id": "my-access-key-id",
        "secret_access_key": "my-secret-access-key"
      }
    }
  ]
}
```
As Riak CS is API-compliant with Amazon S3, any Amazon s3 client will allow you to communicate with your Riak CS instance. One that we have found simple to use is [s3curl](https://github.com/rtdp/s3curl), although others include [s3cmd](http://s3tools.org/s3cmd), and the ruby library [fog](http://fog.io/).

##s3curl

###Pre-requisites
Clone s3curl from github:

`git clone https://github.com/rtdp/s3curl`

Add credentials to `~/.s3curl`:
```
%awsSecretAccessKeys = (
    friendlyuser => {
        id => 'my-access-key-id',
        key => 'my-secret-access-key'
    }
);
```

Edit `s3curl.pl` to add `myriak.hostname` to the known endpoints:

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
                  'myriak.hostname');
...
```
*Note: If you never intend on communicating with any of the amazon services, then you can delete the existing entries (the ones beginning with 's3').*
###Operation

To list bucket contents at service-instance-location:

`./s3curl.pl --id friendlyuser -- http://riak.hostname/service-instance-location`

To put contents file to bucket with key `mykey`:

`./s3curl.pl --id friendlyuser --put filename -- http://riak.hostname/service-instance-location/mykey`

*Note: curl requires you to escape any special characters in filenames - e.g. filename\\.txt*

To get file with key `mykey` from bucket:

`./s3curl.pl --id friendlyuser -- http://riak.hostname/service-instance-location/mykey`

##fog

###Pre-requisites

Create a ruby client object (requires fog):

```
require 'fog'

basic_client = Fog::Storage.new(
  provider: 'AWS',
  path_style: true,
  host: 'myriak.hostname',
  port: 80,
  scheme: 'http',
  aws_access_key_id: 'my-access-key-id',
  aws_secret_access_key: 'my-secret-access-key')
```

###Operation
To list bucket contents at service-instance-location:

`basic_client.get_bucket('service-instance-location')`

To put text to bucket with key `mykey`:

`basic_client.put_object('service-instance-location','mykey','my text here')`

To get file with key `mykey` from bucket:

`basic_client.get_object('service-instance-location', 'mykey')`
