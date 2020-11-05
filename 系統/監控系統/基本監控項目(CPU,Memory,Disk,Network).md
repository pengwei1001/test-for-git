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

## ◎ CPU
如果是在以前實體機或VM的時候，CPU可以監控的細項非常之多，包含使用率、user time、system time、wait、idle、nice、context switch等，但因產品為全雲端(GCP)，所以根據[Google Spanner文件](https://cloud.google.com/spanner/docs/cpu-utilization?hl=zh-tw)所述，GCP上的CPU監控主要就針對**CPU使用率**為主，關於Metric的部分，GCP大致上會提供以下指標：
1.CPU使用率(cpu_usage)(包含user & system使用率)
2.24小時滾動平均值(Rolling average 24 hour)：每個DB總CPU利用率的滾動平均值，以實例CPU資源的百分比表示。每個數據點都是前24小時的平均值。
3.高優先級(High priority)：高優先級任務的CPU利用率（佔實例CPU資源的百分比）。
4.總計(Total)：總CPU利用率，以實例CPU資源的百分比表示。

## ◎ Disk
Disk身為儲存資料的地方，當然看"剩餘空間"就很重要啦，另外Disk的I/O相關數據也很重要，目前GCP提供的Metric如下：
1.Disk 使用情況(bytes_used)
2.Disk I/O時間(io_time)
3.Disk 利用率(percent_used)
4.Disk 讀寫次數(write/read_bytes_count)

◎ Network
1.網路流量(network)
2.網路request數量(network/request_count)
3.TCP連接狀態(tcp_connections)
4.網路封包的接收與傳送數量(received/sent_packets_count)



◎ Memory
關於GCP的memory，雖然GCP提供實例的CPU，Network和Disk使用情況的Metric，但**預設狀況下GCP不提供memory的Metric**。
為了讓使用者可以監控GCP實例的Memory Metric，必須在實例上安裝Stackdriver代理。Stackdriver提供關於Memory的Metric如下：
1.記憶體使用率(memory_usage)
2.快取記憶體大小(cache_memory_usage)



參考文件：
[Red Hat Enterprise Linux 4: 系統管理導論第二章](https://web.mit.edu/rhel-doc/4/RH-DOCS/rhel-isa-zh_tw-4/ch-resource.html)
[Linux系統效能調教](https://ithelp.ithome.com.tw/users/20001007/ironman/357)
[Google Cloud metrics](https://cloud.google.com/monitoring/api/metrics_gcp)
[Agent metrics](https://cloud.google.com/monitoring/api/metrics_agent)
---
