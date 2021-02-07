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
 
 ## note by FQA
 
 - enables “compute” in the cloud
 - AMI (Amazon Machine Image):打包的環境
 - 最多購買20個預留實例，再多要透過申請
 - SMTP endpoint policy -> port 25
 - Service level agreement (SLA): Monthly Uptime Percentage of at least 99.99% EC2 and EBS within a Region.
 
 ### Instance Pricing Modules
 
 - EC2 On-Demand Instance:
  - five vCPU-based instance limits(region basis), 再多要透過申請
  - 以秒計費, 最低的計價需要60秒
 - Reserved Instances:
  - 提供了大幅折扣(最高可達75%)，但需要先預付一筆金額購買 AWS 服務內的服務時數，價格依據購買的服務等級與時間會有不同的折扣(1 年期或 3 年期。可彈性變更自己預留執行個體的可用區域、執行個體大小及類型)
  - 
 - Spot Instances:
  - 以競標的方式標用服務，可以用比較低廉的價錢租用機器，但缺點是隨時可能會中斷，所以程式必須定時記錄進度，自動處理重跑的流程。當出價不足以標到該服務時，會先關起來 (Stop)，等到價錢低於出價後就會再打開機器，這時候的狀態就會恢復(必須是使用 EBS 的機器才有支援)。當你想用的時段越少人搶標，選用這樣的計價方式就越划算。
 - Dedicated Hosts:
  - Dedicated Hosts 簡單來講就是租用實體主機，所以會根據執行個體系列、區域及付款選項而有不同。無論在特定專用主機上選擇啟動的執行個體數目或大小為何，都只需支付每個作用中專用主機每小時的費用，不會針對執行個體的使用時間計費。但好處是上面可以安裝你指定的軟體與套件。
  - 而 Dedicated Hosts 也可以搭配 Reserved Instances 購買並預付較長期的合約換取更佳的價格折扣。
  
### Instance Types
- https://www.evernote.com/shard/s201/client/snv?noteGuid=c4deb1da-8a0b-4dcc-ad19-201c4bbb7085&noteKey=de2bc90c858d8333&sn=https%3A%2F%2Fwww.evernote.com%2Fshard%2Fs201%2Fsh%2Fc4deb1da-8a0b-4dcc-ad19-201c4bbb7085%2Fde2bc90c858d8333&title=AWS%2B-%2BEC2
- https://ithelp.ithome.com.tw/articles/10192098

### Storage
 - 在EBS-backed instance，預設行為是Instance terminated時，root EBS也會被刪掉，但可以透過設定讓他們的lifecycle獨立，讓EBS不會自動刪除
 - Snapshots備份到S3是Incrementally
 - EBS可以在使用中時改變，type跟size都可以
 - EBS always be in the same availability zone as the EC2 instance
 - Snapshot可以encrypted，如果是從encrypted volume create/restore的snapshot，都會是encrypted
 - Snapshot可以share，但只有unencrypted的可以share

  
# ref
https://ithelp.ithome.com.tw/articles/10192098

