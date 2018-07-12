### Other useful commands

#### Download Trained model files
```
$ bxaws s3 cp s3://$bucket_name/<training run ID> ./trainedmodel --recursive
ls ./trainedmodel 
```

#### List the buckets
```
$ bxaws  s3api list-buckets 
```

#### List the contents of your bucket
```
$ bxaws s3 ls s3://$bucket_name/
```

#### Query status of the job
```
$ bx ml show training-runs <training run ID>
```

#### View log files 
```
$ bxaws s3 ls s3://$bucket_name/<training_id>/learner-1/
```

#### Delete files from your bucket
```
$ bxaws s3 rm s3://$bucket_name/fileOrDirectoryName  --recursive
``` 
