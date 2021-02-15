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

3. fix latency issue
```
(1)content delivery network (CDN)
  - read only website: cache is good solution
```

4. Server Name Indication(SNI) and SSL
```
SNI自定義SSL依賴於傳輸層安全協議的SNI擴展，該協議允許多個域通過包含查看者試圖連接的主機名，通過同一IP地址為SSL流量提供服務
```















Amazon CloudFront is a web service that gives businesses and web application developers an easy and cost-effective way to distribute content with low latency and high data transfer speeds. CloudFront delivers your files to end-users using a global network of edge locations.
Amazon CloudFront delivers your content from each edge location and offers the same security as the Dedicated IP Custom SSL feature. 
