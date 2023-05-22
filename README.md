# Basic Web

## 專案目的

練習Web 開發上常見的功能，如註冊、登入、權限等，使用語言、框架不限。

---

## 開發規範

> ### Git Branch

1. 使用Fork 功能將專案複製到自己的Github 之中
2. 以master 為主要分支，實作各個階段時，建立對應階段的分支，如phase_1、phase_2 等

> ### Git Commit

提交訊息內容必須包含異動代號、內容，如：

- docs: add README.md
- feat: add 新功能

異動代號如下：

- feat: 新增/修改功能
- fix: 修復Bug
- refactor: 重構功能
- perf: 改善效能
- test: 新增/修改測試程式
- style: 修改程式風格(不影響功能)
- docs: 新增/修改文件
- chore: 變更建構程式或輔助工具
- revert: 撤回先前的commit

> ### Docker

- 將專案容器化並部署至Docker
- 每一階段使用同一個version，如第一階段version 為1，第二階段version 為2 以此類推

---

> ## 第一階段

### 需求

- 每個API 請求資料欄位都需要經過驗證
- 每個API 需要有回應代碼(Return Code)和回應訊息(Return Msg)
- 紀錄API 之request 及response
- 使用Swagger 建立RESTful API 文件

### 資料庫規格

- 使用兩種不同類型的資料庫，如MySQL、PostgreSQL
- 資料庫連線不可使用root 帳號

> #### 1. PRODUCT (產品主檔)

| Column      | Type   | Comments | Remark |
|-------------| ------ |----------| ------ |
| ID          | String | 流水號 |
| NAME        | String | 名稱 |
| PRICT       | INT | 售價 |
| AMOUNT      | INT | 數量 |
| STATUS      | String | 狀態 | 
| CREATE_TIME | Date   | 建立時間 | yyyy/MM/dd HH:mm |
| UPDATE_TIME | Date   | 更新時間 | yyyy/MM/dd HH:mm |

> #### 2. API_LOG (API紀錄檔)

| Column      | Type   | Comments | Remark |
|-------------| ------ | -------- | ------ |
| ID          | String | ID |
| REQUEST     | String | 請求本文 |
| RESPONSE    | String | 回覆本文 |
| RETURN_CODE | String | 回覆代碼 |
| RETURN_MSG  | String | 回覆訊息 |
| CREATE_TIME | Date   | 建立時間 | yyyy/MM/dd HH:mm |

### API 規格

> #### 1. 查詢所有產品

|                 | Content |
| --------------- | ------- |
| **URL**         | /product |
| **HTTP Method** | GET |
| **Desc**        | 查詢所有產品 |

> #### 2. 查詢產品

|                 | Content |
| --------------- | ------- |
| **URL**         | /product/{name} |
| **HTTP Method** | GET |
| **Desc**        | 根據名稱查詢產品 |

> #### 3. 新增產品

|                 | Content |
| --------------- | ------- |
| **URL**         | /product |
| **HTTP Method** | POST |
| **Desc**        | 新增產品 |

> #### 4. 更新產品

|                 | Content |
| --------------- | ------- |
| **URL**         | /product |
| **HTTP Method** | PUT |
| **Desc**        | 更新產品 |

> #### 5. 刪除產品

|                 | Content |
| --------------- | ------- |
| **URL**         | /product/{name} |
| **HTTP Method** | DELETE |
| **Desc**        | 根據名稱刪除產品 |

> #### 6. 查詢API 紀錄檔

|                 | Content |
| --------------- | ------- |
| **URL**         | /api/log |
| **HTTP Method** | GET |
| **Desc**        | 查詢API_LOG 資料 |

### 知識點

1. 程式語言與Web 框架特性
2. 常見程式語言與Web 框架使用與比較，如C#/ASP.NET、Java/Spring Boot、Python/Django等
3. 常見資料庫使用與比較，如MySQL、PostgreSQL等
5. Swagger2 及Swagger3 的差異

---

> ## 第二階段

### 需求

- Product RESTful API 都需使用Redis 做快取處理
- API 的回應訊息使用i18n
- 實作JWT 驗證機制
- 使用者必須登入帳號密碼才能取的Token (JWT)
- 機敏資料使用任一加密方法加密
- 使用PGP 加密模擬登入密碼加密機制
- 註冊帳號成功後，寄送認證信至註冊信箱

### 權限設定

- 使用者完成啟用後才可登入帳號
- 登入前，可使用`Product` 相關API，使用其他API 顯示**未登入不可使用**
- 登入後，新增可使用`查詢使用者基本資料`API，使用其他API 顯示**未授權不可使用**
- 登入後，角色權限為ADMIN 才可存取`查詢API 紀錄檔`API

### 資料庫規格

> #### 1. USER (使用者主檔)

| Column      | Type   | Comments | Remark |
|-------------| ------ |----------| ------ |
| USERNAME    | String | 帳號 |
| P_CODE      | String | 密碼 |
| STATUS      | String | 狀態 | Y: 已啟用<br>N: 未啟用 |
| CREATE_TIME | Date   | 建立時間 | yyyy/MM/dd HH:mm |
| UPDATE_TIME | Date   | 更新時間 | yyyy/MM/dd HH:mm |

> #### 2. USER_ROLE (使用者角色檔)

| Column      | Type   | Comments | Remark |
|-------------| ------ |----------| ------ |
| USERNAME    | String | 使用者帳號 |
| ROLE        | String | 角色 |
| CREATE_TIME | Date   | 建立時間 | yyyy/MM/dd HH:mm |

> #### 3. USER_INFO (使用者基本資料檔)

| Column      | Type   | Comments | Remark |
|-------------| ------ | -------- | ------ |
| USERNAME    | String | 使用者帳號 |
| NAME        | String | 名稱 |
| EMAIL       | String | 信箱 |
| PHONE       | String | 電話 |
| CREATE_TIME | Date   | 建立時間 | yyyy/MM/dd HH:mm |
| UPDATE_TIME | Date   | 更新時間 | yyyy/MM/dd HH:mm |

### API 規格

> #### 1. 註冊

|                 | Content |
| --------------- | ------- |
| **URL**         | /user/register |
| **HTTP Method** | POST |
| **Desc**        | 提供使用者註冊帳號 |

> #### 2. 啟用帳號

|                 | Content |
| --------------- | ------- |
| **URL**         | /user/enable |
| **HTTP Method** | POST |
| **Desc**        | 啟用指定帳號 |

> #### 3. 密碼加密

|                 | Content |
| --------------- | ------- |
| **URL**         | /cipher |
| **HTTP Method** | POST |
| **Desc**        | 使用指定加密方式為明文進行加密 |

> #### 4. 登入

|                 | Content |
| --------------- | ------- |
| **URL**         | /user/login |
| **HTTP Method** | POST |
| **Desc**        | 使用帳號及密碼取得Token |

> #### 5. 查詢使用者基本資料

|                 | Content |
| --------------- | ------- |
| **URL**         | /user |
| **HTTP Method** | GET |
| **Desc**        | 依帳號查詢使用者基本資料 |

### 知識點

1. 快取
2. i18n
3. JWT
4. 加解密方法，如單向加密、雙向加密(對稱加密、非對稱加密)
5. 權限設計

---

> ## 第三階段

### 需求

- 設定排程，每天02:00 將未啟用帳號移除
- 建立另一專案，使用任一Message Queue 技術溝通，主要負責寄送認證信
- Redis 改為使用Redis Sentinel
- 使用Docker Compose，執行本專案及相關資料庫、Redis
- 使用任一資料庫版本控管工具

### 知識點

- 排程
- 單體式、分散式和微服務架構
- Redis 高可用性方案(Redis Sentinel)
- Docker Compose

---
