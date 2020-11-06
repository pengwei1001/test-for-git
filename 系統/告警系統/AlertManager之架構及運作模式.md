![image.png](/.attachments/image-da348ceb-88ed-4fbb-b3e5-e5a7f8e6347c.png)
圖片來源：[Prometheus官網](https://prometheus.io/docs/introduction/overview/)
# ◎ 首先，先來聊聊甚麼AlertManager

AlertManager，顧名思義就是"管理Alert"，Prometheus的警報分為兩個部分：
1.Prometheus Server 中的警報規則，將警報發送到Alertmanager。
2.Alertmanager 接收到警報規則後，開始管理這些警報，之後在發出通知，例如e-mail、slack等。

由上得知一個很重要的觀念，就是

**Alertmanager本身是不做告警規則計算的**

意思就是告警規則計算是Prometheus Server來計算，Alertmanager只是在接收Prometheus Server發來的消息，然後結合自己的配置，比如等待時間，重複發送告警時間，路由匹配等項目，然後把接收到的消息發送到指定的接收者。

所以在設計config檔時，Prometheus 和 AlertManager都要設計config檔，分別去設計Promethrus Server的告警規則，以及AlertManager的告警時間及配置模式，並且要讓Prometheus 跟Alertmanager能夠溝通，這整個監視+告警系統才算完善。




