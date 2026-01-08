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

### 1. student 表  

| 欄位名稱                  | 允許空 | 預設值 | 資料類型       | 說明                                                                 |
|---------------------------|--------|--------|----------------|----------------------------------------------------------------------|
| s_id                      | 否     |        | varchar(8)     | 學號，例如 a1115555                                                  |
| line_id                   | 是     | NULL   | varchar(50)    | Line ID，例如 @demouser                                              |
| tg_us                     | 是     | NULL   | varchar(50)    | Telegram username，例如 @demouser                                    |
| ig_us                     | 是     | NULL   | varchar(50)    | Instagram username，例如 @demouser                                   |
| discord_us                | 是     | NULL   | varchar(50)    | Discord username，例如 @demouser                                     |
| roomtype                  | 是     | NULL   | varchar(2)     | 宿舍類型，OB/OA=學一，OE/OF=學二，B1=綜合                             |
| self_intro                | 是     | NULL   | text           | 使用者自己填寫的自我介紹                                             |
| t_id                      | 是     | NULL   | int(11)        | 如果有組隊，隊伍的 t_id，無時=0                                      |
| is_smoke                  | 是     | NULL   | tinyint(1)     | 吸菸=1，不抽=0                                                        |
| want_smoke                | 是     | NULL   | tinyint(4)     | 不想要抽菸的室友=0，無所謂=1，想要吸菸的室友=1                        |
| is_bring_people           | 是     | NULL   | tinyint(1)     | 經常帶人進房間=3，偶爾帶=2，最少帶=1，完全不帶=0                     |
| want_bring_people         | 是     | NULL   | tinyint(4)     | 最高接受室友帶人進房間的程度：經常=3，偶爾帶=2，最少帶=1，完全不帶=0 |
| sleep_time_weekend        | 是     | NULL   | time           | 假日放假的話，幾點睡                                                |
| want_sleep_time_weekend   | 是     | NULL   | time           | 想要找假日放假的話，幾點睡的室友，無所謂=17:55:55                     |
| sleep_time_monday         | 是     | NULL   | time           | 上課的話，幾點睡                                                    |
| want_sleep_time_monday    | 是     | NULL   | time           | 想要找上課的話，幾點睡的室友，無所謂=17:55:55                         |
| country                   | 是     | NULL   | varchar(10)    | 國籍，由使用者填寫                                                   |
| want_country              | 是     | NULL   | varchar(10)    | 可以接受的室友的國籍，使用者填寫                                     |
| phone_call                | 是     | NULL   | tinyint(4)     | 經常在房間打電話=3，偶爾打=2，最少打=1，完全不打=0                   |
| want_phone_call           | 是     | NULL   | tinyint(4)     | 最高接受室友在房間打電話的程度：經常=3，偶爾=2，最少=1，完全不打=0   |
| department                | 是     | NULL   | int(2)         | 自己的學系編號，例如電機系=51                                        |
| want_department           | 是     | NULL   | varchar(30)    | 想要室友是什麼學系，使用者填寫                                       |

### 2. user 表

| 欄位名稱           | 允許空 | 預設值 | 資料類型       | 說明                                      |
|--------------------|--------|--------|----------------|-------------------------------------------|
| username           | 否     |        | varchar(8)     | 使用者名稱，管理員=admin，學生=[學號]      |
| password_salted    | 否     |        | varchar(64)    | 加鹽哈希保存        |


### 3. room 表  

| 欄位名稱   | 允許空 | 預設值 | 資料類型      | 說明                                              |
|------------|--------|--------|---------------|---------------------------------------------------|
| roomtype   | 否     |        | varchar(2)    | 宿舍類型，OB/OA=學一，OE/OF=學二，B1=綜合         |
| r_id       | 否     |        | int(11)       | 房號，例如 101                                    |
| t_id       | 是     | NULL   | int(11)       | 宿舍被哪隊佔用了，未被佔用=NULL                   |

### 4. team 表  

| 欄位名稱   | 允許空 | 預設值 | 資料類型       | 說明                          |
|------------|--------|--------|----------------|-------------------------------|
| t_id       | 否     |        | int(11)        | 隊伍編號                      |
| s1         | 否     | 0      | varchar(8)     | 隊員1的學號，無則為0          |
| s2         | 否     | 0      | varchar(8)     | 隊員2的學號，無則為0          |
| s3         | 否     | 0      | varchar(8)     | 隊員3的學號，無則為0          |
| s4         | 否     | 0      | varchar(8)     | 隊員4的學號，無則為0          |

### 5. waiting_join 表  

| 欄位名稱     | 允許空 | 預設值 | 資料類型       | 說明                                                                                  |
|--------------|--------|--------|----------------|---------------------------------------------------------------------------------------|
| w_id         | 否     |        | int(11)        | 等待的編號                                                                            |
| w_student    | 否     |        | varchar(8)     | 被等待加入隊伍的學生的學號                                                            |
| t_id         | 否     |        | int(11)        | 被 w_student 等待加入的隊伍的編號                                                     |
| t_s1         | 否     | 0      | varchar(8)     | 已於隊伍中的學生的學號，同意加入後改為0                                               |
| t_s2         | 否     | 0      | varchar(8)     | 同上（實務上 t_s1/t_s2 不一定順序）                                                   |
| t_s3         | 否     | 0      | varchar(8)     | 同上                                                                                  |

### 範例說明
- A1119944 申請加入隊伍27，請求現有隊員 A1119933 及 A1119922 同意  
  → `w_student = A1119944`, `t_s1 = A1119922`, `t_s2 = A1119933`

- A1119977 邀請 A1119866 組隊（兩人都無隊伍）  
  → 系統創建新隊伍 t_id=47，`w_student = A1119866`, `t_s1 = A1119866`, `t_id = 47`

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
本系統仍處開發階段，代碼存在除錯輸出且含安全風險。  
 
 
Developed with ♥︎ by Jaxx  
Powered by Bootstrap, PHPMailer and Jin Xuan Oolong.  
本項目使用 [PHPMailer](https://github.com/PHPMailer/PHPMailer) ，基於 LGPL-2.1 授權。
