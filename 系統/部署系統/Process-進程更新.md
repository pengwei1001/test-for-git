#進程的更新(Process Update)<hr>
##進程更新操作說明 
<ol>
<br>
<li>前言 : 在閱讀本篇之前，希望你先了解哪些情況需要通知RD上新版號，如果還不清楚這部分，可先閱讀這篇【需上新版號的情境】。
<li>概要 : 
<br>
有時候某些專案的部屬進程，會有新的流程步驟需要加入，此時須更新job的專案，以下詳細說明 : 
<ul>
<li>當部屬進程有加入新的流程時，需同步更新至有在Job裡的所有Project。
<li>目前是Cdn及Config的專案有在Job中，所以Cdn與Config的所有專案都要同步更新。(請參考下兩圖)
<li>因此次更新會異動到進程的內容，所以在更新後要通知RD上新的版號。
</ul>

![image.png](/.attachments/image-7e0175a3-1437-4e8f-840d-5f3e377bdd95.png)
![image.png](/.attachments/image-3c659607-b099-43b6-9ddd-d585949893bc.png)
<br>
<hr>
<li>實例解說與步驟說明 : 
<br>
<br>
<Step1> 先到已有部屬新流程的專案中，確認有新增哪些流程。

![image.png](/.attachments/image-cf34596d-2c4a-4090-8c22-eab4c70f11d9.png)
-->說明 : 以本次範例的專案太極龍神來說，該專案有新增show-logs與check-success兩項流程，所以要將這兩項流程更新到其他cdn及config的專案中。

<br>

<Step2> 確認要更新哪些流程後，要把新流程複製到cdn及config的專案中，但在此之前先確認bash，檢查變數Job-name內的value是否寫死，因為這關係到要部屬至哪個專案及哪個版本號，如果是寫死的，就要把這個變數改成你要部屬的專案名稱及版本號。以本範例解說，太極龍神專案名稱為cdn-taichidragon，若圖中專案名稱的值被寫成cdn-taichidragon，這就是被寫死的情形。

![image.png](/.attachments/image-842dd193-6d77-4d95-951b-2269114f2089.png)

[備註] 
![image.png](/.attachments/image-6bb6f264-d71d-4e67-a7d4-af1a9cab7f85.png)
![image.png](/.attachments/image-18fedd5b-6a77-404e-a29b-09d7be8cf271.png)
<br>
<Step3> 都確認完之後，就可以把要更新的流程複製到其他cdn及config的專案中哩。 

![image.png](/.attachments/image-d15d9ddc-a43c-46ab-b146-c2156da55a14.png)
![image.png](/.attachments/image-3875223c-2d53-40ff-88dd-22d56ac71c7a.png)


<br>
<Step4> 到複製的目的專案中，檢查是否Copy成功

![image.png](/.attachments/image-91963c1e-9a9e-4347-b844-3d1f462950a7.png)


<br>
<Step5> 確認成功Copy後，此時新增的流程會被排在最後面，這時候要再更改正確的順序。

![image.png](/.attachments/image-93e473cc-e464-454b-93b6-51a62b7f393f.png)
![image.png](/.attachments/image-ec9b85bb-7f18-4f3e-a83e-92d5876cad42.png)
![image.png](/.attachments/image-6d97e194-4ec3-42bc-95fa-1ab5377dad88.png)
<hr>
排序完成後按下Done並儲存，確認進程的順序無誤後，這樣就更新成功囉。

![image.png](/.attachments/image-85069614-594a-40a9-8014-5ec9ff88cf43.png)

只不過，現在只更新完一個專案，要記得所有cdn及config的專案都要更新唷!
#END
