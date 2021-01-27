## Fault Injection
- why important?
  - 因為分佈式系統的network不是100% reliability, 所以要有容錯能力, so you have to have fault tolerance
```
...
spec:
  hosts:
  - ratings
  http:
  - fault:
      delay:
        fixedDelay: 7s
        percentage:
          value: 100
...
```
  - 可以引用別人的服務驗證（將延遲和故障引入微服務架構）： chaos engineering


## Circuit Breaking
- cascading failures(級聯失敗)
  - cause of cascading failures: pod本身run out of resource, run out of ram, coding problem, environment ...
  - 其中一個問題導致整條chain故障，就稱為級聯故障
  - 故障了通常要做什麼呢？
    - 立刻重啟系統並重新運行
    - debug很花時間（故障片及整個系統, 最初原因可能很難追蹤）
  - 如何避免這個問題？設置斷路器
    - 斷路（circuit breaking）
    - 斷路器 circuit breaker: 轉移每個網路請求 （e.g: hystrix）
    - 先轉移走請求（回給使用者503,而不是讓大家都在這裡等待,佔著資源），讓為服務可以快速重啟或安排到其它節點上運作
  - 別的選擇：hystrix (netflix以java開發) [補充：netflix改去開發Resilience4J了]
  
- configuring outlier detection
  - 配置斷路器, 可以設定由什麼觸發，e.g. 502 or 503 or 504 , 幾秒內幾次這個錯誤，或是幾個請求中幾次這樣的錯誤等
  - 偵測到錯誤的方式可以漸進式，例如：第一次錯誤，斷路器開始３０秒；第二次６０秒；第三次９０秒．．．（可倍數增長也可指數增長）

```
...
spec:
  host: httpbin
  trafficPolicy:
    outlierDetection:
      baseEjectionTime: 3m
      consecutiveErrors: 1
      interval: 1s
      maxEjectionPercent: 100
...
```


## Mutual TLS（Transport Layer Security）
- Why is encryption needed inside a cluster?
  - 兩個服務之間的對話要被加密，走https(事實上，是客戶端像server請求的協議)
  - 走完https, tls就要上場囉(https可防止men in the middle攻擊) 
  - 群集的安全性
    - pod之間的交流並未加密
    - 部署可能會在使用者自己架設的多台實體機上
    - 或是部署到ＡＷＳ上，不同的可用區
    - 只要離開node,就很有可能被攔截攻擊(men in the middle)
    - 建議cluster內部的交流都用https
    
- How Istio can upgrade traffic to TLS?
  - 作者認為要把所有交流升級成https有困難，因此提供其他的解決方案
  - istio的中控（istiod）間有個citadel，可以幫助你做到這件事（讓獨立的pod不再是獨立的pod）
  - 是雙向的TLS 
```
...
spec:
  mtls:
    mode: STRICT/PERMISSIVE
...
```

- Enabling mTLS - it's Automatic
  - istio會自動攔截pod之間的http交互,升級成https
  - 不用設定yaml file

- STRICT vs PERMISSIVE mTLS
  - 如果發送請求的pod沒有用proxy, istio無法自動升級請求到https
  - 默認permissive mode(寬鬆模式), 加幾行code在yaml, 變成strict mode（嚴格模式）
  - 寬鬆模式：可以通過http請求（then 自動升級成https?）
  - 嚴格模式：會拒絕http請求



### note & reference
- fallacies of distributed computing (https://en.wikipedia.org/wiki/Fallacies_of_distributed_computing)
- chaos engineering : 一個由netflix做的問題製造機，可以製造問題進到你的程式？！
  (https://en.wikipedia.org/wiki/Chaos_engineering#:~:text=Chaos%20engineering%20is%20the%20discipline,withstand%20turbulent%20and%20unexpected%20conditions)
- circuit breaking: https://www.servicemesher.com/blog/istio-circuit-breaking/
- Istio Destination Rule: https://istio.io/latest/docs/reference/config/networking/destination-rule/
- hystrix: https://www.exoscale.com/syslog/istio-vs-hystrix-circuit-breaker/
- compare TLS and SSL(secure socket layer) （傳輸層安全性 v.s. 安全通訊端層）
  - SSL protocol got to version 3.0; TLS 1.0 is SSL 3.1 
- PCI 認證 (以便您可以處理信用卡數據)


