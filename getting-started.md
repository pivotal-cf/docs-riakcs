#Getting Started with Riak CS

###Prerequisites:

You have created a service instance, bound it to an application, and have the binding credentials from the VCAP_SERVICES environment variable.
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
As Riak CS is API-compliant with Amazon S3, any Amazon s3 client will allow you to communicate with your Riak CS instance. One that we have found simple to use is [s3curl](https://github.com/rtdp/s3curl), although others include [s3cmd](http://s3tools.org/s3cmd), and the ruby library [fog](http://fog.io/).

##s3curl

Clone s3curl from github:

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
