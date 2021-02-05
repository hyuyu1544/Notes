# lambda

1.process the data in close geographical proximity to their users and respond to user requests at low latencies.
```
Integrate CloudFront with Lambda@Edge in order to process the data in close geographical
proximity to users and respond to user requests at low latencies. Process real-time streaming
data using Kinesis and durably store the results to an Amazon S3 bucket.
```

2.The engineer found out that S3 has an event notification whenever a delete or write operation happens within the S3 bucket.
```
Lambda function
SQS(queue)
SNS(topic)
```

3.Which of the following options will allow you to asynchronously process the request to the application from upload request to Kinesis, S3, and return a reply in the most cost-effective manner?
```
Replace the Kinesis Data Streams with an Amazon SQS queue. Create a Lambda function that will asynchronously process the requests.
```

4.EC2ThrottledException
```
Your VPC does not have sufficient subnet ENIs or subnet IPs.
You only specified one subnet in your Lambda function configuration. That single subnet runs out of available IP addresses and there is no other subnet or Availability Zone which can handle the peak load.
```
5.virtual reality (VR) and augmented reality (AR) games which use various RESTful web APIs hosted on their on-premises data center
```
Use AWS Lambda and Amazon API Gateway.
```
6.sensitive database and API credentials
```
Create a new KMS key and use it to enable encryption helpers that leverage on AWS Key Management Service to store and encrypt the sensitive information.
```
7.You are instructed by your manager to significantly reduce the user’s login time to further optimize the system.
```
Set up an origin failover by creating an origin group with two origins. Specify one as the primary origin and the other as the second origin which CloudFront automatically switches to when the primary origin returns specific HTTP status code failure responses.
Customize the content that the CloudFront web distribution delivers to your users using Lambda@Edge, which allows your Lambda functions to execute the authentication process in AWS locations closer to the users.
```





lambda是計次收費


