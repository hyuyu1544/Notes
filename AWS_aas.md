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

- RDP connect to ec2 (window instances) (rdp port用`3389`)
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
- Mount target(3:10)

補充:
  - file: EFS
  - obj: S3, Glacier
  - block: EBS, Instance store


## [Hands On] - Elastic File Service (EFS)




## Simple Storage Service (S3)



## 後記
graph QL : 是一種為API設計的資料查詢(修改)的語言 (https://ithelp.ithome.com.tw/articles/10200678)

cli: https://aws.amazon.com/cli/