---
title: ClickHouse
tags:
  - OLTP
categories:
  - Develop
date: 2025-04-21
draft: true
---

- 多版本並行控制 (MVCC - Multi-Version Concurrency Control): 這是 PostgreSQL 的一大特色。了解 MVCC 如何在沒有讀取鎖定 (read locks) 的情況下，允許多個程序同時存取和修改共享資料，有效地解決了鎖定、阻塞和死鎖等並行問題。
- 事務與 ACID 標準 (Transactions & ACID): 掌握事務的概念，以及 PostgreSQL 如何確保資料的原子性 (Atomicity)、一致性 (Consistency)、隔離性 (Isolation) 和持久性 (Durability)。
- 函數與程序語言 (Functions & Procedural Languages):內建函數: 熟悉常用的內建 SQL 函數。PL/pgSQL: 學習使用程序語言編寫自定義函數和存儲過程，實現複雜的業務邏輯。


## Value Type

### JSON vs JSONB

| 特性       | JSON 類型 (JSON)                                                                         | JSONB 類型 (JSONB)                                                                                 |
| ---------- | ---------------------------------------------------------------------------------------- | -------------------------------------------------------------------------------------------------- |
| 儲存方式   | 儲存為原始文本字串。                                                                     | 儲存為二進制 (Binary) 格式。                                                                       |
| 插入/寫入  | 更快。只需存儲原始文本。                                                                 | 較慢。需要解析並轉換為二進制格式。                                                                 |
| 查詢/讀取  | 較慢。每次查詢時都需要解析文本。                                                         | 更快。已預先解析，可立即操作和索引。                                                               |
| 空間效率   | 可能略大，因為儲存了所有空白和重複鍵。                                                   | 更緊湊，因為重複的鍵不會重複儲存。                                                                 |
| 排序與細節 | 保留元素和鍵的原始順序、重複的鍵和空白。                                                 | 不保留鍵的原始順序、不保留空白。重複的鍵會被捨棄，只保留最後一個。                                 |
| 索引       | 只能做全文索引。                                                                         | 支援更強大的索引 (如 GIN/GiST)，實現快速鍵/值查找。                                                |
| 應用       | 視為一個不可變的文本塊，並且必須保留 JSON 中鍵的原始順序或格式細節（例如作為審計日誌）。 | 經常查詢、處理和修改 JSON 資料，並且需要對 JSON 內的鍵值進行高效索引。這是生產環境中最常用的類型。 |


#### Index ans Search Example

**Table: products**

|id|name |tags |
|---|---|---|
|1|筆記型電腦 A| {"顏色": "銀色", "尺寸": "13吋", "品牌": "XYZ", "特點": ["輕薄", "高效能"]}|
|2|智慧型手機 B| {"顏色": "黑色", "尺寸": "6.1吋", "品牌": "ABC", "特點": ["防水", "高畫質相機"]}|
|3|平板電腦 C| {"顏色": "白色", "尺寸": "10吋", "品牌": "XYZ", "特點": ["電池續航長", "4G連網"]}|
|4|智慧手錶 D| {"顏色": "太空灰", "特點": ["心率監測", "GPS"]}|


- 對 jsonb 建 index： CREATE INDEX idx_products_tags_gin ON products USING GIN (tags);

- jaonb 搜尋，： 
    - check key：SELECT * FROM table WHERE details_jsonb ? 'color';
    - check @> (contains key and value)：SELECT * FROM table WHERE details_jsonb @> '{"price": 100}';
    - selcet key '特點'， array contain '輕薄':SELECT name, tags FROM products WHERE tags -> '特點' @> '["輕薄"]';

### TEXT

- TEXT 效能考量
索引： 如果您需要在 TEXT 欄位上進行高效的相等性或前綴匹配查詢，可以創建 B-tree 索引。

- 全文搜尋： 如果您需要搜尋 TEXT 欄位中的關鍵字或片語，強烈建議使用 PostgreSQL 的全文搜尋 (Full-Text Search) 功能，而不是使用 LIKE '%...%' 這樣的低效模式匹配。

### 使用範例
```
CREATE TABLE articles (
    id SERIAL PRIMARY KEY,
    title TEXT,
    body TEXT,
    -- 這是用於全文搜尋的欄位
    search_vector TSVECTOR 
);

-- 這裡使用 'english' 配置
UPDATE articles 
SET search_vector = to_tsvector('english', title || ' ' || body);

-- 在 search_vector 欄位上建立 GIN 索引
CREATE INDEX idx_fts_article ON articles USING GIN (search_vector);

-- 步驟三：執行搜尋查詢 (Query)
SELECT id, title, body 
FROM articles
WHERE 
    -- 使用 'english' 配置將搜尋字串轉換為 tsquery
    search_vector @@ to_tsquery('english', 'database & performance & optimization');
    
-- & (AND): 必須包含所有詞彙。
-- | (OR): 包含任一詞彙。
-- ! (NOT): 排除某個詞彙。
-- <-> (FOLLOWED BY): 詞彙必須按順序緊跟。

-- 步驟四：結果排序與排名 (Ranking)
SELECT id, title, 
    ts_rank(search_vector, to_tsquery('english', 'database & performance')) AS rank_score
FROM articles
WHERE 
    search_vector @@ to_tsquery('english', 'database & performance')
ORDER BY 
    rank_score DESC  -- 按分數降序排列
;

步驟五：中文全文搜尋 (Chinese Full-Text Search)
PostgreSQL 內建的 FTS 擴展對中文（以及其他東亞語言）支援較弱，因為中文是沒有空格分隔的連續文字。

要實現高效的中文 FTS，您通常需要安裝和使用外部擴展 (External Extensions) 進行分詞，例如：

pg_jieba (基於 Jieba 結巴分詞): 這是目前最流行的 PostgreSQL 中文分詞擴展之一。

zhparser: 另一個常用的中文分詞擴展。

```


## 規則 vs. 觸發器 (Rules vs. Triggers): 

舉例： 如果 tableA 有 insert, update, delete 的異動 (Rules)，會觸發 tableA_log 新增修改記錄 (Trigger Function)
trigger 可以在 Rules 發生前或後執行，也可以設定是每一個 record 都要執行一次或 SQL 語法 run 完再執行


```
-- 建立 function
CREATE OR REPLACE FUNCTION animal_name_if_changed()
    RETURNS trigger AS --return 設定爲 trigger
$$
BEGIN
    IF NEW.name <> OLD.name THEN
    INSERT INTO animals_log (
        animals_id,
        change_time,
        old_name,
        new_name)
    VALUES
        (OLD.id,
         now(),
         OLD.name,
         NEW.name);
    END IF;
    RETURN NEW;
END;
$$ LANGUAGE plpgsql;

-- 觸發條件設定
CREATE TRIGGER animals_update
  AFTER UPDATE
  ON animals
  FOR EACH ROW
  EXECUTE PROCEDURE animal_name_if_changed();
  
-- FOR EACH ROW 是每一行都會執行
-- FOR EACH STATEMENT 預設的方式，執行這次SQL觸發一次！不管變更多少行資料！
  
```

## partition

- partition 有分成 RANGE 與 LIST

### 主表 ddl 建立 partition
```
CREATE TABLE sales_data (
    sale_id SERIAL,
    sale_date DATE NOT NULL,
    amount NUMERIC
) PARTITION BY RANGE (sale_date);

-- 建立 2025 年的空分區作為對照(如果要塞資料理論上要先建 partition)
CREATE TABLE sales_data_2025
    PARTITION OF sales_data
    FOR VALUES FROM ('2025-01-01') TO ('2026-01-01');
```

### ATTACH partition

```
-- 建子表，schema 需要與主表相同但不用 partition
CREATE TABLE sales_data_2026 (
    sale_id SERIAL,
    sale_date DATE NOT NULL,
    amount NUMERIC
)；

-- 將子表變成主表的 partition 
ALTER TABLE sales_data
ATTACH PARTITION sales_data_2026    
FOR VALUES FROM ('2026-01-01') TO ('2027-01-01');

```
