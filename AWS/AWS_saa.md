# Introduction to AWS


## Storage Services


## Database Services


## Compute and Networking Services


## Managemant Services


## Application Services


## Analytics and Machine Learning


## Security, Identity & Compliance


## Developer, Media, Mobile, Migration, Business, IoT
工具介紹
### Developr Tools
Cloud9 -> intergrated development environment

CodeStar -> CICD pipeline (have a proj management dashboard) powered by Atlassian JIRA

X-Ray -> analyze and debug applications

CodeCommit -> a git repository

CodePipeline -> CICD

CodeBuild -> compiles source code which already deploy on AWS ]

CodeDeploy -> 自動部署在不同機台上，像是ec2, lambda... ]

### AWS Media Services
Elemental MediaConvert -> file base (轉換video格式到video main contant)

Elemental MediaPackage -> 傳送準備video contant到網路, 可以使用數位權益管理來保護版權

Elemental MediaTailor -> 安插廣告進入video streams

Elemental MediaLive -> 可以坐直播等級的節目

Elemental MediaStore -> 給影片使用的儲存系統

Kinesis Video Streams -> 可以連到機器學stream服務或其他應用程式（把雲端的影片對外串接）

### Mobile Services
AWS Mobile Hub -> 可以簡單的配置mobile應用程式

AWS Device Farm -> app test service (ios, android and 網路應用程式都可以測試)

AWS AppSync -> 給手持或網路應用程式使用的graph QL

### AWS Migration Services
AWS Application Discovery Service ->蒐集企業內部資訊，以加密格式儲存，已協助遷移

AWS database migration service -> 遷移數據庫（不同database engine type也可以遷移）

AWS server migration service -> 自動遷移on-premise workloads到AWS上，可以降低成本跟最小化停機時間

AWS snowball -> 遷移本地數據至AWS （portable petabyte scale data storage device）

AWS snowball -> 遷移本地數據至AWS （portable petabyte scale data storage device）

### Business Productivity and App Streaming

Amazon WorkDocs -> 支持多種（35種）文件類型託管及協作

Amazon WorkMail -> email and calender service

Amazon chime -> 線上會議服務

Amazon Workspaces -> a fully managed secure desktop as a service (?)

Amazon AppStream -> a fully managed secure application streaming service (?)

### Internet of Things (IoT) 物聯網
AWS IoT -> 讓嵌入式設備（embedded devices）可與cloud applications交互

Amazon freeRTOS -> 給microcontrollers的操作系統，可以連到AWS的物聯網

AWS Greengrass -> local AWS lambda functions, messaging data caching sync and machine learning applications (排成任務)

### Game Development
Amazon Gamelift -> deploy scale and manage game servers 

Amazon Lumberyard -> a game development environment and cross-platform triple-a game engine (?)


# CLI (Command Line Interface)

### Advanced AWS Command line Interface (CLI)

The AWS API -> AWS CLI Appliation -> AWS Cloud9 -> CLI lab

#### The AWS API
remote下指令其實是送http/s calls
  - 要通過安全憑證（身份認證）的用戶方可使用api
    - username and password (console access)
    - access key ID and secret (CLI)
    - temporary credentials (SDKs, like google account, facebook ...,通常沒有aws account會用這個)
  - API calls log on `CloudTrail` service 
  - jpg here
  
#### AWS CLI Appliation 
  - Window, Mac and Linux都可用 (Powershell/shell) 
  
#### AWS Cloud9 IDE
  - running on EC2
  - high security (因為不必下載IAM credentials)
  
#### CLI lab
`aws --version`

create a s3 bucket: `aws s3 mb s3://<bucket_name>`

cp file into s3 bucket: `aws s3 cp <file_name> s3://<bucket_name>`

remove file in s3: `aws s3 rm s3://<bucket_name>/<file_name>`

remove s3 bucket: `aws s3 rb s3://<bucket_name>`

### AWS Elastic Beanstalk
what's elastic beantalk -> highly available & fault tolernt architeture -> deployment options

- can deploy and manage applications on environments
- automatically do `capacity`, `load balancing`, `scaling` and `health monitoring`
- 支持多種語言(事實上，使用者只要會寫code就好，部署跟監控之類的事可以無腦交給它)
- jpg here
- jpg here
- 感覺跟k8s做的事很像（只是k8s若沒有資源就不會部署，這個可以自己在開新的ec2）
- deployment options: all at one, 增加ec2, rolling(a batch or額外的batch),two environments(blue-green，有hand-on)

### AWS Free Tier
https://aws.amazon.com/tw/free

### Business case for AWS
(如何說服你的老闆將AWS服務導入公司)

- Value of AWS
  - 收費方式 （capital expense -> variable expense）
  - 不用猜測容量 (使用多少算多少)
  - 全球都有 (low latency)
  - value to migration in business:
    - costs savings
    - staff productivity
    - operational resilience
    - business agility
- AWS Pricing Calcultor
  - https://calculator.aws/
- Migration
  - business, people, process, platform, operations and security
  - jpg here (effort figure)
  - https://aws.amazon.com/tw/prescriptive-guidance/
- Amazon Inspector
- Compliance
  - AWS config
- AWS Support
  - https://aws.amazon.com/premiumsuooort/compare-plans

###  AWS Architecture and Compliance
https://aws.amazon.com/tw/architecture/
https://aws.amazon.com/tw/compliance/

### Bulletproof Serverless HTML5 Websites

### Architecture Discussion and Lab Preparation
  
###  Purchasing Domain Names with Route 53

## [Hands On] - Connecting to EC2 Instances
以linux instances為例, 以`SSH`連接EC2

- ssh connect to ec2 (ssh 使用TCP, 所以port用`22`)
  0. auto-assign Public IP : Enable
  1. 生成.pem 檔  (launch instance時，就會生成)
  2. 在security group把ssh port打開
  3. 在 EC2 畫面點connect, 會有ssh指令, 複製貼上在`.pem`的目錄下即可
  
  
## [Hands On] - Connecting to EC2 Windows Instances
以window instances為例, 以`RDP`連接EC2

- RDP(`Remote Desktop Port`) connect to ec2 (window instances) (rdp port用`3389`)
  1. 生成.pem 檔 
  2. 在security group把rdp port打開
  3. 在 EC2 畫面點actions > get windows password , then upload `.pem`
  4. 上述步驟會返回`host`, `account`, `password`
  5. 使用remote desktop, 輸入上述資訊即可遠端操作window instances


## [Hands On] - Mac OS X connecting to Linux and Windows EC2 Instances
主要是若mac os沒有安裝ssh,就要多一個步驟 `chmod 400 xxx.pem`


## [Hands On] - Creating a NodeJS Server and Custom AMI
- 手把手教學安裝`git`,`nodejs` ...等在 linux EC2 Instances
- 使用AMI, 可以快速複製EC2
    - actions > image > create image
    - AMI (Amazon Machine Images): https://docs.aws.amazon.com/zh_tw/AWSEC2/latest/UserGuide/ec2-instances-and-amis.html
    - 可以跨region


## [Hands On] - Elastic Block Store (EBS)
EBS是 EC2預設的磁碟區
- snapshop(備份磁碟區)
  - actions > snapshop > create snapshop
  - actions > volume > create volume (for the snapshop)
  - 下次生成新的ec2時，就可以掛儲存的snapshop在EBS


## Advanced - Elastic Container Service (ECS)

- ECR(Elastic Container Registry)
  - 放映像檔的地方（就像docker hub）
  - 對container 加密
- ECS Tasks
  - json file
  - define multiple containers in one task definition
  - application can span multiple task definitions 
- Deployment on EC2 & Serverless(fargate)(?)
  - fargate manages cluster for you
- ECS and EKS
  - ECS : 管理container
  - EKS : AWS上的k8s, 管理


## [Hands On] - ECS on AWS Fargate

- Fargate 
  - on ECS 時，是預設的 Compatibilities
  - 無伺服器運算引擎
  - 用來指定每個應用程式的資源和支付資源費用
  - 提供任務與 pod 自己的隔離運算環境
  - https://aws.amazon.com/tw/fargate/?whats-new-cards.sort-by=item.additionalFields.postDateTime&whats-new-cards.sort-order=desc&fargate-blogs.sort-by=item.additionalFields.createdDate&fargate-blogs.sort-order=desc

  
## Elastic File Service (EFS)
- a simple scalable file storage for use with Amazon ec2 instances 
- network attached storage(NAS) 沒有附加到ec2上，與EBS不同
- can be accessed by multiple ec2 instances at the same time 
- 優勢：
  - fully managed service
  - filesystem grows and shrinks automatically to petabytes size
  - 僅支付使用的存儲空間，沒有最低費用
  - 吞吐量會根據需求自動擴展，以確保一致的低延遲
  - support thousands of connections
  - multiple AZ
- 劣勢：
  - not available in all regions 
  - cross region capability not available
  - 與s3和EBS相比，設置更為複雜
- Mount target
  - required to access EFS from a VPC
  - VPC NFS endpoint
  - has an IP address and a DNS name you can use in the Linux mount command
  - can be mounted by multiple EC2 instances
  - can be in different subnet to instance but different AZ
- accessing file system from ec2
  - requires an NFS client
  - mount file system using the Linux mount command similar to EBS and instance store
  - file system DNS name (easiest) or mount point DNS name can be used to mount EFS on EC2.
- EFS security
  - IAM user permissions for create, update and delete
  - EC2 security group can be set as inbound rules for EFS and vice versa
  - NACLs can be used to control traffic
  - Linux/Unix file root-only permissions by default

補充:
  - file: EFS
  - obj: S3, Glacier
  - block: EBS, Instance store


## [Hands On] - Elastic File Service (EFS)
EFS share, EFS mount points
- create EFS share
- security group for EFS
- 開一個ec2，point到EFS
  - 進到ec2下指令
  - 要使用DNS name
  - 指令在EFS找得到


## Simple Storage Service (S3)
- S3 storage classes
  - bucket:
    - container for objects stored in S3
    - volume and object 無上限
    - 命名限制：不能重複, 3-63個字符, 不能有ip格式, 小寫, 不能有大寫或底線
  - object:
    - 整體存在bucket中（data and metadata）
    - 0 bytes ~ 5TB
    - 單個obj最大上傳 5GB
    - 分段上傳可到 100MB~5TB
  - S3 is a web store not a file system
    - eventually consistent: 跨AZ得到的obj保持一致
    - read after right consistency for new objects
    - flow: `upload new obj` -> `synchronised` -> `S3 index update` -> `success returned`
    -  updates or deletes them by updates are also called overwrite puts they're not going to be read after right or read after delete consistent they're eventually consistent
    - flow: `update or delete存在的obj` -> `success returned` -> `synchronised` -> `S3 index update`
  - Secure by default, but can be modify by:
    - IAM policies
    - bucket policies
    - access control lists (ACL)
  - <a href="https://ppt.cc/fUTCDx"><img src="https://ppt.cc/fUTCDx@.png" border="0" alt="PPT.cc縮圖服務" title="PPT.cc縮圖服務"></a>
  - <a href="https://ppt.cc/fXTdhx"><img src="https://ppt.cc/fXTdhx@.png" border="0" alt="PPT.cc縮圖服務" title="PPT.cc縮圖服務"></a>
  - <a href="https://ppt.cc/fLBvVx"><img src="https://ppt.cc/fLBvVx@.png" border="0" alt="PPT.cc縮圖服務" title="PPT.cc縮圖服務"></a>
- S3 Glacier
  - 歸檔解決方案
  - 最低成本的儲存obj
  - 99.99999..% of durabilty across 3 AZs
  - 3 retrieval options:
    - Expedited (1-5分鐘)
    - standard (3-5小時)
    - bulk (5-12 hours)
- lifecycle management
  - delete an object after expiry time
  - archive an object to Glacier after expiry time
  - can be restored back from glacier to Amazon S3 
  - <a href="https://ppt.cc/f10MMx"><img src="https://ppt.cc/f10MMx@.png" border="0" alt="PPT.cc縮圖服務" title="PPT.cc縮圖服務"></a>
- cross-region replication (跨ＡＺ複製)
  - <a href="https://ppt.cc/fbnG3x"><img src="https://ppt.cc/fbnG3x@.png" border="0" alt="PPT.cc縮圖服務" title="PPT.cc縮圖服務"></a>


## week3

### Advanced Virtual Private Cloud (VPC)
- subnets addressing
  - vpc 可以跨AZ, 但不能跨region (一個region有多個AZ)
  - vpc在不同AZ下，可以切分不同subnets(e.g. VPC subnet 1, ...)
  - TCP/IP(v4)
    - TCP/IP(v4) is 32 bit (255.255.255.255 -> 11111111.11111111.11111111.11111111)
    - private IP(有三類) (only used in private network)
      - 10開頭 （e.g. 10.0.0.0/8）
      - 172.16-31開頭 (e.g. 172.16.0.0/12)
      - 192.168開頭（e.g. 192.168.0.0/16） 
    - /數字 -> subnet mask （前面幾位數都是1，不給變）(classless inter-domain routing, CIDR)
    - IP尾數 0,1,2,3,255 是AWS御用位址（不給使用者使用）
    - 舉例： 如果VPC的IP是192.168.1.0, 他的subnet就可以使用 `192.168.1.4`~`192.168.1.254`
    - 練習 172.31.16.0/20  (ans:`172.31.16.4` ~`172.31.31.254`)
    - VPC的IP range要比subnet要來得大(e.g. VPC`192.31.0.0/16`, subnet`192.31.0.0/20`)
    - IP設得太大或太小，系統都會溫柔地提醒你XD
    - AWS VPC提供`/16`~`/28`的範圍
  - TCP/IP(v6)
    - TCP/IP(v6) is 128 bit
    - VPC: fixed at `/56`
    - subnet: fixed at `/64`
    - all TCP/IP(v6) addresses is public (Elastic IPv6 not supported, IPv4 only)
     
- connecting to a VPC
  - 可直接被外面連接：vpc <->interet gateway 
  - virtual private network: vpc <-> virtual gateway <-> vpn connection(dual tunnel) <-> customer gateway(通常架在使用者端)
- route tables(main route table && custom route table)
  - 當 create a VPC, route table會自動產生(both)
  - 需要有 （1）internet gateway（2）public IP address（3）`custom route`,`internet gateway`,`subnet`要有通 才可連接
  - main route table
    - allow local traffic within VPC
    - explicitly associated to all subnet
  - custom route table
    - allow local traffic within VPC
    - route to internet gateway 
    - explicitly associated to the public subnet
    - 通常也會在這裡指向全網域 e.g. 0.0.0.0/0
- public and private subnet
  - public subnet 放custom route table
  - private subnet 放 main route table
- network address translation(NAT)
  - 可以直接起一個NAT instance(EC2), 也可以做一個NAT gateway
  - 做為一個互聯網代理 （可連到外部網路0.0.0.0/0）
- VPC security 
  - security group
  - network access control list(ACLs)
  - flow logs (cloudwatch)
  - security group v.s. ACLs
    - <a href="https://ppt.cc/f0Msdx"><img src="https://ppt.cc/f0Msdx@.png" border="0" alt="PPT.cc縮圖服務" title="PPT.cc縮圖服務"></a>
    - https://medium.com/awesome-cloud/aws-difference-between-security-groups-and-network-acls-adc632ea29ae#:~:text=Security%20groups%20are%20tied%20to,will%20follow%20rules%20of%20NACL.&text=This%20means%20any%20instances%20within%20the%20subnet%20group%20gets%20the%20rule%20applied
  - stateful && stateless
    - stateful : 輸入會有明確回應（允許的狀態或拒絕）
    - stateless: 輸入不必有明確回應，一定是同意的才會輸出
### [Hand on] - VPC Networking
  create a wordpress ec2 and setting VPC
### AWS WAF, AWS Shield & AWS Firewall Manager
- Distributed deniel of service (DDoS)
  - DDoS attack classification
    - infrastructure layer attacks(基礎設施層攻擊)
    - application layer attacks（應用層攻擊）
  - protection techniques
    - reduce attack surface area (設置security group and ACLs)
    - plan and scale (暫時性增加實體層)
    - 了解正常流量及非正常流量
    - 使用WAF(web application firewall)
- AWS shield
  - AWS shield standard(free)
    - 網路活動監控, DDoS protection, CloudFront and Route 53 availability protection
  - AWS shield advanced
    - 更多的保護：UDP reflection, SYN flood, DNS query flood and HTTP flood
    - AWS DDoS Response Team(DRT) 搜集response的資訊，統計
    - cost protection (for DDos scaling charges)
- WAF(web application firewall)
  - 不只限制ip, 還可以限制request headers, request body, URI strings ...等, 可自由調整
  - deploy on: cloudfront, ec2, api gateway and app load balancer
- AWS firewall manager
  - 中央安全管理服務
  - 管理跨帳號及跨服務的AWS rules
  - 快速針對性地應對威脅
- panetration test(PT, 滲透測試)
  - 用來確認使用者的服務沒有明顯漏洞
  - 要由root account 向AWS申請測試（通常2-3個工作天會回覆）
  - 測試來源(IP 或 DNS 位置); 目的 (EC2、ELB、CloudFront … etc)

### CloudFormation
- 一個以設定檔來整合完整服務的東西（像docker-compose, 是yaml or json file）, 有版控
- template structure
  - json or yaml
- template sections(設定檔中的元素)
  - <a href="https://ppt.cc/fTnLgx"><img src="https://ppt.cc/fTnLgx@.png" border="0" alt="PPT.cc縮圖服務" title="PPT.cc縮圖服務"></a>
  - resources一定要有
- stacksets
  - 可以跨region跟跨帳號建立stack（大型企業可能需要）
- cloudformer
  - create a template從已經存在的AWS resource
- cloudformation designer
  - 視覺畫的UI, 拖拉圖像, 生成yaml or json file
  - https://ap-northeast-2.console.aws.amazon.com/cloudformation/designer/

### Lab Session -CloudFormation
- 不要輕易刪除cloudformation, 會刪除所有設定（有點像k8s deployment.yaml的感覺）

## week4

### Simple Workflow Service (SWF) - Core Knowledge(狀態追蹤器和任務協調器)
  - 協調跨分散式應用元件的工作
  - 適合複雜的業務流程及應用程式流程
  - 適合 long-running execution, 不適合batch processes
  - 可以跨平台工作（ＡＷＳ and 你的實體機器）
  - SWF features:
    - task 不會重複執行
    - routing and queueing of tasks
    - timeout and execution status
    - workflows can have child workflows
    - user data input and execution results output
  - SWF components:
    - workflow: 控制執行任務的流程邏輯
    - domain: contains a workflow and workflows
    - tasks: 可以執行可執行程式碼來執行,它們可以是Web服務呼叫或他們甚至可以是一個終端用戶輸入,他們可以並行或serially執行所以它們形成了工作流程的基礎
    - actors: 會直接與SWF互動,以協調工作
      - starters: initiate execution of a workflow(啟動job的)
      - deciders: implement workflow logic(控制下一步要幹嘛，決定是成功還是失敗)
      - Activity Workers: 執行Task
  - SWF tasks:
    - queue, queues
    - 可以指定worker (需要有 task routing)
  - SWF implementation:
    - SDKs
    - SWF API HTTP POST calls
    - flow framework(JAVA or Ruby)
    - can use console or CLI
  - mech note:
    - workflow executions
    - 提供Task-Oriented API
      - SQS 是Message-Oriented API
    - 可以存放到1 year
    - 會確保assigned task不會重複執行
  
### AWS Big Data Solutions
- go throught 近幾年來AWS提供的big data service
  - big data services
    - Database/storage
      - redshift, s3, RDS, DynamoDB
    - analysis
      - EMR, elasticsearch, quicksight bi, lambda, amazon machine learning
    - real time data
      - kinesis streams
    - third party applications
  - redshift(儲存服務)
    - petabyte scale
    - based upon postgreSQL
    - 提供BI(智能)報告服務
    - 複製所有node跟cluster之間的資料
    - 持續備份到s3 with snapshots(held for 1-35 days)
    - 可快速還原（有snapshots）
    
  - elastic map reduce(EMR)
    - AWS fully managed hadoop service
    - 任務結束後會自動停止cluster, 並刪除
    - 不適合用於小數據集, 或 transcation requirment
  
  - elasticsearch
    - analyze data from: s3, kinesis stream, cloudwatch log, cloudtrail api call logs
    - suitable: query and search large amounts of data 
    - not suitable: online transaction processing, petabyte storage
    
  - quicksight(BI reporting service)
    
  - machine learning
    - data source: s3, RDS, redshift
    - suitable: 分析個人行為, 預測個人行為, 預測產品需求, 標記可疑transaction
    - not suitable: learning skill, 超大的data set
  - kinesis(預測性分析)
    - suitable: real-time analyze
    - not suitable: long-term data storage and analytics

### API Gateway
  - serverless
  - a `front door` for get data, business logic ...

### AWS Serverless Platform
- serverless:
  - 不用煩惱server的相關問題，e.g. load blance, 存在哪, 機台..(都給AWS管)
  - 只要管你的預算限制即可

### Preparing for the AWS Certified Cloud Practitioner Exam
- cloud concepts
  - six advantages:
    - trade capital expense for variable expense
    - benefit from massive economies of scale
    - stop guessing capacity 
    - increase speed and agility
    - stop maintaining and spend money running data centers
    - go global in minutes 
  - TCO(Total cost of ownership)
  - pricing calculator
  - design principes
    - design for failure(multi-AZ, multi-region)
    - elasticity(autoscaling)
    - loose coupling(SQS)
- security and compliance
  - AWS: security of the cloud
  - customers: security in the cloud (e.g. encryption your data...)
  - VPC, VPN, AWS shield, WAF, cloudfront
  - IAM, grant least privilege access
  - amazon inspector, AWS trusted advisor, AWS report abuse
- technology
  - global v.s regional services(s3,cloudfront, route53)
  - ec2, s3, ELB, cloudwatch
  - database: relational v.s. NoSQL, serverless
- billing and pricing
  - AWS organisations
    - multiple accounts
    - consolidated billing
    - access management


## 後記
graph QL : 是一種為API設計的資料查詢(修改)的語言 (https://ithelp.ithome.com.tw/articles/10200678)

cli: https://aws.amazon.com/cli/


https://aws.amazon.com/tw/s3/faqs/
https://docs.aws.amazon.com/zh_tw/AWSEC2/latest/UserGuide/placement-groups.html

## note

### AWS cloudTrail & AWS cloudWatch


## ref
https://awslc.medium.com/aws-solutions-architect-associate%E8%AD%89%E7%85%A7%E6%BA%96%E5%82%99-saa-c01-%E8%88%87-saa-c02-ee4cd66ca2b0
