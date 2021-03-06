# VPC

## General Questions
- 佈建一個在邏輯上隔離的部分，以在自己定義的虛擬網路中啟動 AWS 資源
- VCP的組成：
    - `A Virtual Private Cloud`: A logically isolated virtual network in the AWS cloud
    - `Subnet`: A segment of a VPC’s IP address range where you can place groups of isolated resources
    - `Internet Gateway`: The Amazon VPC side of a connection to the public Internet
    - `NAT Gateway`: A highly available, managed Network Address Translation (NAT) service for your resources in a private subnet to access the Internet
    - `Virtual private gateway`: The Amazon VPC side of a VPN connection
    - `Peering Connection(對等連接)`: A peering connection enables you to route traffic via private IP addresses between two peered VPCs
    - `VPC Endpoints`: Enables private connectivity to services hosted in AWS, from within your VPC without using an Internet Gateway, VPN, Network Address Translation (NAT) devices, or firewall proxies
    - `Egress-only Internet Gateway`: A stateful gateway to provide egress only access for IPv6 traffic from the VPC to the Internet.
<a href="https://ppt.cc/fnGCDx"><img src="https://ppt.cc/fnGCDx@.png" border="0" alt="PPT.cc縮圖服務" title="PPT.cc縮圖服務"></a>
- 無需VPN，硬件或物理數據中心，即可建立自己的虛擬網路
- 可在VPC中，控製網絡內部的Amazon EC2資源如何暴露給Internet
- four basic options for network architectures:
    - Amazon VPC with a single public subnet only
    - Amazon VPC with public and private subnets
    - Amazon VPC with public and private subnets and AWS Site-to-Site VPN access
    - Amazon VPC with a private subnet only and AWS Site-to-Site VPN access
- What are the different types of VPC endpoints available on Amazon VPC?
    - 端點是水平可擴展且高度可用的虛擬設備，可在VPC中的實例與AWS服務之間進行通信
    - 通過VPC端點，您可以將VPC專用連接到AWS上託管的服務，而無需Internet網關，NAT設備，VPN或防火牆代理
    - VPC offers two different types of endpoints:
        - Gateway type endpoints:
            - only available for AWS services including S3 and DynamoDB
            - add an entry to your route table
        - Interface type endpoints:
            - private connectivity to services powered by PrivateLink, being AWS services, your own services or SaaS solutions, and supports connectivity over Direct Connect

## Billing
- 創建和使用VPC本身不收取額外費用
- 以hardware VPN連接將VPC連接到公司數據中心，按小時計費（"available" state）
- via your VPC's Internet gateway：不收費
- via your VPN connection：收費
- 不含稅

## Connectivity(連接性)
- connectivity options:

|      | VPC-VPC                 | VPC-Other AWS services                                           | VPC-The internet  | VPC-corporate data center  |
|------|-------------------------|------------------------------------------------------------------|-------------------|----------------------------|
| 連接方式 | VPC peering connections | internet gateway, NAT, virtual private gateway, or VPC endpoints |  internet gateway | virtual private gateway, Site-to-Site VPN connection    |
- An Internet gateway is horizontally-scaled, redundant, and highly available
- An Internet gateway imposes no bandwidth constraints
- How do instances in a VPC access the Internet?
    - public IP addresses:
        - including Elastic IP addresses (EIPs)
        - directly communicate outbound to the Internet
        - to receive unsolicited inbound traffic from the Internet (eg, web servers)
    - Instances without public IP addresses:
        - through a NAT gateway or a NAT instance to access the Internet
        - 對於具有硬件VPN連接或直接連接連接的VPC，實例可以將其Internet通信沿著虛擬專用網關路由到您現有的數據中心。從那裡，它可以通過您現有的出口點和網絡安全/監視設備訪問Internet
- 您可以使用第三方軟件VPN通過Internet網關與VPC創建站點到站點或遠程訪問VPN連接
- 當兩個實例使用公用IP地址進行通信時，流量是否會通過Internet傳輸？
    - 兩個EC2實例之間或EC2實例與同一AWS區域中的任何AWS區域端點之間的流量即使在公共IP地址上通過，也仍停留在AWS網絡內
    - 如果兩個實例所在的VPC之間存在區域間VPC對等連接，則不同AWS區域中EC2實例之間的流量將保持在AWS網絡內
    - 不能保證這些實例所在的VPC之間沒有區域間VPC對等連接的不同AWS區域中的EC2實例之間的流量不會留在AWS網絡中
- AWS Site-to-Site VPN連接如何與Amazon VPC一起使用？
    - 一個AWS Site-to-Site VPN連接將您的VPC連接到您的數據中心。Amazon支持Internet協議安全性（IPSec）VPN連接。通過加密的VPN連接在VPC和數據中心路由之間傳輸的數據有助於維護傳輸數據的機密性和完整性。建立AWS站點到站點VPN連接不需要Internet網關

## IP Addressing
- IP address ranges:
    - any IPv4 address range for `primary` CIDR block
    - For the `secondary` CIDR block，某些限制適用
    - 公共可路由IP塊只能通過Virtual Private Gateway，而不能over the Internet through the Internet gateway
    - 可以通過調用相關API或通過AWS管理控制台將Amazon提供的IPv6 CIDR塊分配給VPC
- Default VPCs are assigned a CIDR range of 172.31.0.0/16
- Default子網分配了/ 20個網絡塊(netblocks)
- can route traffic via the AWS Site-to-Site VPN connection, 並從您的家庭網絡公佈地址範圍
- Can use public IPv4 addresses in VPC and access them over the Internet
- How large of a VPC can I create?
    - supports 5 IP address ranges
    - 1 primary IPv4
    - 4 secondary IPv4
    - between /16 ~ /28 之間(IPv4)
    - For IPv6, the VPC is a fixed size of /56(不能更改)
    - can have both IPv4 and IPv6
- 每個VPC可以創建200個子網
- subnet size limit
    - IPv4:
        - minimum size: /28
        - maximun size: 不能大於VPC
    - IPv6:
        - fixed to be a /64
        - Only one IPv6 CIDR block can be allocated to a subnet
- Amazon保留每個子網的前四（4）個IP地址和後一（1）個IP地址用於IP網絡
- 在VPC中啟動Amazon EC2實例時，可以選擇指定實例的主要私有IP地址
- 在實例或接口的生命週期內保留主專用IP地址。可以隨時在接口或實例之間分配，取消分配或移動輔助專用IP地址
- 如果Amazon EC2實例在VPC內停止，我是否可以在同一VPC中啟動具有相同IP地址的另一個實例？
    - NO (`終止`狀態不行，`運行`和`停止`就可以保留ip)
- Can I assign IP addresses for multiple instances `simultaneously`?
    - no (通常是啟用instance時就指定了)
- can assign any IP address to your instance：
    - Part of the associated subnet's IP address range
    - Not reserved by Amazon for IP networking purposes
    - Not currently assigned to another interface
- Can I assign multiple IP addresses to an instance?
    - yes.(您可以分配的輔助專用IP地址的數量取決於實例類型)
- Can I assign one or more Elastic IP (EIP) addresses to VPC-based Amazon EC2 instances?
    - 是的，但是，只能從Internet上訪問EIP地址（不能通過VPN連接）。每個EIP地址必須與實例上的唯一私有IP地址關聯。EIP地址僅應在配置為將其流量直接路由到Internet網關的子網中的實例上使用。EIP不能在配置為使用NAT網關或NAT實例訪問Internet的子網中的實例上使用。這僅適用於IPv4。Amazon VPC目前不支持用於IPv6的EIP

## Topology
- You may create a default route for each subnet
- The default route can direct traffic to egress the VPC via the Internet gateway, the virtual private gateway, or the NAT gateway

## Security and Filtering
- Amazon EC2 security groups can be used to help secure instances within an Amazon VPC
    - 通過VPC中的安全組，您可以指定允許或禁止來自每個Amazon EC2實例的入站和出站網絡流量。
    - 自動拒絕未明確允許進出實例的流量。
    - 除了安全組，還可以通過網絡訪問控制列表（ACL）允許或拒絕進入和退出每個子網的網絡流量
- VPC v.s. ACL
    - VPC 中的安全群組指定獲允許進出 Amazon EC2 執行個體的流量(允許;狀態篩選)
    - 網路 ACL 會在子網路層級上運作，並評估進出子網路的流量(允許和拒絕;無狀態篩選)
- 狀態篩選 v.s. 無狀態篩選
    - 狀態篩選:
        - 會追蹤請求的來源
        - allow the return traffic
    - 無狀態篩選:
        - 只會檢查來源或目的地 IP 地址和目的地連接埠
        - ignoring whether the traffic is a new request or a reply to a request
- 是否可在 Amazon VPC 中使用為 Amazon EC2 中的執行個體所建立的 SSH 金鑰對，或是在 Amazon EC2 中使用為 Amazon VPC 中的執行個體所建立的 SSH 金鑰對?
    - yes
- Can Amazon EC2 instances within a VPC communicate with Amazon EC2 instances `not` within a VPC?
    - yes
    - Internet gateway or VPN
- Can Amazon EC2 instances within a VPC in one region communicate with Amazon EC2 instances within a VPC in `another` region?
    - ???
    - yes, instances in `one region` can communicate with each other using `Inter-Region` VPC Peering, public IP addresses, NAT gateway, NAT instances, VPN Connections or Direct Connect connections.
- Can Amazon EC2 instances within a VPC communicte with Amazon S3?
    - VPC endpoint (all traffic remains within Amazon's network)
    - Internet gateway
    - Direct Connect or VPN connection
- Can I monitor the network traffic in my VPC?
    - yes, `VPC traffic mirroring` and `VPC flow logs features`

## VPC Traffic Mirroring
- What is Amazon VPC traffic mirroring?
    - 複制往返於Amazon EC2實例的網絡流量，並將其轉發到帶外安全和監視設備
    - These appliances can be deployed on an individual EC2 instance or a fleet of instances behind a Network Load Balancer (NLB) with User Datagram Protocol (UDP) listener.
- How does Amazon VPC traffic mirroring work?
    - 1.`copies network traffic from Elastic Network Interface (ENI) of EC2 instances`
    - 2.`mirrored traffic can be sent to another EC2 instance or to an NLB with a UDP listener`
    - 3.`Traffic mirroring encapsulates all copied traffic with VXLAN headers`
    - The mirror source and destination (monitoring appliances) can be in the same VPC or in a different VPC, connected via VPC peering or AWS Transit Gateway
- Amazon VPC流量鏡像可以監視哪些資源？
    - supports `network packet` captures at the `Elastic Network Interface (ENI) level` for EC2 instances
    - supported on all `virtualized Nitro based` EC2 instances
- What type of appliances are supported with Amazon VPC traffic mirroring?
    - 幾乎任何network packet collector/broker or analytics tool都可以用
- `Amazon VPC traffic mirroring` v.s. `Amazon VPC flow logs`
    - `VPC traffic mirroring`: actual network traffic content
    - `VPC flow logs`: 全部都收

## Amazon VPC and EC2
- 同個 VPC 不能跨region
- VPC 可以跨AZ
- subnet不能跨AZ
- 啟動EC2 instance時，可以先指定subnet （就會在subnet下的AZ launch）
- 創建subnet時，就要先指定AZ(不指定就在region隨便選一個)
- ec2 instance 在不同ＡＺ間的數據傳輸費，$0.01 per GB
- `DescribeInstances（）`可列出所有正在運行的實例
- `DescribeVolumes（）`return all your EBS volumes
- 一個VPC,可以有20個實例（超過要申請）
- 可以使用現有的AMI（in same region）
- 可以使用現有的Amazon EBS snapshots（in same region）
- an instance launched in a VPC using an Amazon `EBS-backed AMI` maintains the same IP address when stopped and restarted
- Can I use Amazon EC2 Reserved Instances with Amazon VPC?
    - yes
- 可以在Amazon VPC中使用Amazon CloudWatch
- 可以在Amazon VPC中採用Auto Scaling
- Can I launch Amazon EC2 Cluster Instances in a VPC?
    - yes

## Default VPCs
- When you launch an instance `without` specifying a subnet-ID, your instance will be launched in your default VPC
- benefits: `security group egress filtering`, `multiple IP addresses`...
- account create after March 18, 2013, 就會有默認VPC（EC2-Classic and EC2-VPC）如果沒有，官方建議重新創一個帳號
- Do I need to have a VPN connection to use a default VPC?
    - no
- 除了默認VPC之外，還可以創建其他VPC並使用
- 可以在默認VPC中創建其他子網，private subnet
- 每個region都可以有一個Default VPC
- How many default subnets are in a default VPC?
    - 1
- 我可以指定哪個VPC是我的默認VPC嗎？
    - 目前不行
- 我可以指定哪些子網是我的默認子網嗎？
    - 目前不行
- 可以刪除默認的VPC (刪除後可重新創建)
- 可以刪除默認子網 (刪除後可重新創建)
- How are IAM accounts impacted by default VPC?
    - If your AWS account has a default VPC, any IAM accounts associated with your AWS account use the same default VPC as your AWS account.

## Elastic Network Interfaces
- Can I `attach` or `detach` one or more network interfaces to an EC2 instance while it’s running?
    - yes
- Can I have `more than two` network interfaces attached to my EC2 instance?
    - depends on the instance type
- Network interfaces can only be attached to instances residing in the same Availability Zone
- Network interfaces can only be attached to instances in the same VPC as the interface
- You can `attach and detach` secondary interfaces (eth1-ethn) on an EC2 instance, but you `can’t detach` the eth0 interface
- 我是否需要為與網絡接口關聯但網絡接口未附加到正在運行的實例的彈性IP地址付費？
    - yes

## Peering Connections
- Peering connections can be created with VPCs in different regions
- 不同帳號下的VPC也可對等連接
- 創建對等連接不用付費，數據傳輸要付費
- VPC peering connections `do not` require an Internet Gateway
- Is VPC peering traffic within the region encrypted?
    - no
- 如果我將VPC A與VPC B對等，並且將VPC B與VPC C對等，這是否意味著VPC A和C對等？
    - no
- 跨區域VPC(Inter-Region VPC)對等連接不能引用安全組
- 區域間VPC對等支持IPv6
- 跨區域VPC對等不能與EC2-ClassicLink一起使用
- Is Inter-Region VPC Peering traffic encrypted?
    - Traffic is encrypted using modern AEAD (Authenticated Encryption with Associated Data) algorithms.

## ClassicLink
- Amazon Virtual Private Cloud（VPC）ClassicLink允許EC2-Classic平台中的EC2實例使用私有IP地址與VPC中的實例進行通信
- 有跨可用區數據傳輸費用
- 如何使用ClassicLink：
    - EC2-Classic實例鏈接到VPC，並且是VPC中所選安全組的成員
- The EC2-Classic instance does not become a member of the VPC
- Can I use EC2 public DNS hostnames from my EC2-Classic and EC2-VPC instances to address each other, in order to communicate using private IP?
    - no
- 是否有不能為其啟用ClassicLink的VPC？
    - 是的。對於具有在10.0.0.0/8範圍內的無類域間路由（CIDR）的VPC，不能啟用ClassicLink，但10.0.0.0/16和10.1.0.0/16除外。此外，不能為具有指向10.0.0.0/8 CIDR空間的路由表條目指向除“本地”以外的目標的任何VPC啟用ClassicLink。
- 來自EC2-Classic實例的流量只能路由到VPC內的專用IP地址(不會通過Internet網關)
- 在EC2-Classic實例的停止/啟動週期中，ClassicLink連接將不會持續

## AWS PrivateLink
- AWS PrivateLink使客戶能夠以高度可用和可擴展的方式訪問AWS上託管的服務
- 將所有網絡流量保持在AWS網絡內
- AWS PrivateLink當前提供哪些服務？
    - EC2, ELB, Kinesis Streams, Service Catalog, EC2 Systems Manager, Amazon SNS, and AWS DataSync
- Can I privately access services powered by AWS PrivateLink over AWS Direct Connect?
    - yes
- Currently, `no` CloudWatch `metric` is available for the interface-based VPC endpoint.

## Bring Your Own IP
- What is the Bring Your Own IP feature?
    - 自帶IP（BYOIP）使客戶可以將其現有的公共可路由的全部IPv4或IPv6地址空間的全部或部分移至AWS，以與其AWS資源一起使用
- Why should I use BYOIP?
    - IP Reputation(信譽)：已經營有成
    - 客戶白名單
    - Hardcoded dependencies（硬編碼的依存關係）
    - Regulation and compliance（法規和合規性）
    - 本地IPv6網絡策略（On-prem IPv6 network policy）
- How many IP ranges can I bring via BYOIP?
    - 5 per account
- Which RIR prefixes can I use for BYOIP?
    - You can use ARIN, RIPE, and APNIC registered prefixes.
- Can I move a BYOIP prefix from one AWS Region to another?
    - yes


## Additional Questions
- You can use the AWS Management Console to manage Amazon VPC objects such as VPCs, subnets, route tables, Internet gateways, and IPSec VPN connections
- How many VPCs, subnets, Elastic IP addresses, and internet gateways can I create?
    - 5 Amazon VPCs per AWS account / region
    - 2 hundred subnets / Amazon VPC
    - 5 Amazon VPC Elastic IP addresses / AWS account per region
    - 1 internet gateway / Amazon VPC
- `ElasticFox` is no longer officially supported for managing your Amazon VPC(建議使用AWS API)





# Reference
https://aws.amazon.com/tw/vpc/faqs/

https://tutorialsdojo.com/amazon-vpc/
