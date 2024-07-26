# 2024-07-26

This was presented as a PD session on 2024-07-26.

## GitHub

- Create a new repository
  - <https://github.com/new>
  - Owner
    - SignalWhisperer
  - Repository name
    - pd-session-2024-07-26
  - Private

## Route 53

- Create hosted zone
  - Domain name
    - my-awesome-website.example.com
- Delegate the DNS name
  - Copy the NS records
  - Create NS records in the parent hosted zone

## S3

- Create bucket
  - Bucket name
    - my-awesome-website-1234567890

## CloudFront

- Create distribution
  - Origin domain
    - my-awesome-website-1234567890.s3.us-west-2.amazonaws.com
  - Origin path
    - /
  - Name
    - my-awesome-website.example.com
  - Origin access
    - Origin access control settings (recommended)
  - Origin access control
    - Create new OAC
      - Name
        - my-awesome-website
    - my-awesome-website
  - Viewer
    - Viewer protocol policy
      - Redirect HTTP to HTTPS
    - Allowed HTTP methods
      - GET, HEAD
  - Web Application Framework (WAF)
    - Do not enable security protections
  - Price class
    - Use only North America and Europe
  - Alternate domain name (CNAME)
    - my-awesome-website.example.com
  - Custom SSL certificate
    - Request certificate
      - Fully qualified domain name
        - my-awesome-website.example.com
    - Create records in Route 53
    - Wait for certificate to be issued
  - Default root object
    - index.html
- Copy policy
  - Go to S3 bucket
  - Permissions
  - Edit Bucket policy
    - Paste policy

## Back to Route 53

- Create record
  - Name
    - leave blank
  - Type
    - A
  - Alias
    - true
  - Route traffic to
    - Alias to CloudFront distribution
    - my-awesome-website.example.com
- Add another record
  - Name
    - leave blank
  - Type
    - AAAA
  - Alias
    - true
  - Route traffic to
    - Alias to CloudFront distribution
    - my-awesome-website.example.com

## IAM

- Create role
  - Trusted entity type
    - Web identity
  - Identity provider
    - Create new
      - Provider type
        - OpenID Connect
      - Provider URL
        - https://token.actions.githubusercontent.com
      - Audience
        - sts.amazonaws.com
  - Identity provider
    - tokens.actions.githubusercontent.com
  - Audience
    - sts.amazonaws.com
  - GitHub organization
    - SignalWhisperer
  - Role name
    - MyAwesomeWebsiteAutomation
- View role
  - Permissions policies > Add permissions > Create inline policy
    - Service
      - S3
    - Actions allowed
      - ListBucket
      - GetBucketLocation
    - Resources
      - Resource bucket name
        - my-awesome-website-1234567890
  - Add more permissions
    - Service
      - S3
    - Actions allowed
      - PutObject
      - GetObject
      - DeleteObject
    - Resources
      - Resource bucket name
        - my-awesome-website-1234567890
      - Resource object name
        - Any object name
  - Policy name
    - S3Sync
  - Permissions policies > Add permissions > Create inline policy
    - Service
      - CloudFront
    - Actions allowed
      - CreateInvalidation
    - Resource distribution
      - (enter distribution id)
    - Policy name
      - CFInvalidation

## GitHub Secrets

- Repository Settings
  - <https://github.com/SignalWhisperer/pd-session-2024-07-26>
- Secrets and variables > Actions
  - Secrets
    - AWS_ROLE_TO_ASSUME
      - arn:aws:iam::1234567890:role/MyAwesomeWebsiteAutomation
    - AWS_S3_BUCKET
      - my-awesome-website-1234567890
    - AWS_CLOUDFRONT_DISTRIBUTION
      - (enter distribution id)
  - Variables
    - AWS_REGION
      - us-east-1

## Code

- See local code
- Commit
- Push
- Go to GitHub Actions
  - <https://github.com/SignalWhisperer/pd-session-2024-07-26/actions>

## Enjoy

- <https://my-awesome-website.example.com>
