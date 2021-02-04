# dojo - ec2

## multi-type instance

###
1. an Auto Scaling group of Amazon EC2 instances
```
Amazon S3或Amazon EFS。兩種服務都有生命週期配置
```  
2. EBS-backed EC2 instance which uses Simple Workflow Service (SWF) to handle its sequential background jobs
```
You can use a combination of EC2 and SWF for the following scenarios:

Managing a multi-step and multi-decision checkout process of an e-commerce mobile app.
Orchestrating the execution of distributed business processes
```

3. globally to upload and store several types of files,  old files yet still provide durability and high availability.
```
Use Amazon S3 and create a lifecycle policy that will move the objects to Amazon S3 Glacier after 2 years.
Use Amazon S3 and create a lifecycle policy that will move the objects to Amazon S3 Standard-IA after 2 years.
```

4. EC2 instances in your VPC, trigger the Amazon EC2 API to request 50 EC2 instances in a single Availability Zone.
```
There is a vCPU-based On-Demand Instance limit per region which is why subsequent requests failed. Just submit the limit increase form to AWS and retry the failed requests once approved
```

 Scheduled Reserved Instances: 使用計劃的預留實例
 On-Demand EC2 instances : 穩定但成本高（會一直開著？都要算錢？）, 20 Reserved Instances per region
 Spot EC2 Instances: 會被打斷,不適合處理關鍵數據
 Dedicated Hosts :
 Nitro-based EC2 instance：
