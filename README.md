# MySQL 8.0 インデックスパフォーマンス練習用テーブル
```sql
SELECT COUNT(*) FROM user;
+----------+
| count(*) |
+----------+
| 10000000 |
+----------+
```
### テーブル構造

```sql
CREATE TABLE user (
    id INT AUTO_INCREMENT PRIMARY KEY,
    username VARCHAR(50) NOT NULL,
    email VARCHAR(100) NOT NULL,
    password VARCHAR(255) NOT NULL,
    first_name VARCHAR(50),
    last_name VARCHAR(50),
    date_of_birth DATE,
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    updated_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP,
    status TINYINT DEFAULT 1
);
```

### テーブルのフィールド

| フィールド名     | データ型         | NULL許可 | キー | デフォルト値           | 説明                                    |
|------------------|------------------|----------|-----|-----------------------|---------------------------------------|
| id               | int              | NO       | PRI | NULL                  | ユーザーの一意な識別子                |
| username         | varchar(50)      | NO       |     | NULL                  | ユーザー名                             |
| email            | varchar(100)     | NO       |     | NULL                  | ユーザーのメールアドレス              |
| password         | varchar(255)     | NO       |     | NULL                  | ユーザーのパスワード                  |
| first_name       | varchar(50)      | YES      |     | NULL                  | ユーザーの名                           |
| last_name        | varchar(50)      | YES      |     | NULL                  | ユーザーの姓                           |
| date_of_birth    | date             | YES      |     | NULL                  | 生年月日                               |
| created_at       | timestamp        | YES      |     | CURRENT_TIMESTAMP     | 登録日時（デフォルト）                 |
| updated_at       | timestamp        | YES      |     | CURRENT_TIMESTAMP     | 更新日時（自動更新）                  |
| status           | tinyint          | YES      |     | 1                     | ユーザーの状態（アクティブ/非アクティブ）|

### サンプルデータ

以下は、`user` テーブルのサンプルデータです。

| id     | username | email             | password    | first_name | last_name | date_of_birth | created_at          | updated_at          | status |
|--------|----------|-------------------|-------------|------------|-----------|---------------|---------------------|---------------------|--------|
| 500001 | user0    | user0@example.com | password123 | First0     | Last0     | 2018-11-26    | 2025-02-13 06:25:59 | 2025-02-13 06:25:59 |      1 |
| 500002 | user1    | user1@example.com | password123 | First1     | Last1     | 2024-02-24    | 2025-02-13 06:25:59 | 2025-02-13 06:25:59 |      1 |
| 500003 | user2    | user2@example.com | password123 | First2     | Last2     | 2008-12-03    | 2025-02-13 06:25:59 | 2025-02-13 06:25:59 |      1 |
| 500004 | user3    | user3@example.com | password123 | First3     | Last3     | 2007-01-05    | 2025-02-13 06:25:59 | 2025-02-13 06:25:59 |      1 |
| 500005 | user4    | user4@example.com | password123 | First4     | Last4     | 2013-02-24    | 2025-02-13 06:25:59 | 2025-02-13 06:25:59 |      1 |

### 注意事項
大量のデータを扱う場合、インデックスを適切に設定することで検索性能が向上します。
不要なインデックスは、INSERTやUPDATEのパフォーマンスを低下させる可能性がありますので、慎重に設計してください。

### 練習課題
このREADMEを参考にして、インデックスの追加や他の最適化を行い、パフォーマンスを向上させる練習をしてみましょう。具体的には、以下のようなことに挑戦してみてください。

1. インデックスの追加
email カラムにユニークインデックスを追加して、重複したメールアドレスの登録を防ぎましょう。以下のコマンドを実行してください。
```sql
ALTER TABLE user ADD UNIQUE INDEX idx_unique_email (email);
```
2. 複合インデックスの作成
first_name と last_name の組み合わせに対して複合インデックスを作成し、名前での検索を高速化しましょう。以下のコマンドを実行してください。
```sql
ALTER TABLE user ADD INDEX idx_name (first_name, last_name);
```
3. インデックスの効果を測定
インデックス追加前後での検索クエリの実行時間を測定します。以下のクエリを実行し、各クエリの実行時間を記録してください。
```sql
SELECT * FROM user WHERE email = 'user1@example.com';
SELECT * FROM user WHERE first_name = 'First1' AND last_name = 'Last1';
```
4. 不必要なインデックスの削除
もし、インデックスが不要だと感じる場合、不要なインデックスを削除して、INSERTやUPDATEのパフォーマンスを向上させましょう。以下のコマンドを実行して、インデックスを削除します。
```sql
ALTER TABLE user DROP INDEX idx_name;
```
5.クエリの最適化
status カラムに基づいてユーザーをフィルタリングするクエリを作成し、パフォーマンスを向上させるためにインデックスを追加します。次のコマンドを実行して、インデックスを追加し、実行時間を測定してください。
```sql
ALTER TABLE user ADD INDEX idx_status (status);
SELECT * FROM user WHERE status = 1;
```
