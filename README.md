# week5

# Week5

## Task2-1 建立資料庫

```sql
SELECT * FROM website.member;
```

## Task2-2 建立資料表

```sql
CREATE TABLE member (

    ->     id BIGINT PRIMARY KEY AUTO_INCREMENT,

    ->     name VARCHAR(255) NOT NULL,

    ->     username VARCHAR(255) NOT NULL,

    ->     password VARCHAR(255) NOT NULL,

    ->     follower_count INT UNSIGNED NOT NULL DEFAULT 0,

    ->     time DATETIME NOT NULL DEFAULT CURRENT_TIMESTAMP

    -> );
```

id：PK，自動遞增。
name：字串，不能為空。
username：字串，不能為空。
password：字串，不能為空。
follower_count：非負數的整數，不能為空，預設為 0。
time：日期，不能為空，預設為建立時間。

## Task3-1 選擇資料庫，並插入資料

插入資料語法：

```sql
INSERT INTO 資料表(columns) VALUES(內容);
```

Code：

```sql
USE website;
INSERT INTO member(name, username, password) VALUES('test','test','test');

INSERT INTO member(name, username, password, follower_count, time)
VALUES('Joey', 'Joey', 'Joey123', 100000000, '2022-6-18 00:00:00');

INSERT INTO member(name, username, password, follower_count, time)
VALUES('Andy', 'Andy', 'Andy123', 12345, '2021-2-27 00:00:00');

INSERT INTO member(name, username, password, follower_count, time)
VALUES('Sandy', 'Sandy', 'Sandy123', 33333, '2022-8-1 12:35:37');

INSERT INTO member(name, username, password, follower_count, time)
VALUES('William', 'William', 'William123', 1247, '2021-11-20 16:30:42');
```

![結果：](./Result/Task3-1InsertData.jpg)

## Task3-2 顯示所有資料

query 語法：

```sql
SELECT * FROM 資料表;
```

Code：

```sql
SELECT * FROM member;
```

![結果：](./Result/Task3-2-SelectAll.jpg)

## Task3-3 顯示所有 member 資料，透過時間排序

query 語法：

```sql
SELECT 顯示的欄位 FROM 資料表 order by 欄位 DESC;
```

Code：

```sql
SELECT * FROM website.member order by time desc;
```

![結果：](./Result/Task3-3-TimeSortDesc.jpg)

## Task3-4 顯示第 2 到 4 筆資料

query 語法：

```sql
SELECT * FROM 資料表 limit 數字(顯示數量) OFFSET 數字(第幾筆之後);
```

OFFSET 的數值從 0 開始，因此從第二筆開始顯示，是 OFFSET 1，類似於 array 的 index 從 0 的概念。

Code：

```sql
SELECT * FROM member limit 3 OFFSET 1;
```

![結果：](./Result/Task3-4-2to4.jpg)

## Task3-5 單一條件查詢，顯示 Username 等於 Test 的資料

query 語法：

```sql
SELECT * FROM 資料表 where 條件;
```

Code：

```sql
SELECT * FROM member where username = "test";
```

![結果：](./Result/Task3-5UsernameTest.jpg)

## Task3-6 多重條件查詢，顯示 Username 和 Password 都是 test 的資料

```sql
SELECT * FROM 資料表 where 條件1 and 條件2 ;
```

Code：

```sql
SELECT * FROM website.member where username = "test" and password = "test";
```

![結果：](./Result/Task3-6-UsernameAndPassword.jpg)

## Task3-7 更新資料，將 test 改為

```sql
UPDATE 資料表 SET 更改欄位 where 更改條件;
```

Code：

```sql
UPDATE member SET username = 'test2' where username = 'test';
```

![結果：](./Result/Task3-7-UpdateTest.jpg)

## Task4-1 計算資料量總和，計算 member 數總和

```sql
SELECT count(*) FROM 資料表;
```

Code：

```sql
SELECT count(*) FROM member;
```

![結果：](./Result/Task4-1-memberCount.jpg)

## Task4-2 資料內容總和，計算 follower 內容總和

語法：

```sql
SELECT SUM(欄位) FROM 資料表;
```

Code：

```sql
SELECT SUM(follower_count) FROM member;
```

![結果：](./Result/Task4-2-followerCount.jpg)

## Task4-3 計算平均數，計算 follower_count 內容平均

語法：

```sql
SELECT AVG(欄位) FROM 資料表;
```

Code：

```sql
SELECT FORMAT(AVG(follower_count),0) FROM member;
```

FORMAT 可以設定小數點第幾位。

![結果：](./Result/Task4-3-AVG.jpg)

## Task5-1 合併查詢，取得所有留言並顯示留言者的姓名

新增資料：

```sql
INSERT INTO message(member_id, content, like_count)
VALUES(3, 'Hi here is Joey', 99999);
INSERT INTO message(member_id, content, like_count)
VALUES(1, 'Hi here is Andy', 10000);
INSERT INTO message(member_id, content, like_count)
VALUES(2, 'Hi here is Test', 0);
INSERT INTO message(member_id, content, like_count)
VALUES(4, 'Hi here is Sandy', 50000);
INSERT INTO message(member_id, content, like_count)
VALUES(5, 'Hi here is William', 100000);
INSERT INTO message(member_id, content, like_count)
VALUES(3, 'Hi here is Joey second', 20000);
INSERT INTO message(member_id, content, like_count)
VALUES(2, 'Hi here is Joey second', 20000);
INSERT INTO message(member_id, content, like_count)
VALUES(5, 'Hi here is William second', 200000);
INSERT INTO message(member_id, content, like_count)
VALUES(1, 'Hi here is Test second', 10);
INSERT INTO message(member_id, content, like_count)
VALUES(2, 'Hi here is Joey third', 30000);
INSERT INTO message(member_id, content, like_count)
VALUES(2, 'Hi here is Joey fourth', 40000);

```

加入 Foreign Key：

```sql
ALTER TABLE message ADD CONSTRAINT 'member_fk' ADD FOREIGN KEY('member_id') REFERENCES memeber('id');
```

JOIN 語法：

```sql
SELECT 資料表.欄位, 資料表.欄位 FROM 左表
JOIN 右表 ON 條件
order by 欄位;
```

Code：

```sql
SELECT member.name, message.content FROM message JOIN member ON message.member_id = member.id order by member_id;
```

![結果：](./Result/Task5-1-JOINALL.jpg)

## Task5-2 多重條件合併查詢，查詢 test 的所有留言

Code：

```sql
SELECT member.name, message.content FROM member
JOIN message on member.id = message.member_id
and member.name = 'test';
```

![結果：](./Result/Task5-2-JOINTEST.jpg)

## Task5-3 合併查詢搭配 Aggregate Fn，取得 member 資料表中欄位為 test 的所有留言，平均按讚數

```sql
SELECT member.name,AVG(message.like_count) AS average_likes
From member
JOIN message on member.id = message.member_id
and member.name = 'test'
GROUP BY member.name;
```

GROUP BY 可以將同個 Column 中的指定內容，做區別計算。
![結果：](./Result/Task5-3-like_AVG.jpg)
