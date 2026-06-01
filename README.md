# 開發基於數位繪畫之多模態焦慮程度評估系統 A Multimodal Deep Learning Framework for Anxiety Level Assessment Using Digital Drawing 
**背景** ：焦慮症是常見的精神疾病之一，若未及時治療，將嚴重影響患者日常生活功能。然而焦慮症的臨床評估，可能會因為患者情緒詞彙不足或防衛性心態，以致量表或晤談的信效度有所受限。繪畫測驗在臨床醫學領域被視為診斷輔助工具之一，近年亦有機器學習研究將受試者的繪畫成品作為心理狀態評估依據，正確率約80%。  

**問題與假說** ：過去研究資料來源為單一模態，為提升準確度，本研究假設除了靜態繪畫作品中存在情緒狀態特徵外，可與動態墨水數據及訪談語意資料結合，利用多模態深度學習模型，準確推斷作畫者的焦慮程度。

**研究目標** ：結合數位繪畫相關之多模態特徵，開發深度學習模型之焦慮程度評估系統。  

**實驗設計**：擬招募120位18歲到40歲受試者，將蒐集受試者焦慮相關量表，並在數位繪板進行畫人測驗、房樹人測驗及畫鐘測驗後，接受結構式訪談。繪圖成品特徵、動態墨水數據與訪談語意特質等資訊，將提供多模態Transformer深度學習模型進行訓練，最後採用交叉驗證提供焦慮程度評估成效。  

**重要性**：此系統將有助於提供自動化與客觀之焦慮程度初步診斷參考依據，並可望推廣於一般民眾居家使用，作為自我情緒狀態評估與追蹤的日常工具，進而提高心理健康意識。
## 實驗模型架構圖
<img width="1339" height="533" alt="螢幕擷取畫面 2026-05-21 215808" src="https://github.com/user-attachments/assets/3e8458aa-5d1b-4dd5-9135-07ca0f7085d4" />

## 畫人測驗
**以下分析以陳牧宏醫師於2025年所蒐集之15個個案為主**
### 時間光譜圖
**為了可以看到成品的筆畫順序，我打算利用程式產生timestamp時間光譜圖，讓筆畫的顏色隨時間變化**  


第一次測試：DAP_Timestamp_v1.py   
<img width="330" height="576" alt="01_temporal_spectrum" src="https://github.com/user-attachments/assets/ade5d356-1955-4cec-99d7-25bb890c9cc7" />   
程式產生了以上範例圖片以及 **ink time(筆畫時間) & think time(思考時間) & total time** 的Excel  
但因為圖中包含到了受試者擦拭掉的筆畫，因此需要做第二次的修改  

第二次測試：  
<img width="256" height="512" alt="01_spectrogram_without_erased" src="https://github.com/user-attachments/assets/b0c2e9bd-2f65-4759-969c-422343ebc1ba" />    
圖確實刪除掉擦拭的筆畫，但筆畫產生的線條太粗，且圖畫顛倒，因此需要做第三次的修改  

第三次測試：  
<img width="326" height="512" alt="01_spectrogram_without_erased" src="https://github.com/user-attachments/assets/cd1f992c-94fd-4950-850e-7c643c10d3b3" />  
圖筆畫確實變細且回正方向，但光譜時間軸標示不明確，因此需要做第四次的修改  

第四次測試：DAP_Timestamp_v2.py  
<img width="330" height="576" alt="01_temporal_spectrum_without_erased" src="https://github.com/user-attachments/assets/6b77ea86-5173-4f29-8636-7873e28b3ea5" />  
順利解決時間軸問題  

### 墨水數據統計  
**為了可以看到繪畫過程的各項墨水資訊及統計數值，我利用程式產生一Excel檔**  
DAP_Statistical_Analysis.py  
<img width="145" height="505" alt="螢幕擷取畫面 2026-04-27 095503" src="https://github.com/user-attachments/assets/75a98c78-52f9-4ecd-bdf6-701c7bfde302" />  
以上為範例圖片，墨水數據包含時間、座標、筆壓、傾角、速度、筆畫數、擦拭筆畫數等等，都有被記錄下來    

### 標準差和熵統計
**Zhang 等人於2024年的文章「Feasibility study on using house-tree-person drawings for automatic analysis of depression」提到，熵很重要，可以反應筆壓的整體分佈混亂程度，於是我把筆壓的標準差和熵做簡單的統計**  

DAP_Pressure_Analysis.py  
第一次測試：  
<img width="417" height="295" alt="DAP_Effective_Pressure_Summary" src="https://github.com/user-attachments/assets/b5500dfa-9643-48d7-b956-ebecba637d9b" />  
我參考文獻中的有效像素範圍 [9 - 248]，將筆壓範圍0-1，正規化至9-248  
但每一個subject 不須區分顏色，因此需做修改  

第二次測試：  
<img width="417" height="295" alt="DAP_Effective_Pressure_Summary" src="https://github.com/user-attachments/assets/08416ee7-8001-421e-8ae9-98b0db41b0e7" />  
柱狀圖顏色單一，但既然有筆壓數值，便不須正規化，直接用0-1分析即可，因此需做修改  

第三次測試：  
<img width="1072" height="712" alt="DAP_Pressure_Distribution_Original" src="https://github.com/user-attachments/assets/b7e02a70-88db-4ac3-8eea-c08ce3aecf0f" />  
<img width="417" height="295" alt="DAP_Pressure_Summary_Original" src="https://github.com/user-attachments/assets/e9164e3b-d002-471d-88ad-9d3d7267a9e7" />  
直接使用筆壓墨水數據分析  

### Object detection  
**接著為了能自動化辨識物件，我打算用YOLOv8做物件偵測，看哪裡有畫，偵測到之後再交由其他模型識別畫了什麼，我打算分別做兩件事情:**  
**1.將15個受試者的DAP圖框出每一個部位的bounding box，並計算框框內的各項指標(參考我們列出之與焦慮相關繪畫特徵來實作)**  
**2.利用網路上的公開資料集，手工框出每一個DAP的每一個部位的bounding box，讓模型訓練能否自動找出哪裡是圖畫**  

資料集: https://www.kaggle.com/datasets/lachin007/drawaperson-handdrawn-sketches-by-children  
圖片標記工具: https://roboflow.com/  

#### Task 1:  
我先使用 Roboflow，將15個受試者的DAP成品每個身體部位進行框選，類別有頭、眼睛、鼻子、嘴巴、耳朵、脖子、身體、手臂、手、腿和腳  
再來我將標註好的圖片輸出出YOLOv8必備的檔案(圖片檔、與圖片檔同名之文字檔作為標註檔、資料集設定檔yaml、readme檔)，透過python將有bounding box的圖片產生出來。我統計每個受試者，每個物件的左上方座標點及框的長寬、每個受試者的Object數量、每個類別物件在所有受試者中的總數量。  

程式: DAP_BoundingBox_Analysis.py  

以下為範例結果:  
<img width="1152" height="618" alt="01_annotated" src="https://github.com/user-attachments/assets/0d9c13a4-ed6e-465a-9f5e-a20f36b39481" />  
<img width="1152" height="618" alt="15_annotated" src="https://github.com/user-attachments/assets/0cd54da6-444d-4d68-a778-aa8490f8cccf" />  
<img width="853" height="320" alt="螢幕擷取畫面 2026-06-01 152630" src="https://github.com/user-attachments/assets/a585577c-4cb5-44a6-8fbe-48c2fa05487f" />  
<img width="347" height="339" alt="螢幕擷取畫面 2026-06-01 152813" src="https://github.com/user-attachments/assets/a8392e59-369f-4136-ae53-f32ba683afe9" />  
<img width="764" height="254" alt="螢幕擷取畫面 2026-06-01 152458" src="https://github.com/user-attachments/assets/e75b0376-fff9-41c7-bdf0-14a0cad9f342" />

接下來我打算用這些資訊，進一步計算Bounding box中的各個墨水指標以及與焦慮相關之特徵數值。  


#### Task 2:  
我先將資料集下載下來，並選擇410張圖片作為訓練用資料，將它們重新命名。之後使用 Roboflow，將410幅DAP成品每個身體部位進行框選，類別有頭、眼睛、鼻子、嘴巴、耳朵、脖子、身體、手臂、手、腿和腳
