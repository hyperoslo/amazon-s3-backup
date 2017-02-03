# Amazon S3 bucket sync

Syncs buckets between Amazon S3 and Google Cloud storage, and sets up recurring
transfer jobs that copies contents from S3 every night at 02:00 AM.

## Configuration

1. Setup an AWS IAM user with read and list access to all buckets

```
  {
      "Version": "2012-10-17",
      "Statement": [
          {
              "Sid": "Stmt1486043994000",
              "Effect": "Allow",
              "Action": [
                  "s3:GetObject",
                  "s3:ListBucket",
                  "s3:ListAllMyBuckets",
                  "s3:GetBucketLocation"
              ],
              "Resource": [
                  "arn:aws:s3:::*"
              ]
          }
      ]
  }
```

2. Set up a Google Cloud Storage project
3. Add a Google Cloud IAM user with `editor` permissions to your project, and
download the credentials
4. It's strange, but you also need to add a `storage-transfer` account as
`editor` on your project. Do that by first setting up a bucket manually,
add create a transfer, then go to bucket permissions and copy the user 
that has been added to the bucket permissions. Use that email `storage-transfer-[someID]@partnercontent.gserviceaccount.com`
and add it in the IAM section as an editor to your project.
5. Enable Storage API: https://console.cloud.google.com/flows/enableapi?apiid=storage_component
6. Make sure the following ENV variables exist (see .envrc-example)

```
AWS_ACCESS_KEY_ID
AWS_SECRET_ACCESS_KEY
GCS_PROJECT
GCS_PRIVATE_KEY
GCS_CLIENT_EMAIL
```

That should be everything you need to get going.

## Usage

./bin/sync-buckets
