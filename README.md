# 樂透號碼統計分析專題

此專案透過 Java 程式統計過往樂透資料，選出出現次數最多的六個號碼，並將結果儲存至 Microsoft SQL Server 資料庫中。專案同時附有建立資料庫 Table 的 SQL 檔案。

## 專案背景
樂透資料通常為隨機出號，但透過大量歷史資料統計，仍能找出部分常見的號碼。本專案使用隨機產生 100,000 筆樂透數字（範圍 1~42）作為模擬資料，計算各數字出現頻率，並依頻率排序選出前六名，最後將結果插入資料庫中的 Table_1 表格。

---

## 環境需求
- Java 開發環境
  - JDK 1.8 或以上版本
- 資料庫
  - Microsoft SQL Server
  - 資料庫名稱：tiffany
  - 使用者：watcher
  - 密碼：P@ssw0rd
- JDBC Driver
  - 請下載並加入 Microsoft SQL Server JDBC Driver 至 classpath。

---

## 專案結構

```scss
├── README.md              // 本說明文件
├── src
│   └── tw
│       └── tiffany
│           └── jdbc
│               └── hw_1.java      // 主程式，進行資料統計與資料庫操作
└── sql
    └── create_table.sql   // 建立資料庫 Table_1 的 SQL 指令
```

---
## 程式說明
Java 程式 (hw_1.java)
- **資料庫連線**:
  1. 利用 JDBC 連線至 SQL Server，驗證連線狀態後建立查詢與更新。
- **統計機制**:
  1. 產生 100,000 個隨機數字，數值介於 1 至 42。
  2. 使用兩個陣列 tnum 與 t2num 分別統計每個數字出現次數，並進行排序。
  3. 從排序後的結果中選出出現次數最多的六個數字，並呼叫 processStoreAction 方法進行資料庫寫入。
- **資料儲存**:
  1. 將選出的數字透過 PreparedStatement 寫入 Table_1 表格。
- **額外功能**:
  1. 提供 createMemberDao 方法，回傳 IMemberDao 介面的實作 (依賴外部程式碼 MemberDaoJdbcImpl，此部分用於擴充其他會員相關功能)。

---

## SQL 檔案 (create_table.sql)
提供建立資料庫表格 Table_1 的指令，資料型態為 nvarchar(50) 用於儲存樂透號碼。
```sql
USE [tiffany]
GO

SET ANSI_NULLS ON
GO

SET QUOTED_IDENTIFIER ON
GO

CREATE TABLE [dbo].[Table_1](
	[number] [nvarchar](50) NULL
) ON [PRIMARY]
GO
```

---

## 安裝與執行步驟
1. 建立資料庫表格
- 在 SQL Server Management Studio 中執行 sql/create_table.sql 指令，建立 Table_1 表格。
2. 設定專案
- 確保已安裝 JDK 並下載 Microsoft SQL Server JDBC Driver，並加入專案的 classpath 中。
- 根據實際資料庫連線資訊調整 Java 程式中的連線字串。
3. 編譯與執行
- 編譯 Java 程式：
  ```bash
  javac -d bin src/tw/tiffany/jdbc/hw_1.java
  ```
- 執行主程式:
  ```bash
  java -cp bin;[path_to_jdbc_driver] tw.tiffany.jdbc.hw_1
  ```
4. 檢查結果
- 執行後，程式會在命令列印出選出的樂透號碼以及資料庫操作狀態。
- 可在 SQL Server 中查詢 Table_1，確認資料是否正確寫入。

---

## 注意事項與建議
- 錯誤處理:
  - 目前程式中主要以拋出 Exception 處理錯誤，建議在實際應用中加入更細緻的錯誤處理與資源釋放機制 (例如使用 try-with-resources)。

- 資料邏輯:
  - 程式統計的邏輯使用了兩個陣列來計數與排序，未來可考慮使用 Collection 或 Map 結合排序的方式，讓程式邏輯更清晰。
- 擴充功能:
  - 已提供 IMemberDao 介面與 MemberDaoJdbcImpl 實作的參考點，可根據需求擴充會員管理相關功能。

---

## 結語
本專案以簡單的統計方式展示如何從大量樂透資料中選出熱門號碼，並結合 JDBC 技術進行資料庫操作。歡迎大家提出建議與改進，讓程式碼更穩定、結構更完善。
若有任何問題，歡迎在 Issue 中討論或提交 Pull Request。
