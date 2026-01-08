# RoommateSelectionPlatform
宿舍室友互選Web系統，讓學生能根據生活習慣、作息時間、興趣等條件，自主尋找並組隊適合的室友，提升宿舍生活品質。

## 功能特色

**個人檔案填寫與編輯**：詳細記錄作息時間、冷氣溫度偏好、吸菸習慣、是否常帶人進房、打電話頻率、國籍、聯絡方式與自我介紹。  
**徵友條件設定**：明確指定對未來室友的接受程度（如吸菸、帶人進房等）。  
**學生瀏覽與配對**：以表格形式快速比較「個人情況」與「徵友條件」，方便篩選潛在室友。  
**組隊請求系統**：主動送出邀請、接收他人請求、同意或拒絕，並自動檢查宿舍房型人數上限。  
**即時通知與Email提醒**：使用Bootstrap Alert顯示最新通知，同時透過PHPMailer寄送HTML格式郵件。  
**離開隊伍功能**：不滿意可隨時單獨退出，系統自動建立新隊伍。  
**身分遮蔽機制**：真實學號經單向演算法轉換為masked_id，保護隱私。  
**管理員批量匯入**：支援快速新增學生資料並自動產生帳號與隊伍。


## 資料庫結構
<img width="1221" height="801" alt="image" src="https://github.com/user-attachments/assets/a5ada020-acaa-4bfc-a5a8-3dc4912e6971" />


## 運行環境  
PHP 7.4  
MySQL 5.7  

## 安裝與部署
將所有檔案上傳至伺服器根目錄。  
匯入資料庫檔案 rsp(2)(2).sql 至 MySQL 資料庫 rsp。  
修改 connect.php 中的資料庫連線資訊（目前為硬編碼，請依實際環境調整）。  
設定 Nginx 指向根目錄，並確保 PHP 正常運作。    
管理員帳號：admin，初始密碼請參考資料庫 user 表或自行設定。     
學生首次登入密碼為 masked_id 的 MD5 值（由管理員匯入時自動產生）。  

## 注意事項
本系統為測試／開發階段，程式碼中保留部分除錯輸出且存在安全風險  
 
 
Developed with ♥︎ by Jaxx  
Powered by Bootstrap, PHPMailer, and Jin Xuan Oolong.  
本項目使用 [PHPMailer](https://github.com/PHPMailer/PHPMailer) ，基於 LGPL-2.1 授權。
