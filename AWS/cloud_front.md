# cloud front

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












## 
1.You need to secure your application by allowing multiple domains to serve SSL traffic over the same IP address.
```
Generate an SSL certificate with AWS Certificate Manager and create a CloudFront web distribution. Associate the certificate with your web distribution and enable the support for Server Name Indication (SNI).
```

2.member-only access to some of its high quality media files
```
Use Signed Cookies to control who can access the private files in your CloudFront distribution by modifying your application to determine whether a user should have access to your content. For members, send the required Set-Cookie headers to the viewer which will unlock the content only to them.
```
```
Use signed URLs for the following cases:

– You want to use an RTMP distribution. Signed cookies aren’t supported for RTMP distributions.

– You want to restrict access to individual files, for example, an installation download for your application.

– Your users are using a client (for example, a custom HTTP client) that doesn’t support cookies.

Use signed cookies for the following cases:

– You want to provide access to multiple restricted files, for example, all of the files for a video in HLS format or all of the files in the subscribers’ area of a website.

– You don’t want to change your current URLs.
```
3. the load on the website has increased which resulted in slower response time for the site visitors.
```
Use Amazon CloudFront with website as the custom origin.
Use Amazon ElastiCache for the website's in-memory data store or cache.
```
4.application’s origin server is being hit for each request instead of the AWS Edge locations,
```
The Cache-Control max-age directive is set to zero.
```
5.Amazon S3 to serve high-quality photos
```
Configure your S3 bucket to remove public read access and use pre-signed URLs with expiry dates.
```

6.Using CloudFront, what method would be used to serve content that is stored in S3, but not publicly accessible from S3 directly?
```
Create an Origin Access Identity (OAI) for CloudFront and grant access to the objects in your S3 bucket to that OAI.
```

7.launch a news website which is expected to be visited by millions of people around the world.
```
Edge Location
```







Amazon CloudFront is a web service that gives businesses and web application developers an easy and cost-effective way to distribute content with low latency and high data transfer speeds. CloudFront delivers your files to end-users using a global network of edge locations.
Amazon CloudFront delivers your content from each edge location and offers the same security as the Dedicated IP Custom SSL feature. 
