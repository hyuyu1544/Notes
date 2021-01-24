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



## circuit breaking
- cascading failures
- configuring outlier detection
- testing circuit breakers



## mutual TLS
