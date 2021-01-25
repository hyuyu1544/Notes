## fault injection
- why important?
  - 因為network不是100% reliability, 所以要有容錯能力
  - 分佈式系統不可能100％ reliabe, so you have to have fault tolerance.
  - search: fallacies of distributed computing (https://en.wikipedia.org/wiki/Fallacies_of_distributed_computing)
  - 把錯誤引進architecture in kiali：
    - 刪掉gateway
    - delete metadata
  - chaos engineering : 一個由netflix做的問題製造機，可以製造問題進到你的程式？！
  (https://en.wikipedia.org/wiki/Chaos_engineering#:~:text=Chaos%20engineering%20is%20the%20discipline,withstand%20turbulent%20and%20unexpected%20conditions)
  
  - architecture in kiali
  - 延遲和故障引入微服務架構 (demo is fail???)(他覺得netflix這一塊（？）做得很好)
```  
- why do that?
  - because network is not 100% reliable: 對分布式系統的錯誤認知：
    1. network is reliable
    2. ...
  - 可以在yaml 這樣寫：fault: delay: value: fixedDelay ...
  - 也可以引用別人的服務： chaos engineering
```


## circuit breaking
- cascading failures(級聯失敗)
  - 過去may用過：hystrix
  - cause of cascading failures: 很多原因像是，pod本身run out of resource, run out of ram, coding problem, environ, TCP on pod ...
  - 宜中一個問題導致整條chain故障，就稱為級聯故障
  - 故障了通常要做什麼呢？
    - 立刻重啟系統並重新運行
    - debug很花時間（故障片及整個系統, 最初原因可能很難追蹤）
  - 如何避免這個問題,要設置斷路器
    - 斷路（circuit breaking）
    - 斷路器 circuit breaker: 轉移每個網路請求 （e.g: hystrix）
    - 先轉移走請求（回給使用者503,而不是讓大家都在這裡等待,佔著資源待久），讓為服務可以快速重啟或安排到前他節點上運作
  - hystrix (netflix使用此架構)
  - ref: https://www.servicemesher.com/blog/istio-circuit-breaking/
- configuring outlier detection
- testing circuit breakers














## mutual TLS
