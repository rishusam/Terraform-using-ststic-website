# Terraform-using-ststic-website
Creating a Terraform configuration file for hosting a static website on AWS S3 involves defining the necessary AWS resources and their configurations. Below is an example of a Terraform configuration file (`main.tf`) for setting up static website hosting on AWS S3:

```hcl
# Define the provider
provider "aws" {
  region = "us-east-1"  # Specify your desired AWS region
}

# Define the S3 bucket for hosting the static website
resource "aws_s3_bucket" "static_website_bucket" {
  bucket = "your-unique-bucket-name"  # Specify a unique bucket name for your website
  acl    = "public-read"  # Set bucket ACL to public-read for website hosting

  website {
    index_document = "index.html"  # Specify the main index file for your website
    error_document = "error.html"  # Specify the error document for your website
  }
}

# Configure the bucket policy to allow public access to the website content
resource "aws_s3_bucket_policy" "static_website_bucket_policy" {
  bucket = aws_s3_bucket.static_website_bucket.bucket

  policy = <<EOF
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Principal": "*",
      "Action": "s3:GetObject",
      "Resource": "${aws_s3_bucket.static_website_bucket.arn}/*"
    }
  ]
}
EOF
}

# Output the website URL
output "website_url" {
  value = aws_s3_bucket.static_website_bucket.website_endpoint
}
```

Before running the Terraform commands, ensure you have Terraform installed and configured with your AWS credentials. Then, create a directory for your Terraform configuration and place the `main.tf` file inside it.

To initialize the Terraform configuration, run the following command in your terminal:

```bash
terraform init
```

To preview the changes that Terraform will make, run:

```bash
terraform plan
```

If the plan looks good, apply the changes to create the resources on AWS:

```bash
terraform apply
```

After Terraform has finished applying the changes, it will output the URL of your static website. You can then upload your static website files (e.g., HTML, CSS, JavaScript) to the S3 bucket, and they will be publicly accessible via the provided URL.

Remember to replace placeholders like `your-unique-bucket-name` with your desired values before running Terraform. Also, ensure that you have appropriate AWS permissions to create and manage S3 resources.
