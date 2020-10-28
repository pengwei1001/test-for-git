#更新進程的變數(Process Variable Update)<hr>
##變數更新操作說明
<br>
<ol>
<li>前言 : 
在閱讀本篇之前，希望你先了解哪些情況需要通知RD上新版號，如果還不清楚這部分，可先閱讀這篇【需上新版號的情境】。
<li>概要 : 
<br>
有時RD會新增變數並指定到某專案中，此時須對某專案之變數的設定做更新，以下詳細說明 :
<ul>
<li>當變數集群有異動(新增、刪除)，或變數集群內的變數有異動，需到指定的專案中，對變數的設定進行更新，後面會以實例做詳細解說。
<li>因此次更新會異動到進程的內容，所以在更新後要通知RD上新的版號。
</ul>
<br>
此為變數集群

![image.png](/.attachments/image-e8303827-e6e7-48bc-8128-3ab67ac7ecdc.png)
<br>
點擊某一變數集群內查看內容，圖中紅框處即為變數集群內的變數。

![image.png](/.attachments/image-6eab03c4-d1a2-4eff-8858-1c4367d58fbb.png)

----------------------------------------------------------------------------------------------------------------------
<li> 實例解說與步驟說明
<br><br>
<Step1>進入RD新增的變數集群內，確認要新增的各個變數(紅框處)後，更新至指定的專案。

![image.png](/.attachments/image-c0061bf0-d900-4bc0-a5a6-9d05d64cbeaa.png)
-->說明 : 本次實例為RD新增了 IAMRICH_PUBLISHER 這個變數集群，並指定更新至Platform的【login】、【center】、【analytics】、【http】、【api】的五項專案中。所以我們需要將紅框處的各個變數設定到這五項專案的process。

<br>
<Step2> 到RD指定的專案中，更新Process的變數設定。

![image.png](/.attachments/image-42bcb2ca-c1d3-47b1-bab4-b437429d7e2c.png)

<br>
<Step3>到要更新的專案後，將剛才要新增的各個變數設定在Container的環境變數中。

![image.png](/.attachments/image-0d75debf-fc66-455a-9ad9-af79ce7b9090.png)
![image.png](/.attachments/image-d55ede37-cadf-4126-9479-cf9e3c795735.png)
![image.png](/.attachments/image-b79b9a61-e7ba-4a32-9246-c26556656fc5.png)

<br>
<Step4> 各個變數都新增完後，檢查設定是否正確(有無打錯變數名稱、變數有沒有都加到)

![image.png](/.attachments/image-5a021f3b-a7b0-44b8-8848-b2e527575abc.png)

<br>
<Step5> 將各個變數新增至Container的環境變數後，要確認這次的變數集群有在專案的變數設定中，如果沒有的話要新增進來，否則剛才設定的變數在這個專案就會找不到。

![image.png](/.attachments/image-80459c60-b821-4c1f-bae2-ec950140837e.png)
![image.png](/.attachments/image-b6ce41c3-e4f3-458e-b9b1-7c713ea82c20.png)
<hr>
以上只是更新好【login】的process變數設定，剩下的四個專案的更新步驟跟上面一樣，把他們都更新完成後，我們就完成變數的更新囉!

#END

