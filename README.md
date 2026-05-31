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

### 標準差和熵統計
