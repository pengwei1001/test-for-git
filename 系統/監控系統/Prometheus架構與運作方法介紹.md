# ◎ 前言
在看這篇文件前，建議先了解一些專有名詞及概念會比較好理解

- [ ] Metrics：度量、指標
我們可先從2種定義去理解 Metrics 這概念：
- [ ] Google Analytics 服務裡面對 Metrics 的定義為：Metrics 是資料的量化評估方式。是指維度中可透過總計或比率方式衡量評估的個別元素。[(引用來源)](https://support.google.com/analytics/answer/6086087?hl=zh-Hant&ref_topic=6083659)
- [ ] IEEE的軟體工程術語標準辭典(IEEE Standard Glossary of Software Engineering Terms)中對 metric (度量)的定義：系統、元件或過程具有給定屬性程度的定量測量。[(引用來源)](http://www.mit.jyu.fi/ope/kurssit/TIES462/Materiaalit/IEEE_SoftwareEngGlossary.pdf)

從上面兩種定義敘述， Metrics (指標)可理解成"給予特殊屬性或定義，並計算出數值，並可從這數值了解資料、或系統的狀況，進而評估接下來的改善方式"

例如說， Prometheus 中的其中1個 Metric 為"prometheus_tsdb_reloads_total"，那這 Metric 所計算出來的數值為"計算 TSDB reloads 的總數"，那就可以根據這數值判斷 TSDB 的狀況並看接下來有何改善方式。




- [ ] [TSDB(Time Series Database)](https://en.wikipedia.org/wiki/Time_series_database)：時間序列資料庫
針對時間戳記(Timestamp)或時間序列數據進行優化，經過優化後，專門用來儲存與管理時間序列資料(Time Series Data)的資料庫系統。

- [ ] Multi-Dimensional Model：多維度資料模型
一般的資料結構通常是表格式資料模型（Tabular Model），使用傳統的表格然後多個表格關聯
但多維度資料結構型態為Cube(可想像成正方體)，使用維度(Dimension)和量值(Measure)為基礎
相較之下多維度資料模型具有較高的可擴展性，但也較為困難且複雜。

---


# ◎ 架構與相關元件
![image.png](/.attachments/image-19eba535-8d5b-4fcb-a6cc-fa6bfb40d11a.png)
圖片來源：[Prometheus官網](https://prometheus.io/docs/introduction/overview/)

從官網提供之架構圖可知，Prometheus 架構中是由多個元件組成，相關元件介紹如下：
(P.S其中有些是可選可不選的元件，依據自己產品或設計架構有所調整)

**1. Prometheus Server**
![image.png](/.attachments/image-2f7b0c23-bc8e-4982-b200-44f928296dda.png)
為 Prometheus 的核心主程式，本身也是1個時間序列資料庫，負責整個監控集群的數據拉取、處理、計算和存儲。
Prometheus Server 裡面包含Retrieval、TSDB及HTTP Server3個模組。






**2. Pushgateway (可選)**
![image.png](/.attachments/image-69991ab3-3588-4bf9-9353-6ab344b11451.png)
為 Prometheus 組件，類似代理服務概念，Pushgateway 存在的原因主要是為了解決"不支援或無法用 pull 方式獲取數據的情形"，例如：
(1) 用於臨時性Job(Short-lived jobs)推送(最大宗)。
有些 Job 存在期間較短，有可能 Prometheus 來 Pull 時就消失，因此需透過1個 Pushgateway 來推送，並讓 Prometheus 去 Pushgateway pull metrics。

(2) 當網路環境不允許 Prometheus Server 和 Exporter 直接進行資料數據pull的時候(ex：在不同的子網路or防火牆)，就可使用 PushGateway 來進行轉運資料(可想像成類似轉運站的概念)。

(3)如果有"自己定義 shell 的腳本"來監控服務健康狀態，因自己定義shell的腳本可能會不符合Prometheus的定義，所以也可使用 Pushgetway 將資料push上去。
(不過如果是都要自己定義shell腳本的話，那通常都會寫exporter，所以第3種狀況不太會用 Pushgetway 來處理XD")

但是官網建議在上述幾種狀況用 PushGateway 即可，如太常使用的話可能會有影響，須留意~!：
1.如果將多個targets、jobs or instances的數據資料匯入到單一 PushGateway 做監視，容易使這 PushGateway 有單點失誤及潛在危險的風險。

2.如果Prometheus pull "up" 的資訊(up的 Metric 意義：指那機器是否有on起來)，只會pull到 PushGateway 的up資訊，無法去細分說在 Pushgetway 之前的節點up資訊為何。

3.Pushgateway 可以"永遠 push 監控資料"&永遠暴露給 Prometheus "，看起來好像是優點，但是，如果需要 push 到 Pushgetway 的資料不需要監控了， Pushgateway 還是會繼續 push 數據&暴露給 Prometheus ，就會導致 Prometheus 一直 pull 到不必要的數據。要解決這狀況，只能手動清理並釐清有哪些資料需要 push 到 Pushgateway 。






**3. Exporters/Jobs**
![image.png](/.attachments/image-b76482ac-e8bd-4120-8852-d5cba68d4e57.png)
Exporter 為第三方 Client Library 開發的 HTTP server，用來曝露已有第三方服務的 Metrics 給 Prometheus Server，只要符合 Prometheus 的接口格式，就可被採集 Metrics 。針對是否有直接支援 Prometheus 部分，Exporter 分成2類：
(1) 直接採集：直接內建對 Prometheus 支援，例如 cAdvisor，Kubernetes，Etcd 等。
(2) 間接採集：原有監控目標不直接支援 Prometheus ，因此需要通過官方或第三方提供 exporter，才能監控目標並採集資料。例如：要監控 Mysql 的話需要 Mysql Exporter，要監控 Nginx 則需要 nginx-prometheus-exporter 等。





**4. AlertManager**
![image.png](/.attachments/image-f4488b5d-9d5b-4242-886f-ee8bce0f7d46.png)
接收來至 Prometheus Server 的 Alert event ，並依據定義的 Notification 組態發送警報，常見的接收方式有：e-mail，pagerduty，webhook，slack 等。

◎關於 AlertManager 更詳細資料可到「告警系統」




**5. Service Discovery**
Service Discovery 是自動檢測網絡上的設備和服務的過程，通過網絡上的通用語言連接，服務端與客戶端雙方可允許設備或服務進行連接而無需任何手動干預。Prometheus 支援多種自動發現機制，比如kuberbetes、DNS、consul、zookeeper、etcd等服務。





---
# ◎ Prometheus運作方法
1. Prometheus 的基本原理是**通過HTTP**，**定期**pull被監控組件的狀態，任意组件只要提供對應的HTTP接口，即可被監控。


首先，Prometheus Server 中的 Retrieval 負責定義 Prometheus Server 需要從哪些地方pull數據，定義完後再定時收集及pull數據，要監控的數據資料可從4個地方來(如下圖)
- [ ] 從 Jobs 或者 Exporters 中拉取 Metrics
- [ ] 來自 Pushgateway 的 Metrics
- [ ] 如有監控其他的 Prometheus Server 的需求，因都有HTTP接口，所以也可從被監控的 Prometheus Server 拉取 Metrics。
- [ ] Service discovery 中發現的targets
![image.png](/.attachments/image-bec00276-5d49-4ff2-b325-f7301324ee21.png)





2. Retrieval 收集數據後，就會把數據資料傳給TSDB，TSDB再做以下事情
![image.png](/.attachments/image-944cf4df-b993-4c56-b75e-25db44721f09.png)
- [ ] 數據處理：根據配置的數據格式或者標籤做轉換/刪除等操作。
- [ ] 根據已定義好的alert.rule中進行計算&判斷：例如rule裡面有條告警規則定義是"CPU使用率達到80%"，那 TSDB 會對數據進行計算看是否符合告警定義，如果符合，則發送警告給 AlertManager ；如不符合就不做操作。
- [ ] 存儲資料：完成上面的操作之後，TSDB 會根據配置時間周期保存數據到Local端或者是第三方存儲中。
(至於HTTP Server則是提供 http 服務接口查詢，預設 Port 為9090。)





3. Alertmanager 接收到 Prometheus Server 之告警後，依據配置文件進行告警發送。
◎關於 AlertManager 更詳細資料可到「告警系統」
![image.png](/.attachments/image-38009904-ff3c-461a-a902-243e7dd58538.png)





4. 視覺化的部分，則是透過 PromQL 語法進行查詢，查詢後再將資料給 Web UI or Dashboard，例如 Prometheus 自己的UI介面、Grafana等。(目前最推薦的是Prometheus+Grafana這組合)
![image.png](/.attachments/image-3077f114-149b-4824-a447-fe7c317b813f.png)


---
由上面的架構跟運作方法，我們可以知道：


# ◎ Prometheus適合用在?
因Prometheus本身為時間序列資料庫 (TSDB，Time Series Database) ，再加上資料本身為多維度資料模型 (Multi-Dimensional Model) ，所以 Prometheus 很適合記錄任何純數字時間序列的資料。


# ◎ Prometheus的侷限
- [ ] 因Prometheus 是基於 Metric 的監控，所以不適用於日誌（Logs）、事件(Event)。
- [ ] Prometheus 預設是 Pull 模型，如果需要抓到Push的資料需使用第三方服務幫忙，或搭配Pushgateway。







