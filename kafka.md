# kafka

## 10 Kafka ElasticSearch Consumer & Advanced Configurations

### set up elasticsearch in the cloud

在本地裝elasticsearch
```
brew install elasticsearch
```
or, 可以用一個線上支援來建立雲端上的elasticsearch
```
bonsai elasticsearch
```
### 簡易對elasticsearch下query
如果用的是bonsai: console
也可以用postman這些工具

下query的語法可以參考elasticsearch官方文件

查看資訊：`/_cat/health?v`
create new index: `put` `/<index_name>`
insert data: `put` `/<index_name>/<type>/<index_number> {<json_data>}`

p.s. 記得看response status
p.s. https://www.elastic.co/guide/en/elasticsearch/reference/6.8/indices-get-index.html

### setup proj: 塞 id into elasticsearch
`kafka-consumer-elasticsearch` insert twitter id into elasticsearch

### 74: 把整包data塞進elasticsearch

`kafka-basics` >> `demogroups`

### 75: delivery semantics(傳遞語義)

At-most-once: 只做一次，存進es時若失敗，將丟失數據，因為offset在插入數據前就移到下一個位置了

<a href="https://ppt.cc/fx2fYx"><img src="https://ppt.cc/fx2fYx@.png" border="0" alt="PPT.cc縮圖服務" title="PPT.cc縮圖服務"></a>

at-least-once（default）:At-least-once is when offsets are committed `after` the message is processed.(要確保data是冪等(idempotent))

<a href="https://ppt.cc/fDDB5x"><img src="https://ppt.cc/fDDB5x@.png" border="0" alt="PPT.cc縮圖服務" title="PPT.cc縮圖服務"></a>

exactly-once: only for Kafka to Kafka workflow using Kafka Streams.

### 76: 實作idempotent

什麼是idempotent： https://william-yeh.net/post/2020/03/idempotency-key-test/

讓同筆資料的id固定不變，如果id一樣就只是rewrite進資料庫，而不是又生成一筆，造成資料重複

### 77:Consumer Poll Behaviour

poll model: consumer `poll` data
可以設置`max.poll.records`，來提高throughput（越高須越多的memory）
`fetch.max.bytes`

<a href="https://ibb.co/mDszDzw"><img src="https://i.ibb.co/SsTvsv8/Screen-Shot-2021-03-02-at-5-14-37-AM.png" alt="Screen-Shot-2021-03-02-at-5-14-37-AM" border="0"></a>


push model:
server `pushes` data to the consumer and the consumer waits

### 78: Consumer Offset Commit Strategies
(1)enable.auto.commit = true & synchronous processing of batches (consumer直到處理完所有message,才poll;consumer也會有丟失message的風險，因為offset已經move on 了)

<a href="https://ppt.cc/fQodtx"><img src="https://ppt.cc/fQodtx@.png" border="0" alt="PPT.cc縮圖服務" title="PPT.cc縮圖服務"></a>

(2)enable.auto.commit = false & manually(手動) commit the offsets
```
properties.setProperty(ConsumerConfig.ENABLE_AUTO_COMMIT_CONFIG, "false"); // disable auto commit of offsets
properties.setProperty(ConsumerConfig.MAX_POLL_RECORDS_CONFIG, "100"); // max poll record at same time
```

<a href="https://ppt.cc/fj97jx"><img src="https://ppt.cc/fj97jx@.png" border="0" alt="PPT.cc縮圖服務" title="PPT.cc縮圖服務"></a>

用來確認consumer offset停的位置
```
kafka-consumer-groups --boostrap-server 127.0.0.1:9092 --group kafka-demo-elasticsearch --describe
```

### 80: Consumer Part 5 - Performance Improvement using Batching
用bulk就不用get one message pre request

### 81: Consumer Offsets Reset Behaviour
若consumer已經關閉超過七天(沒有committed offset跟read new data)，offset會失效
用`auto.offset.reset`來重新設置offset
`offset.retention.minutes`來設置offset的保留期

```
kafka-consumer-groups --boostrap-server 127.0.0.1:9092 --group kafka-demo-elasticsearch --reset-offsets --execute --to-earliest --topic <topic_name>
```

### 83: Consumer Internal Threads

兩個不同thread:
1. poll thread給broker:
    - [被自殺]在poll thread, 兩次poll間隔超過`max.poll.interval.ms`，consumer會被kill
2. heartbeat thread 給consumer coordinator確認consumer是否健康: 
    - [自然死亡]每10秒戳一下，沒反應就斷連接，並rebalance consumer


## 11 ===== Kafka Ecosystem & Real-World Architectures =====
預告接下來的課程會以案例來讓學員以更高層次的方式來了解卡夫卡如何將所有事物融合在一起
看一下卡夫卡生態系統是如何工作的
我們將了解有關Kafka Connect，Kafka Streams

## 12 Kafka Extended APIs for Developers


### kafka connect

concept: code and connectors re-use

歷史演進：
`0.8`:可以複製topic, 簡化producer API 及壓縮log
`0.9`:簡化consumer API, 不需要Zookeeper來存儲訪問權限, 增強了安全性及多了 Kafka Connect APIs
`0.10.0`:added Kafka Streams API
後來的幾次小改版，又新增了improved Kafka Connect API and a Single Message Transforms API

四個常用案例：
<a href="https://ppt.cc/fNDWGx"><img src="https://ppt.cc/fNDWGx@.png" border="0" alt="PPT.cc縮圖服務" title="PPT.cc縮圖服務"></a>
Kafka Connect Source:它大大簡化了將數據從來源導入Kafka。
對於從Kafka到Kafka，我們稍後會看到它是Kafka Streams
Kafka接收器，我們將使用Kafka Connect Sink


summary: 
因為大部分會進到kafka的資料來源大多很相似，資料出kafka後儲存的位置也很相近，所以大部分的工都有人做了（fault tolerance, idempotence, distribution, and ordering），就把這些工彙整成為Connect Cluster source API。
接著使用kafka stream對資料進行過濾、aggregate等，最後再把資料拉回sink中。
好處在於scales很棒，可以從小專案擴展到公司規模。
主要就是上code可以re-use。


<a href="https://ppt.cc/fasPJx"><img src="https://ppt.cc/fasPJx@.png" border="0" alt="PPT.cc縮圖服務" title="PPT.cc縮圖服務"></a>

hand-on:

google 
```kafka connectors confluent```
選這個人的（）
https://github.com/jcustenborder/kafka-connect-twitter

```
kafka-topics --zookeeper 127.0.0.1:2181 --create --topic twitter_status_connect --partitions 3 --relication-factor 1
```
```
kafka-topics --zookeeper 127.0.0.1:2181 --create --topic twitter_deletes_connect --partitions 3 --relication-factor 1
```
看有幾個topic
```
kafka-topics --zookeeper 127.0.0.1:2181 --list
```

```
kafka-console-consumer --bootstrap-server 127.0.0.1:9092 --topic twitter_status_connect --from-beginning 
```


### kafka streams

concept: filter messages or aggregate message

it is a library for data process or transformation

<a href="https://ppt.cc/fKqxIx"><img src="https://ppt.cc/fKqxIx@.png" border="0" alt="PPT.cc縮圖服務" title="PPT.cc縮圖服務"></a>

hand-on:

search : `maven kafka streams`

create a new topic:
```
kafka-topics --zookeeper 127.0.0.1:2181 --topic important_tweets --create --partitions 3 --replication-factor 1
```

run java code in IDE

run consumer in console:
```
kafka-console-consumer --bootstrap-server 127.0.0.1:9092 --topic important_tweets --from-beginning 
```

### Kafka Schema Registry Introduction

schema registry(架構註冊表)

kafka不會解析或查看您的數據, 因此不會佔用cpu

It just receives bytes and sends bytes

Kafka只是獲取數據，將其發送出去,甚至不知道數據格式

所以如果資料格式有誤怎麼辦？ 因此需要先定義 data的schema

<a href="https://ppt.cc/f7DyTx"><img src="https://ppt.cc/f7DyTx@.png" border="0" alt="PPT.cc縮圖服務" title="PPT.cc縮圖服務"></a>

summary:
因為kafka本身不會解析或查看數據，因此使用schema registry來協助數據驗證。
以 Apache Avro作為數據格式



## 13 Real World Insights and Case Studies (Big Data / Fast Data)

### 91: Choosing Partition Count & Replication Factor
Partition Count: 
- 取決於預期的throughput(建議在set up 前就先測量)
- 多個Partition表示：
    - 更好的throughput及平行處理
    - 可以同時有更多consumer在consumer group
    - 使用槓桿來操縱更大的cluster
    - but, you have more files opened on Kafka
    - but, have more elections that Zookeeper will perform for you
        

### 92: Kafka Topics Naming Convention約定的命名方式
https://medium.com/@criccomini/how-to-paint-a-bike-shed-kafka-topic-naming-conventions-1b7259790073 

### 93: Case Study - MovieFlix

業務內容：允許使用者按需觀看電視節目和電影的公司

需求：
1. 可以暫停後接續播放影片
2. build a user profile in real time
3. recommend the next show to the user in real time
4. store all the data in the analytics store


### 94: Case Study - GetTaxi

業務內容：allows people to match with taxi drivers on demand, right away.

需求：
1. The user should match with a close by driver
2. 按供需法則來做價格匹配（e.g.乘客多司機少，貴）
3. 旅行之前和旅行期間的所有位置數據都應存儲在分析存儲中，以便可以準確計算成本

### 95: Case Study - MySocialMedia

業務內容：user可以發布照片，而其他人則可以通過使用喜歡和評論來做出反應

需求：
1. The user should be able to post, like, and comment
2. The user should be able to see the total number of likes and comments per post in real time
3. Users should be able to see trending posts
4. high volume of data to be expected on the first of launch

note:
由於職責是分開的，所以我們可以稱其為CQRS

### 96: Case Study - MyBank

業務內容：數位銀行，allows real-time banking for its users。
想部署一種全新的功能：如果發生了非常大的交易，給用戶實時警報

需求：
1. the transaction data already exists in one of the databases
2. The thresholds can be defined by the users through their apps(can be changed at any time)
3. alerts must be sent in realtime to the users

### 97: Case Study - Big Data Ingestion

主要目的：transfer data from Kafka to HDFS, Amazon S3, or Elasticsearch...

kafka當作buffer, 通常會有兩層：
1. a speed layer for real time applications
2. a slow layer(batch layer) which helps with basically buffering data ingestion into analytic stores


### 98: Case Study - Logging and Metrics Aggregation

主要目的：蒐集多個applications的log or matrics then aggregation


