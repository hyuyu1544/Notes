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
        - 
    - 無狀態篩選:
        - 只會檢查來源或目的地 IP 地址和目的地連接埠
        - 忽略流量是新請求還是對請求的回覆


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
