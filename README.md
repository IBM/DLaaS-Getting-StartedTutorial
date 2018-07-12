# MITIBMCloud


**Note: These steps use a Command Line Interface (CLI). There is an alternative browser used interface** 

### [Steps for one time Setup:](https://github.com/mypublicorg/pytorch-cifar10-in-ibm-cloud/blob/master/onetimesetup.md)

Step 0. Pre requisites

Step 1. Login to your bluemix account

Step 2. Create a Watson ML Instance

    2.1 Setup a Watson ML Instance : Create WML Access key  
    2.2 Set up Environment variables
    
    
Step 3. Define a [Cloud Object Storage](https://www.ibm.com/cloud/object-storage/faq) Instance to store your data.

    3.1. Create a cloud storage instance
    3.2. Get security credentials
    3.3 Create and configure your aws profile
    3.4. Create an alias
    3.5. Create a bucket
    
You can find the one time setup instructions [HERE](https://github.com/mypublicorg/pytorch-cifar10-in-ibm-cloud/blob/master/onetimesetup.md)
    
 
 ### [Steps for the Demo:](https://github.com/mypublicorg/pytorch-cifar10-in-ibm-cloud/blob/master/demo.md)
 
 0: Get a dataset
 
 1: Upload your dataset to the bucket
 
 2: Edit your manifest file
 
    2.1. Copy the template manifest
    
    2.2. Edit the configuration file
 
 3: Send code to run on Watson Studio!
 
    3.1. Zip all the code and models into a .zip file
    
    3.2. Send your code and manifest to IBM Watson Studio
 
 4: Monitor the training
 
 **You can find the Steps for Demo [HERE](https://github.com/mypublicorg/pytorch-cifar10-in-ibm-cloud/blob/master/demo.md)**

 **Click [HERE](https://github.com/mypublicorg/pytorch-cifar10-in-ibm-cloud/blob/master/usefulcommands.md)  for Other useful commands**
