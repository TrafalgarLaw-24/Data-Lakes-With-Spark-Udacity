# Data Lake project - Udacity

## Table of Contents

1. [Summary](#summary)
2. [Setting up a Spark Cluster](#spark)
3. [How to run the script](#run)
4. [About](#about)

## <a name="summary"></a>Summary
The purpose of this project is to provide Sparkify the necessary tools to retrieve vital and business oriented information about their services provided. Their current focus is to be able to retrieve information easily and fast about which songs users are listening to. Information about this is stored in logfiles but it is not easy to search or aggregate data from these logfiles. In the Data Lake assignment we have been asked to transform data from the logfiles together with meta data about songs (also stored in files) into a business oriented (star) database design that will allow Sparkify to meet their business need related to for example finding out which songs their users are listening to and when.

In the Data Lake project we will read the logfiles and song data files into Spark and also use Spark to transform the files and store Spark parquet files that follows the business oriented star schema. I.e. we will create artist, song, user, time dimension tables and a songplay fact table stored as Spark parquet files.

## <a name="run"></a>How to run the script

### Setup a Spark Cluster using AWS Console
The script _etl.py_ is designed to run on a Spark cluster and it is therefore necessary to setup the cluster before the script can be run. The next paragraphs describes the steps used in order to setup a cluster for this project and how to run the script on the cluster.

#### Create keys
To login to the master node of your Spark cluster it is necessary to generate a keypair for ssh. The private part of the key will have to be stored on the machine you use to login from and the public key will be on the master Spark node.

Login to your AWS account and select the EC2 service. Select *Key Pairs* from *Network and Security* in the right hand side menu. Click on the  *Create Key Pair* button and give it an appropriate name (the private part of the key will be automatically downloaded to your local computer. Store it in a safe place and never share it anywhere).

#### Setup Spark Cluster
The easiest way to setup a Spark cluster is to use the (mostly) automated Elastic Map Reduce (EMR) service. The steps used in this project are as follows:

1. Login to your AWS account and select the EMR service
2. Click on the *Create Cluster* button
3. Give the cluster a name and make sure that *Launch Mode*: *Cluster* is selected
4. Select hardware configuration *m5.xlarge* and software configuration release *emr-5.27.0* and application *Spark*.
5. Select the number of nodes in your cluster
5. In the *Security and Access* configuration make sure to select the keypair you crerated in the previous section
6. Scroll down and click the final *Create Cluster* button

After a while the Spark cluster should be up and running.

### Run etl.py - standalone
To transfer the python script to your Spark master node, first transfer it to S3. I created a bucket that I have used for this purpose (and for storage of parquet files) and then used _aws s3 cp s3://bucket_name/etl.py ._ to copy it to the EMR master node (after logging in to the master node using ssh).

Note: You will have to add a rule to the firewall for the master node that allows inbound traffic from your IP address first (click on the *Security groups for* master in the cluster overview to do this before logging into master node).


_ssh -i ~/.ssh/file_name.pem hadoop@ec2-xxx-xxx-xxx-xxx.us-west-2.compute.amazonaws.com_

Once logged in, you can run the python script like below (after you have copied it to the master node):

_spark-submit etl.py_

##<a name="about"></a> About:
This project is done as part of Udacity Data Engineer Nano degree program
