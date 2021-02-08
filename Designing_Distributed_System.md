# Designing_Distributed_System

## Introduction 
- 容器化: 容器化是一個實現分佈式的高效的方法
- 容器化設置了一個天然的邊界，邊界之外用接口進行通信
- 任何意料之外的情況都可以被限制在最小的影響範圍


## single node patterns
- 構成分散式系統模式的最小單位（Atomic elements）

### The Sidecar Pattern

<a href="https://ppt.cc/flBFEx"><img src="https://ppt.cc/flBFEx@.png" border="0" alt="PPT.cc縮圖服務" title="PPT.cc縮圖服務"></a>

- concept: 透過其他容器來「增添、擴展」主要容器的功能性
- 適當的將主應用功能與新功能進行解耦，便能`提高重用性`與`降低複雜度`
- Application Container: 負責主要業務邏輯之容器
  - Codebase 龐大且難以增加新功能
  - 可以移植進容器但已無人維護的 Legacy 服務
- Sidecar Container: 
  - 透過不包含在主容器的功能，來擴增及改善主應用容器的功能性
  - 會盡量`提高通用性`使其能夠搭配不同的應用容器
  - 常見應用：
    - Parameterized Containers: 將容器可調整的變數參數化，讓第三者使用時能夠根據不同場景進行調整
    - Define Each Container’s API: 提供對外 API 介面以供外部存取、監控
      - 向後兼容性 (backward compatibility)
      - 版本號 (Versioning)
    - Documenting Your Containers: 撰寫容器的文件

### Ambassadors

<a href="https://ppt.cc/fTse8x"><img src="https://ppt.cc/fTse8x@.png" border="0" alt="PPT.cc縮圖服務" title="PPT.cc縮圖服務"></a>

- concept: 代替應用程式容器與其他世界的服務做互動(通訊代理)
- 可以對容器做熔斷、流量監控、安全控制
- 常見應用：
  - 分流（load balance, 請求分流, 實驗分流）
  - 分片（sharding）

### Adapter

<a href="https://ppt.cc/fSLnex"><img src="https://ppt.cc/fSLnex@.png" border="0" alt="PPT.cc縮圖服務" title="PPT.cc縮圖服務"></a>

- concept: 修改應用程式容器介面
- 讓不同的容器有通用介面（統一的監控接口）
- 常見應用：
  - 監控
  - logging(fluentd)



## reference
1. https://rickhw.github.io/2019/05/17/About/DDS-zh_TW/
2. https://www.infoq.com/presentations/distributed-systems-complexity-human-factor/
3. https://tachingchen.com/tw/blog/desigining-distributed-systems-the-sidecar-pattern-concept/
4. https://segmentfault.com/a/1190000019600110

