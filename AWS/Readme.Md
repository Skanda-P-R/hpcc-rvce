<h2>For integrating AWS with HPCC-platforms we have found 2 approaches as listed above. Each have their own Advantages and Limitations</h2>

For both the approaches, the verisoning concept will be the same. Here the original files which are in their latest version are kept in one bucket(here awsbkt870) and their every other verisons are kept in another(here versions870). Following are the steps to get the versions in destination_bucket(versions870) from source_bucket(awsbkt870):

<h4>Step 1:</h4> Enable the versioning of the source_bucket
<h4>Step 2:</h4> Create a lambda function on AWS and add the code in lambda_function.py in the above file. Change the bucket names according to yours
<h4>Step 3:</h4> Deploy the code
<h4>Step 4:</h4> Attach the following policies(in execution role) to the function so that it is able to do the operations
    AmazonS3ReadOnlyAccess	
<br>
    AWSLambdaBasicExecutionRole	
<br>
    AWSLambdaRole	
<br>
<h4>Step 5:</h4> So now, the last step is to continuosly monitor source_bucket(awsbkt870) for upgraded versions or new files we should add the trigger. It can be done in two ways:
<br>
a) Go the lambda function --> select add trigger --> select source=s3, bucket=source_bucket, event= all object create events --> select add
    This will add the trigger such that whenever a new version is added or a new file is created in the source_bucket the function will run to copy the verisons to       destination_bucket
<br>
b) Put the lambda function in cron To do this go the lambda function --> select add trigger --> select cloud watch events --> select create new rule --> slect rule type as schedule expression --> type `rate(1 minute)`
<br>
  This will run the lambda function every minute to monitor
<br>
For more details regarding this see the above versionUpdater(1).yaml file . The example files for incremental_data_push.csv/xml/json are present in the examples_used directory.
