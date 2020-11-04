# ● 架構與相關元件
![image.png](/.attachments/image-19eba535-8d5b-4fcb-a6cc-fa6bfb40d11a.png)
圖片來源：[Prometheus官網](https://prometheus.io/docs/introduction/overview/)

從官網提供之架構圖可知，Prometheus 架構中是由多個元件組成，相關元件介紹如下：
(P.S其中有些是選擇性的元件，依據自己產品或設計架構有所調整)

**1. Prometheus Server**
為 Prometheus 的核心主程式，本身也是一個時間序列資料庫，負責整個監控集群的數據拉取、處理、計算和存儲。Prometheus Server 裡面包含三個模組：
(1) Retrieval：負責採樣、定時收集及pull數據
(2) TSDB：儲存時間序列資料於本地磁碟
(3) HTTP Server：提供 http 服務接口查询，預設 prot 為9090


**2. Pushgateway (可選)**
為 Prometheus 組件，類似代理服務概念，適合用於服務層面的 Metrics。 Pushgateway 存在的原因主要是"解決不支援或無法用 pull 方式獲取數據的情形"，例如：
(1) 用於臨時性Job(Short-lived jobs)推送。有些 Job 存在期間較短，有可能 Prometheus 來 Pull 時就消失，因此透過一個閘道來推送，並讓 Prometheus 去 Pushgateway pull metrics。
(2) 當網路環境不允許 Prometheus Server 和 Exporter 直接進行通訊時(ex：子網路or防火牆)，可以使用 PushGateway 來進行中轉(類似轉運站)。
(3)如果有自己定義 shell 的腳本來監控服務健康狀態，可藉助 PushGateway 把對應數據按照 Prometheus 的格式 push 到 PushGateway。

◎須留意：Prometheus只拉取使用者最後1次push的資料。如果客戶端一直沒有推送新的指標到 PushGateway ，那麼 Prometheus 將始終拉取最後推送上的資料，直到指標消失，預設是5分鐘。


**3. Exporters/Jobs**
Exporter 為 Client Library 開發的 HTTP server，用來曝露已有第三方服務的 Metrics 給 Prometheus Server，只要符合接口格式，就可被採集Metrics。針對是否有直接支援Prometheus部分，Exporter分成2類：
(1) 直接採集：直接內建對Prometheus支援，例如cAdvisor，Kubernetes，Etcd，Gokit等。
(2) 間接採集：原有監控目標不直接支援Prometheus，因此需要通過Prometheus提供的Client Library編寫該監控目標的監控採集程式。例如：Mysql Exporter，JMX Exporter，Consul Exporter等




