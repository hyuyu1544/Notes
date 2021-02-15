# ec2
 
## note by FQA
 
 - enables “compute” in the cloud
 - AMI (Amazon Machine Image):打包的環境
 - 最多購買20個預留實例，再多要透過申請
 - SMTP endpoint policy -> port 25
 - Service level agreement (SLA): Monthly Uptime Percentage of at least 99.99% EC2 and EBS within a Region.
 
### Instance Pricing Modules
 
##### EC2 On-Demand Instance:
  - five vCPU-based instance limits(region basis), 再多要透過申請
  - 以秒計費, 最低的計價需要60秒
  - 20 Reserved Instances per region
##### Reserved Instances:
  - 提供了大幅折扣(最高可達75%)，但需要先預付一筆金額購買 AWS 服務內的服務時數，價格依據購買的服務等級與時間會有不同的折扣(1 年期或 3 年期。可彈性變更自己預留執行個體的可用區域、執行個體大小及類型)
##### Spot Instances:
  - 以競標的方式標用服務，可以用比較低廉的價錢租用機器，但缺點是隨時可能會中斷，所以程式必須定時記錄進度，自動處理重跑的流程。當出價不足以標到該服務時，會先關起來 (Stop)，等到價錢低於出價後就會再打開機器，這時候的狀態就會恢復(必須是使用 EBS 的機器才有支援)。當你想用的時段越少人搶標，選用這樣的計價方式就越划算。
  - 會被打斷，不適合處理關鍵數據
##### Dedicated Hosts:
  - Dedicated Hosts 簡單來講就是租用實體主機，所以會根據執行個體系列、區域及付款選項而有不同。無論在特定專用主機上選擇啟動的執行個體數目或大小為何，都只需支付每個作用中專用主機每小時的費用，不會針對執行個體的使用時間計費。但好處是上面可以安裝你指定的軟體與套件。
  - 而 Dedicated Hosts 也可以搭配 Reserved Instances 購買並預付較長期的合約換取更佳的價格折扣。
  
### Instance Types
| general-purpose              | compute-optimized | memory-optimized          | accelerated-computing            | storage-optimized |
|------------------------------|-------------------|---------------------------|----------------------------------|-------------------|
| 一般應用                         | 運算優化(CPU)         | 記憶體優化                     | 加速運算優化(GPU)                      | 儲存優化              |
| 網頁應用程式伺服器,程式碼儲存槽（repository） | 資料分析、影音處理         | in-memory資料庫、Apache Spark | 影音呈向（rendering）、machine learning | 資料分析或分散式資料庫       |
| m 系列、t系列 、a系列              | c 系列              | r系列、x系列                   | p系列、g系列、i系列、f系列                  | i系列、d系列、h系列       |



### Storage

##### EBS
 - 預設為`終止instance，自動刪除`代號為`N`
 - 透過設定讓他們的lifecycle獨立，讓EBS不會自動刪除
 - EBS可以在使用中時改變，type跟size都可以
 - EBS always be in the same availability zone as the EC2 instance
 - SSD(transactional workloads, 事務處理工作負載) and HDD(throughput intensive workloads, 吞吐量工作負載)
   - Throughput Optimized HDD (st1): 
     - frequently accessed, large datasets and large I/O sizes
     - e.g. MapReduce, Kafka, log processing, data warehouse, and ETL workloads
   - Cold HDD (sc1): 
     - lowest cost `per GB` of all EBS volume types
     - less frequently, large and cold datasets
   - IOPS SSD (io1 and io2):
   - General Purpose SSD (gp2 and gp3):
 - `IOPS io1 volume`，在同`AZ`內，可以共用16到`Nitro-based EC2 instances`
 - EBS snapshots:
   - only available through the Amazon EC2 APIs
   - 使用snapshots時,可能會移除本地緩存的數據,建議做法：`移除連接`->`snapshots`->`重新連接`
   - Snapshot可以encrypted，如果是從encrypted volume create/restore的snapshot，都會是encrypted
   - Snapshot可以share，但只有unencrypted的可以share
   - Snapshots備份到S3是Incrementally(增加上去,不會覆蓋)
##### EFS
 - 用DNS掛載到instance(所有類型ec2都可以使用, 可以同時有1000個instance連接到EFS)
 - 同個ＶＰＣ內可以直接訪問ＦＥＳ，不在ＶＰＣ內要使用`ClassicLink`來訪問
 - 在instance內的數據可以直接上傳EFS, 不在的可以透過Secure Copy (SCP)
##### NVMe Instance storage
 - I3en, I3, C6gd, C5d, C5ad, M6gd, M5d, M5dn, M5ad, R6gd, R5d, R5dn, R5ad, z1d, P3dn, G4dn, and F1, X1, X1e才有這個儲存方式
 - 都會有加密，在`AWS Nitro hardware module`以`XTS-AES-256`來加密
 - encryption keys is unique

### Networking and security
##### Elastic Fabric Adapter (EFA)

 
### Placement Groups
 - 要確保EC2 Instance之間的網路效率時，只放在同一個AZ是不足夠的：要在同一個Cluster Placement Group才是正確做法

##### Spread Placement Groups(降低故障風險)
 - 可降低實例共享同一機架時可能發生的同時發生故障的風險
 - 只有特定的Zone有支援，一個Zone只能放最多7台SPG Instance
 - 可以跨AZ部署
##### Cluster Placement Group(效能)
 - 將實例打包在一起放在可用區中。該策略使工作負載能夠實現HPC應用程序中典型的緊密耦合的節點到節點通信所需的低延遲網絡性能
 - 不能跨AZ部署
##### Partition Placement Group（大型分佈式）
 - 將您的實例分佈在邏輯分區上，這樣一個分區中的實例組就不會與不同分區中的實例組共享基礎硬件。大型分佈式和復制工作負載（例如Hadoop，Cassandra和Kafka）通常使用此策略
 - 可以跨AZ部署
  
# ref
- https://ithelp.ithome.com.tw/articles/10192098
- https://aws.amazon.com/ec2/faqs/
