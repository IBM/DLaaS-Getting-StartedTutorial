## Step 0: [Pre-requisites](https://github.com/mypublicorg/pytorch-cifar10-in-ibm-cloud/blob/master/pre-req.md)

Please make sure you have completed the prequisites before starting this tutorial.
You can get the prequisites [here](https://github.com/mypublicorg/pytorch-cifar10-in-ibm-cloud/blob/master/pre-req.md)


## Step 1: Login to IBM Cloud and target the correct area 

### Step 1.1 Login Using API Key:   
Login to your IBM Cloud account using the apiKey (see [pre-requisite document](https://github.com/mypublicorg/pytorch-cifar10-in-ibm-cloud/blob/master/pre-req.md) for details)

```
$ bx login --apikey <your_api_key>
```

## Step 1.2 Target correct Region, Resource group, Org and Space.
Use the "bx target" command to target the correct Cloud parameters.
```
$ bx target -r us-south -g MITIBMWatsonAiLab -o MITIBMWatsonAiLab -s dev
```

## Step 2: Create a Watson ML Service Instance
Below `pm-20` is the name of the Watson ML Service. 
You create a new instance and give it a name, e.g., `myuseridMLinstance1`. Below, replace  <CLI_WMLi> by your chosen name.
```
$ bx service create pm-20 standard <CLI_WMLi>
```

#### 2.1. Create an Access key for accessing your Watson ML service instance
Below, replace <key_CLI_WMLi>  by a key name of your choice.

```
$ bx service key-create <CLI_WMLi> <key_CLI_WMLi>
```
#### 2.2 Retrieve credentials:
```
$ instance_id=`bx service key-show <CLI_WMLi> <key_CLI_WMLi> | grep "instance_id"| awk -F": " '{print $2}'| cut -d'"' -f2`
$ username=`bx service key-show <CLI_WMLi> <key_CLI_WMLi> | grep "username"| awk -F": " '{print $2}'| cut -d'"' -f2`
$ password=`bx service key-show <CLI_WMLi> <key_CLI_WMLi> | grep "password"| awk -F": " '{print $2}'| cut -d'"' -f2`

$ echo ""; echo "ML Instance Credentials:"; echo "instance_id: $instance_id"; echo "username: $username "; echo "password: $password"; echo ""
```

#### 2.2 Set up Environment Variables:
```
$ export ML_INSTANCE=$instance_id
$ export ML_USERNAME=$username
$ export ML_PASSWORD=$password
```

## Step 3: Create a bucket in the Cloud Object Storage (COS) to store data

A [bucket](https://datascience.ibm.com/docs/content/analyze-data/ml_dlaas_object_store.html) is a huge "folder" 
in the COS. 
You use the bucket to put and get any file or folder (e.g., your datasets).

#### 3.1. Create a cloud storage instance:

Lets create a personal cloud storage instance to hold your bucket(s) and name the instance <my_COS_instance>.
The `service-instance-create` command below creates the COS instance, and the `service-instance` command retrieves its attributes.

```
$ bx resource service-instance-create <my_COS_instance> cloud-object-storage standard global
$ bx resource service-instance <my_COS_instance>

```

#### 3.2. Get security credentials:

Now create and get the credentials to access `my_COS_instance`.
Give a name to your credentials (replace `<my_COS_key>` below).

Create key, store it and print it:

```
$ bx resource service-key-create <my_COS_key> Writer --instance-name <my_COS_instance> --parameters '{"HMAC":true}' > /dev/null 2>&1
$ access_key_id=`bx resource service-key <my_COS_key> | grep "access_key_id"| cut -d\:  -f2`
$ secret_access_key=`bx resource service-key <my_COS_key> | grep "secret_access_key"| cut -d\:  -f2`
$ echo ""; echo "COS Credentials:"; echo "access_key_id: $access_key_id"; echo "secret_access_key: - $secret_access_key"; echo ""
```
Save your keys to shell variables. (write down the keys as you'll need them again later to access your resources.)
```
export MY_BUCKET_KEY=$access_key_id
export MY_BUCKET_SECRET_KEY=$secret_access_key
```

#### 3.3 Create and configure your aws profile.
Use the `aws` tool to add `access_key_id` and `secret_access_key` to a aws-profile,
and give a name to your aws-profile (Replace <my_aws_profile> below). 

```
$ aws configure --profile <my_aws_profile>
```
The above command will ask for your `access_key_id` and `secret_access_key`.
Press enter for all other fields requested. [none]

#### 3.4. Create an alias to simplify the invocation of the aws command:

First, lets create an alias for repeating parts of the command (to avoid typing too much).

Mac OS users:
```
$ alias bxaws='aws --profile <my_aws_profile> --endpoint-url=http://s3-api.us-geo.objectstorage.softlayer.net'
```

Windows OS users:
```
doskey bxaws=aws --profile <my_aws_profile> --endpoint-url=http://s3-api.us-geo.objectstorage.softlayer.net $*
```

#### 3.5. Create a bucket:

Now, lets make a bucket and name it something unique! Buckets are named globally, which means that only one IBM Cloud account can have a bucket with a particular name. 
**NB: the bucket names may not contain upper-case, underscores, dashes, periods, etc. Just use simple text, and add your userid as part of the bucket name.  
```
$ bxaws s3api create-bucket --bucket <your-bucket-name>
```

## Congratulations you are done with the one-time SETUP!


