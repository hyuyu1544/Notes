# cloud front

- low latency and high data transfer speeds
- CloudFront delivers your files to end-users using a global network of edge locations

1. policy
```
Grant the CloudFront origin access identity(OAI) the applicable permissions on the bucket.
Deny access to anyone that you don’t want to have access using Amazon S3 URLs.
```

2. control who can access your content
```
(1)signed URLs:
  - You want to use an RTMP distribution. Signed cookies aren’t supported for RTMP distributions.
  - You want to restrict access to individual files, for example, an installation download for your application.
  - Your users are using a client (for example, a custom HTTP client) that doesn’t support cookies.
(2)signed cookies
  - You want to provide access to multiple restricted files, for example, all of the files for a video in HLS format or all of the files in the subscribers’ area of a website.
  - You don’t want to change your current URLs.
```


















Amazon CloudFront is a web service that gives businesses and web application developers an easy and cost-effective way to distribute content with low latency and high data transfer speeds. CloudFront delivers your files to end-users using a global network of edge locations.
Amazon CloudFront delivers your content from each edge location and offers the same security as the Dedicated IP Custom SSL feature. 
