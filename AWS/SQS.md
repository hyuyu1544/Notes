# SQS

## Overview
- benefits:
    - no need hardware maintenance
    - little configuration
    - high message durability
- provide message ordering (FIFO)
- guarantee delivery of messages at least once

| SQS           | SNS           | AWS MQ            |Kinesis Streams|
|---------------|---------------|---------------|---------------|
|A SQS Standard Queue|A SNS Topic|A managed message broker service|A Kinesis Data Stream|
|託管佇列，可以解偶發送端跟接收端|發送通知給多個訂閱者|主要用在遷移本地數據到AWS|處理或分析串流資料|
|可以用在新的服務|可以用在新的服務|舊有服務存在時，使用它|可以用在新的服務|
| 1.a message queue service  | 1.send time-critical messages to multiple subscribers through a “push” mechanism | 1.If you're using messaging with `existing` applications, and want to move your messaging to the cloud quickly and easily   |message可以到多個應用程式並行處理（一個queue可以對應到多個consumer）|
| 2.exchange messages through a polling model | 2.don't need to periodically check or “poll” for updates       |    ||
|3.decouple sending and receiving components||||


## Billing
- pay only for what you use,no minimum fee
- `per request` and data transfer charges for `data transferred out of SQS`(same region ec2 and lembda除外)
- Free Tier：
    - 1 million requests per month
    - （數據傳輸費還是會收）
- batch operations (批次處理,SendMessageBatch , DeleteMessageBatch , and ChangeMessageVisibilityBatch)可以減少成本
- track and manage the SQS costs: 
    - tag and track your queues for resource
    - A tag is a metadata label comprised of a key-value pair
    - 對queue最分類，依分類來追蹤成本
    - 價格不含稅，日本帳單的要加繳日本消費稅

## Features, functionality and interfaces
- SQS可以結合AWS的運算服務、儲存數據庫一同使用
- 可使用console跟web api來跟SQS interact
- Only an AWS `account owner` can perform operations on an Amazon SQS message queue
- All messages have a global unique ID
- dead letter queue:
    - 要另開一個queue來處理dead letter
    - 當message無法被處理而且到達到最大嘗試處理數後，message被丟到這個queue
    - isolate messages that can't be processed for later analysis
- `visibility timeout`: a period of time during which Amazon SQS prevents other consuming components from receiving and processing a message
- SQS support message metadata
    - can contain up to 10 metadata attributes
    - attributes form: `name-type-value`
    - supported types: `string`, `binary`, and `number`
- `time-in-queue` value: message 到queue中時請求`SentTimestamp`, 再用當前時間去減掉這個值
- typical latency: SendMessage, ReceiveMessage, and DeleteMessage API requests 的延遲約幾十毫秒或幾百毫秒
- 當AWS賬戶ID不可用時（例如，當匿名用戶發送消息時），Amazon SQS提供IP地址在`SenderId` attribute
- SQS `long polling` and `short polling`
    - long polling: `doesn't` return a response until `a message arrives in the message queue`, or the long poll `times out` (可以省成本，減少空接收的數量)
        - times out預設20秒（最低可以設置1秒）
        - 要省成本times out可以設長一點
    - short polling: returns immediately
- `AmazonSQSBufferedAsyncClient` for Java:
    - 自動批處理多個SendMessage，DeleteMessage或ChangeMessageVisibility請求，而無需對應用程序進行任何必需的更改
    - 將消息預取到本地緩衝區中，該緩衝區允許您的應用程序立即處理來自Amazon SQS的消息，而無需等待消息被檢索
    - 自動批處理和預取協同工作可以提高吞吐量，減少應用程序的延遲，同時通過減少Amazon SQS請求來降低成本
    - 用於Java的AmazonSQSBufferedAsyncClient被實現為現有AmazonSQSAsyncClient的替代產品
- subscribe Amazon SQS message queues to receive notifications from Amazon SNS topics:
    - 1. In the Amazon SQS console, select an Amazon SQS standard queue.
    - 2. Under Queue Actions, select Subscribe Queue to SNS Topic from the drop-down list.
    - 3. In the dialog box, select the topic from the Choose a Topic drop-down list, and click Subscribe.
- 可以使用`PurgeQueue`來刪除messages而不會刪除queue


## FIFO queues
- 使用時機：事件順序很重要時
- available in all AWS regions
- 創建queue時就必須選好type(standard or FIFO), 之後不能更改
- FIFO queues are designed to never introduce duplicate messages
    - message可能重複的情境：
        - the producer sends a message, does not receive a response, and then resends the same message
    - SQS API提供重複數據刪除功能(removed within a 5-minute deduplication interval)
    - （standard queues可能會收到重複的message）
- when sending messages, you must specify a message group ID（跟standard的差異）
- 與FIFO不兼容（compatible）的服務：
    - Auto Scaling Lifecycle Hooks
    - AWS IoT Rule Actions
    - AWS Lambda Dead Letter Queues
    - Amazon SQS Buffered Asynchronous Client
- 支持的CloudWatch metrics：
    - `roximateNumberOfMessagesDelayed`:隊列中已延遲但無法立即讀取的消息數
    - `roximateNumberOfMessagesVisible`:可從隊列中檢索的消息數
    - `roximateNumberOfMessagesNotVisible`:正在運行的消息數（發送給客戶端，但尚未刪除或尚未到達其可見性窗口的末尾）
- `message groups`:
    - Messages are grouped into distinct, ordered "bundles" within a FIFO queue
    - 有group ID的，會以嚴格的順序來發送及接收（沒有則否）
    - 如果將具有相同消息組ID的多個主機（或同一主機上的不同線程）發送的消息發送到FIFO隊列，則Amazon SQS將按它們`到達進行處理的順序`傳遞消息。為確保Amazon SQS保留發送和接收消息的順序，請確保多個發件人發送具有唯一消息組ID的每條消息
- SQS support multiple producers (以接收成功的順序來排序)
- 要用group ID來送資料給多個consumer
- must use a FIFO dead letter queue with a FIFO queue(standard dead letter queue with a standard queue)
- throughput（吞吐量） quota for FIFO queue:
    - 3,000 messages per second with batching
    - 300 messages per second without batching
    - high throughput mode： 30000/sec（not every region）
- FIFO queue must end with the `.fifo` suffix

## Security and Reliability
- Reliability:
    - 會同時存在`a single, highly-available AWS region`跟`multiple redundant Availability Zones (AZs)`
    - 不怕單一機器、網路、AZ壞掉
- Security:
    - Authentication mechanisms(身份驗證機制)
    - 加密message
    - supports the HTTP over SSL (HTTPS) and Transport Layer Security (TLS) protocols （in all region）
- separate `ReceiveMessage` and `DeleteMessage` operations

## Server-Side encryption(SSE)
- 使用`AWS KMS`中的密鑰加密message
- not all region, please check on the document
- 為SQS啟用SSE:
    - customer master key (CMK) ID (alias `ARN`)
    - `KmsMasterKeyId`
- Both standard and FIFO queues support SSE
- calls from Amazon SQS to AWS KMS要收費
- `AES-GCM 256`
- 將SSE與Amazon SQS結合使用需要什麼權限?
    - 收發皆需搭配customer master key (CMK)
    - producer -> kms:GenerateDataKey and kms:Decrypt permissions for the CMK
    - consumer ->  kms:Decrypt permission for any CMK
- SSE只對`消息正文`進行加密（metadata不加密）
- SSE是否限制了每秒的事務數（TPS）或可以使用Amazon SQS創建的隊列數?
    - a. The data key reuse period (1 minute to 24 hours)
    - b. AWS KMS每個賬戶的配額（默認為100 TPS）
    - c. 訪問隊列的IAM用戶或帳戶的數量
    - d. 存在大量積壓(backlog)（較大的積壓需要更多的AWS KMS調用）
    - (a*b)/c=d
- 如何估算我的AWS KMS使用成本？
    - B是計費周期（以秒為單位）
    - D是數據密鑰重用周期（以秒為單位）
    - P是發送到Amazon SQS隊列的生產主體的數量
    - C是從Amazon SQS隊列接收的使用主體的數量
    - R: number of API requests per queue
    - R = B / D *（2 * P + C）
    - (In general, producing principals incur double the cost of consuming principals)

## Compliance
- PCI DSS Level 1 certified
- HIPAA Eligible

## Limits and restrictions
- message可在queue中保留時間
    - 1min~14days(預設4 days)
    - 用`MessageRetentionPeriod`來設置保存時間(單位用秒)
- message可在queue中的maximum size
    - 1KB ~ 256KB
    - `MaximumMessageSize`
    - 大於256KB要參考`SQS Extended Client Library for Java`, 可以到2GB
- message type in queue
    -  text data, including XML, JSON and unformatted text
- SQS message queues 可以容納多少 message
    - standard queue: 120,000
    - FIFO queue: 20,000
- message queues 的數量無限制
- queue命名的限制
    - limited to 80 characters
    - 可用英文字母、數字、`_`、`-`
    - 名字在同帳號及同region內必須唯一

## Queue sharing
- API settings of queue sharing:
    - AddPermission
    - RemovePermission
    - SetQueueAttributes
    - GetQueueAttributes
- queue owner要負擔共享相關費用
- 使用完整URL來共享queue(`CreateQueue`和`ListQueues`可以看到full URL)
- 可以匿名訪問queue



## Service access and regions
- SQS message queue is `independent` within each region(不可再不同region間共享)
- 中國（北京）的定價與其他region不同
- 同region的數據傳輸免費（`SQS`<->`EC2`<->`lambda`）



## Reference
https://aws.amazon.com/sqs/faqs/
https://medium.com/@kumargaurav1247/sqs-vs-sns-vs-kinesis-difference-63b1aca143f4
