![image.png](/.attachments/image-da348ceb-88ed-4fbb-b3e5-e5a7f8e6347c.png)
圖片來源：[Prometheus官網](https://prometheus.io/docs/introduction/overview/)
# ◎ 首先，先來聊聊 AlertManager 是甚麼

AlertManager，顧名思義就是"管理Alert"，Prometheus 的警報分為兩個部分：
1.Prometheus Server 中的警報規則，將警報發送到 Alertmanager。
2.Alertmanager 接收到警報規則後，開始管理這些警報，之後再發出通知，例如e-mail、slack等。

由上得知一個很重要的觀念，就是

** Alertmanager 本身是不做告警規則計算的**

意思是說，告警規則計算是由 Prometheus Server 負責， Alertmanager 只是在接收 Prometheus Server 發來的消息，然後結合自己的配置，比如等待時間，重複發送告警時間，路由匹配等項目，然後把接收到的消息發送到指定的接收者。

所以在設計 config 檔時，Prometheus 和 AlertManager 都要設計config檔，分別去設計 Promethrus Server 的告警規則，以及 AlertManager 的告警時間及配置模式，並且要讓Prometheus 跟 Alertmanager 能夠互相溝通，這整個監視+告警系統才算完善。

◎ AlertManager 有哪些特性？
首先，AlertManager 會將收到的告警做「重複數據刪除」(deduplicating)，確定資料無重複性後，再依據自身的 config 配置將告警做「分組」(grouping)，分組後再依據自身的配置時間等等將內容「路由」(routing )給指定的接收者。其中告警會經歷３種狀態：
- [ ] Inactive：沒有觸發閾值
- [ ] Pending：已觸發閾值但未滿足告警持續時間
- [ ] Firing：已觸發閾值且滿足告警持續時間

![image.png](/.attachments/image-e89e08bb-4daa-4de5-953d-179ec4deeb18.png)

另外 AlertManager 還有相關的核心概念如下：
1.分組(Group)：當系統一次出現大量的故障並且可能同時觸發數百到數千個警報時，分組這功能可以將類似性質的警報分類，有點類似合併的概念，不僅可以整合同類的告警，幫助維運單位排查問題
之外，通過告警郵件、訊息的合併，也減少告警數量，畢竟告警數量一多，很有可能無法排查真正的問題。至於警報如何分組、分組通知的時間，以及這些通知的接收者由 config 中的 route 去配置。


2.抑制(Inhibition)：這概念可以解釋為"如果某些其他警報已經觸發，則抑制某些警報的通知"，這概念可以往兩個方向去延伸：
(1) 消除無關的告警：有可能在觸發警報的當下也會因為這警報觸發其他無關的警報，所以這功能可以防止與實際問題無關的觸發警報通知。
(2) 高級別告警抑制低級別告警："如果某些其他警報已經觸發，則抑制某些警報的通知"，這概念其實也有點"優先權"的意味，而且很有可能高等級的告警一觸發，低等級的告警閥值因比高等級低所以也一定會被觸發到，所以可以藉由這功能去做"高級別告警"抑制"低級別告警"的作用，這部份則是由 Alertmanager 的 config 文件去設定。

3.靜默(Silences)
可以在給定時間內使警報靜音。靜默會檢查傳入警報是否與靜默的配置相同，或正則表達式匹配項是否匹配。如果相同，則不會針對該警報發送任何通知。這樣就阻止發送可預期的告警，確保處理期間不會收到重複的告警。配置靜默的位置則是在 Alertmanager 的 Web 界面。






