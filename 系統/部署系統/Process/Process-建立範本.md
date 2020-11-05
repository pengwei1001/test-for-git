#<center>流程範本的建立</center>
<hr>

##建立流程範本說明
<br>
<ol>
<li> 前言 : 上篇【 Process更新 】有介紹流程更新，在閱讀本篇之前，建議先瞭解如何更新流程。
<li> 概要 : 
<br>
在【 Process更新 】有說明，當新流程要加入時，我們需要對Process做更新。那假如需要修改流程內的內容 ( 比如流程的名稱、流程內的程式碼等等 ) ，<br>想想看，是不是就要去有這個流程的每項專案裡，對該流程做同樣的更動 ? 
<br>
答案是要去每項專案的那個流程做同樣的修改，否則其他專案的該流程就等於沒有更新。這樣真的很耗費時間，所以本篇要介紹流程範本，流程範本可以讓我們省事很多，本次範例會以上篇新增的show-logs流程作範例，請繼續往下看。
<br>
<hr>

<li> 實例解說與步驟說明 : 
<br>
<Step1> 到流程範本的頁面中新增範本。

![image.png](/.attachments/image-043b0c47-1657-4088-b063-1bd1339b2616.png)
![image.png](/.attachments/image-8660002c-a1cc-40b7-a215-471c24febc5b.png)

<br>
<Step2> 為範本取一個名稱，並描述這個範本的用意。

![image.png](/.attachments/image-790f7a41-2f1c-4018-a21b-e99ef6dbc674.png)

<br>
<Step3> 設定範本的功能。這邊可以先依照原流程他原本的設定，然後再去調整需要更動的地方。

![image.png](/.attachments/image-296cc7f4-4d57-4b18-b597-a4662a47b5ef.png)
--> 說明 : 以本次範例來說，是要對程式碼做修改，所以除了程式碼部分要更動，其他設定就依照原本show-logs的設定即可。

<br>
<Step4> 處理好範本的設定後，接下來就是將範本加到其他專案中。

![image.png](/.attachments/image-32256e5e-9f41-46a3-863a-9ee24d5da284.png)
![image.png](/.attachments/image-b4d64737-8e11-4092-8bd9-fb12bcddfddc.png)

<br>
<Step5> 修改新增的流程內容，並刪除原流程，刪除後將順序更正。

![image.png](/.attachments/image-c405c04c-ac99-43cc-8539-90294c2f8a3e.png)
![image.png](/.attachments/image-be1f9882-cdd4-4e2a-9f2b-a414e53d3141.png)

<br>
<Step6> 檢查順序是否正常

![image.png](/.attachments/image-f04d5738-f655-44e3-9ca6-5eab060adbde.png)

<br>
以上就順利完成範本的建立，並將範本套用在專案上了。接下要說明怎麼應用範本帶來的便利!
</ol>

<hr>

##範本的應用
<br>
<ol>
<li> 讀完上面就會知道，若是修改流程內容，那麼只要有其他專案也有這個流程時，就需進到那個專案的流程裡同步修改。而範本可以解決這個問題，我們只需按更新鈕，就能為各別的專案更新該項流程，繼續引用上面的show-logs來做示範。
<li> 首先，我到show-logs的範本設定中隨便修改一些東西，最後會發現只要有利用這個範本建立流程的專案都會列出來。

![image.png](/.attachments/image-8ee655fa-7950-4824-ba7b-0032f2043795.png)
![image.png](/.attachments/image-fa342285-646a-42ca-8909-ab0ce5772281.png)

<li> 我也可以到其中一個專案去查看，show-logs這個流程是不是需要更新。

![image.png](/.attachments/image-ce6e7d6a-122a-4027-8633-e48e60cabf14.png)

<br>
有了這個範本，我們若要修改流程內容，只需要針對範本修改，然後再按鈕更新至其他專案，如此就不用再去各專案中修改流程內容，可以省去很多時間~
<br><br>以上就是範本的介紹以及瞭解它的便利性，謝謝收看，我們下篇見。
<hr>

#<center>END</center>

