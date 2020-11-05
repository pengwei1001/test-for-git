在看這篇文件前，建議先了解一些專有名詞及概念會比較好理解

- [ ] Metrics：度量、指標
可從兩種定義去理解Metrics這概念
Google Analytics服務裡面對Metrics的定義為：Metrics是資料的量化評估方式。是指維度中可透過總計或比率方式衡量評估的個別元素。[(引用來源)](https://support.google.com/analytics/answer/6086087?hl=zh-Hant&ref_topic=6083659)
IEEE的軟體工程術語標準辭典(IEEE Standard Glossary of Software Engineering Terms)中對metric(度量)的定義：系統、元件或過程具有給定屬性程度的定量測量。[(引用來源)](http://www.mit.jyu.fi/ope/kurssit/TIES462/Materiaalit/IEEE_SoftwareEngGlossary.pdf)

從上面兩種定義敘述，Metrics(指標)可理解成"給予特殊屬性或定義，並計算出數值，並可從這數值了解資料、或系統的狀況，進而評估接下來的改善方式"

例如說，Prometheus中的其中一個Metric為"prometheus_tsdb_reloads_total"，那這Metric所計算出來的數值為"計算TSDB reloads的總數"，那就可以根據這數值判斷TSDB的狀況並看接下來有何改善方式。




- [ ] TSDB(Time Series Database)：時間序列資料庫
針對時間戳或時間序列數據進行優化的數據庫。時間序列數據隨時間跟踪，監視，下採樣和聚合的測量或事件。

- [ ] Multi-Dimensional Model：多維度資料模型
資料結構型態：Cube
使用維度(Dimension)和量值(Measure)為基礎
維度 (Dimension)：資料庫中的設計是一個表格 (Table)，是物件的敘述性屬性或特性，可以有不同的值。
量值 (Measure) 資料庫中的Fact Table
較高的可擴展性
要為成熟的資料模型架構
從使用者觀點出發，依照資料使用的視角或資料分析的維度來設計資料結構
較為困難且複雜


---


# ● 架構與相關元件
![image.png](/.attachments/image-19eba535-8d5b-4fcb-a6cc-fa6bfb40d11a.png)
圖片來源：[Prometheus官網](https://prometheus.io/docs/introduction/overview/)

從官網提供之架構圖可知，Prometheus 架構中是由多個元件組成，相關元件介紹如下：
(P.S其中有些是可選可不選的元件，依據自己產品或設計架構有所調整)

**1. Prometheus Server**
![image.png](/.attachments/image-2f7b0c23-bc8e-4982-b200-44f928296dda.png)
為 Prometheus 的核心主程式，本身也是一個時間序列資料庫，負責整個監控集群的數據拉取、處理、計算和存儲。
Prometheus Server 裡面包含三個模組如下：
(1) Retrieval：負責採樣、定時收集及pull數據。
(2) TSDB：儲存時間序列資料於本地磁碟。
(3) HTTP Server：提供 http 服務接口查询，預設 Port 為9090。






**2. Pushgateway (可選)**
![image.png](/.attachments/image-191630a8-1845-49de-a23e-81bc45a0a56d.png)
為 Prometheus 組件，類似代理服務概念，Pushgateway 存在的原因主要是為了解決"不支援或無法用 pull 方式獲取數據的情形"，例如：
(1) 用於臨時性Job(Short-lived jobs)推送。有些 Job 存在期間較短，有可能 Prometheus 來 Pull 時就消失，因此需透過一個Pushgateway來推送，並讓 Prometheus 去 Pushgateway pull metrics。

(2) 當網路環境不允許 Prometheus Server 和 Exporter 直接進行資料數據pull的時候(ex：在不同的子網路or防火牆)，就可使用 PushGateway 來進行中轉(可想像成類似轉運站的概念)。

(3)如果有自己定義 shell 的腳本來監控服務健康狀態，或有監控多項不同的數據，可藉助 PushGateway 把對應數據按照 Prometheus 的格式 push 到 PushGateway。

但是官網建議在上述幾種狀況用 PushGateway 即可，如太常使用的話可能會有影響(須留意)：
1.如果將多個targets、jobs or instances的數據資料匯入到單一PushGateway做監視，容易使這PushGateway有單點失誤及潛在危險的風險。
2.如果Prometheus pull up 的資訊，up資訊只會針對PushGateway, 不會pull到每個匯入資料節點的up資訊。
3.Pushgateway 可以"永遠push"&永遠暴露給Prometheus"相關節點给它的所有監控數據。看起來好像是優點，但是如果不需要監控了，Pushgateway還是會繼續push數據&暴露給Prometheus，就會導致Prometheus一直pull到舊數據，要解決這狀況只能手動清理Pushgateway不要的數據。






**3. Exporters/Jobs**
![image.png](/.attachments/image-826bed33-f601-4408-a936-efe142747e2f.png)
Exporter 為 Client Library 開發的 HTTP server，用來曝露已有第三方服務的 Metrics 給 Prometheus Server，只要符合接口格式，就可被採集Metrics。針對是否有直接支援Prometheus部分，Exporter分成2類：
(1) 直接採集：直接內建對Prometheus支援，例如cAdvisor，Kubernetes，Etcd，Gokit等。
(2) 間接採集：原有監控目標不直接支援Prometheus，因此需要通過Prometheus提供的Client Library編寫該監控目標的監控採集程式。例如：Mysql Exporter，JMX Exporter，Consul Exporter等





**4. AlertManager**
![image.png](/.attachments/image-721237a5-1159-46c0-91df-c97391bbb6ad.png)
接收來至 Prometheus Server 的 Alert event ，並依據定義的 Notification 組態發送警報。
Prometheus Server 中支援基於PromQL建立告警規則，如果滿足PromQL定義的規則，則會產生一條告警。AlertManager從 Prometheus server 端接收到 alerts後，會進行去除重複資料，分組，並傳輸到指定的接受方式發出告警訊息。常見的接收方式有：e-mail，pagerduty，webhook，slack 等。

◎關於 AlertManager 更詳細資料可到「告警系統」




**5. Service Discovery**
Service Discovery是自動檢測網絡上的設備和服務的過程，通過網絡上的通用語言連接，服務端與客戶端雙方可允許設備或服務進行連接而無需任何手動干預。Prometheus支援多種自動發現機制，比如kuberbetes、DNS、consul、zookeeper、etcd等服務。





---
# ● Prometheus運作方法
1. Prometheus的基本原理是**通过HTTP**，**定期**pull被監控組件的狀態，任意组件只要提供對應的HTTP接口，即可被監控。
首先，Prometheus Server中的Retrieval會定時採樣、收集及pull數據，要監控的數據資料可從4個地方來(如下圖)
- [ ] 從 Jobs 或者 Exporters 中拉取 Metrics
- [ ] 來自 Pushgateway 的 Metrics
- [ ] 如有監控其他的 Prometheus Server 的需求 ，因都有HTTP接口，所以也可從被監控的 Prometheus Server 拉取 Metrics。
- [ ] Service discovery中發現的targets
![image.png](/.attachments/image-bec00276-5d49-4ff2-b325-f7301324ee21.png)










---

# ● Prometheus適合用在?
因Prometheus本身為時間序列資料庫 (TSDB，Time Series Database) ，再加上資料本身為多維度資料模型 (Multi-Dimensional Model) ，所以 Prometheus 很適合記錄任何純數字時間序列的資料。


# ● Prometheus的侷限
- [ ] 因Prometheus 是基於 Metric 的監控，所以不適用於日誌（Logs）、事件(Event)。
- [ ] Prometheus 預設是 Pull 模型，如果需要抓到Push的資料需使用第三方服務幫忙，或搭配Pushgateway。







