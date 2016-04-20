In your picture_uploader.rb file under app/uploaders folder, replace the line that says storage :file with the following:
```ruby
if Rails.env.production?   
  storage :fog 
else   
  storage :file
 end
```
Sign up for Amazon web services at aws.amazon.com

1) Create IAM user
2) Create S3 bucket
3) Create policy with s3 bucket details
4) Attach policy to IAM user created

Here is a sample policy, replace the code with your s3 bucket name as needed below:
```json
{
	"Version": "2012-10-17",
	"Statement": [
	{
		"Effect": "Allow",
		"Action": "s3:*",
		"Resource": [
			"arn:aws:s3:::yours3bucketname",
			"arn:aws:s3:::yours3bucketname/*"
		]
	},
	{
		"Effect": "Allow",
		"Action": "s3:ListAllMyBuckets",
		"Resource": "arn:aws:s3:::*"
	}
	]
}
```