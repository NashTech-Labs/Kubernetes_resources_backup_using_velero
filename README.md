# Complete Backup of Kubernetes Resources

Collection of YAML manifest to create a complete backup of your Kubernetes resources and upload it to an Amazon S3 bucket using Velero.

## Pre-requisite

- Velero installed and configured
- AWS credentials to upload the backup to S3

## Manifest Details

### aws_keys.yml
The cloud field inside `aws_keys.yml` should contain your AWS credentials in JSON format, base64 encoded. 
Replace `<AWS_ACCESS_KEY_ID>, <AWS_SECRET_ACCESS_KEY>, and <REGION>` with your actual AWS credentials and desired region.

The JSON format looks like this:
~~~
{
  "awsAccessKeyId": "<AWS_ACCESS_KEY_ID>",
  "awsSecretAccessKey": "<AWS_SECRET_ACCESS_KEY>"
}
~~~
`Encode this JSON string using base64 encoding before placing it in the secret.`

### bucketdetails.yml
In `bucketdetails.yml` replace `<BUCKET_NAME>` with the name of your S3 bucket.

Ensure the `region` matches the region of your S3 bucket and replace `<BUCKET_NAME>` with the actual name of your S3 bucket.

### backup.yml
The `backup.yml` resource specifies a backup of **all namespaces `('*')`** and stores it in the previously defined `BackupStorageLocation` named s3-bucket. The `ttl` field specifies how long the backup should be retained; adjust this value according to your backup policy.

## Applying the Configuration
Apply these configurations to your Kubernetes cluster using kubectl:

~~~
kubectl apply -f aws-creds-secret.yaml
kubectl apply -f backupstoragelocation.yaml
kubectl apply -f backup.yaml
~~~

## Monitoring the Backup
Monitor the progress of the backup using the following command:

~~~
kubectl get backup -n velero
~~~~
And check the status of the backup with:
~~~
kubectl describe backup full-cluster-backup -n velero
~~~