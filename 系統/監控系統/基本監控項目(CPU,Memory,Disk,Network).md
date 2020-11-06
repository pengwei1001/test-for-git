# ◎前言
在講監控時，都會知道說要監控一些資源之類的，但是，講到要監控資源，會想到哪些?監控的基本項目有哪些？除了使用率、負載以外還有哪些細項需要監控？
這篇文章主要重點主要會有2點：
1.基本監控項目有哪些？有哪些細項?
2.因監控系統使用Prometheus，所以會列出基本監控項目會搭配哪些Metric


# ◎ 一、基本監控項目

在監控資源之前，必須先知道要監控哪些資源。雖然說監控的範圍真的很廣，但基本上所有系統都會有以下4個資源：
- [ ] CPU 
- [ ] Disk 
- [ ] Network 
- [ ] Memory

因基本上不管任何系統產品或服務，都會有這4項基本的監控資源，所以先以這4大項做細部解說

---

在這邊要先講一件重要的概念，就是GCP對於VM層級和GKE層級的監控項目略有不同喔~

如果在VM instanes上要觀察Memory、磁碟使用率&磁碟I/O指標的話，就會出現**您必須安裝 Cloud Monitoring 代理程式，才能取得記憶體用量指標。**的提醒訊息(如下圖)，但GKE可以觀察到Memory和磁碟使用率相關資訊。

![image.png](/.attachments/image-b869f9a2-3f74-4720-9f75-4b8966bce169.png)

![image.png](/.attachments/image-bc2ce231-fbed-4134-babb-5c71b0351225.png)


GKE就會顯示CPU、Memory、Disk的使用率(如下圖，以rd1的環境為例)

![image.png](/.attachments/image-491dd56c-f778-4121-a30a-338934ee79d2.png)

---

## ◎ CPU
如果是在以前實體機或VM的時候，CPU可以監控的細項非常之多，包含使用率、user time、system time、wait、idle、nice、context switch等，但因產品為全雲端(GCP)，所以根據[Google Spanner文件](https://cloud.google.com/spanner/docs/cpu-utilization?hl=zh-tw)所述，GCP上的CPU監控主要就針對**CPU使用率**為主，關於Metric的部分，GCP大致上會提供以下指標：
1.CPU使用率(cpu_usage)(包含user & system使用率)
2.最高CPU使用率

## ◎ Disk
Disk身為儲存資料的地方，當然看"剩餘空間"就很重要啦，另外Disk的I/O相關數據也很重要，關於Disk的監控細項如下：
1.Disk 使用率(percent_usage)
2.Disk 最高讀寫總處理量(write/read_bytes_count)

◎ Network
Network部分相較之下就比較多元，包含要注意流量、Port connection的狀況、封包(packet)傳輸接收的情形等，目前GCP提供關於Network的監控細項如下：
1.Port使用狀況(port_usage)
2.網路request數量(network/request_count)
3.TCP連接狀態(tcp_connections)
4.網路封包(packets)的接收與傳送數量(received/sent_packets_count)
5.網路封包(packets)被丟棄的數量(dropped_packets_count)(有可能因為firewall的關係導致packet被丟棄)
6.HTTP request & response 的個數(request/response_count)
7.最高傳送&接收流量
8.防火牆擋下流量最高
9.RTT 延遲時間


◎ Memory
關於GCP的memory，雖然GCP提供實例的CPU，Network和Disk使用情況的Metric，但**預設狀況下GCP不提供memory的Metric**。
為了讓使用者可以監控GCP實例的Memory Metric，必須在安裝Cloud Monitoring 代理程式才能監控，但GKE可直接監控Memory的狀況，相關於Memory監控細項如下：
1.記憶體使用率(memory_usage)
2.快取記憶體大小(cache_memory_usage)



參考文件：
[Red Hat Enterprise Linux 4: 系統管理導論第二章](https://web.mit.edu/rhel-doc/4/RH-DOCS/rhel-isa-zh_tw-4/ch-resource.html)
[Linux系統效能調教](https://ithelp.ithome.com.tw/users/20001007/ironman/357)
[Google Cloud metrics](https://cloud.google.com/monitoring/api/metrics_gcp)
[Agent metrics](https://cloud.google.com/monitoring/api/metrics_agent)
---
