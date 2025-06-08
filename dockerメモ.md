# dockerメモ

## dockerのコマンド

### イメージ取得
```bash
$ docker pull mysql
```

### イメージ一覧表示
```bash
$ docker images
```

### コンテナ作成
```bash
$ docker run --name mysql1 -p 13306:3306 -e MYSQL_ROOT_PASSWORD=pw1 -d mysql
```
引数
- `run`
    runコマンド
- `--name`
    コンテナ名
- `-p ホスト側ポート(転送元) コンテナ側ポート(転送先)`
    ポート転送しないと、ホストからコンテナへの接続ができない。
    逆向きは何もしなくていい。
- `-e 環境変数=値`
    初回起動時に、ここで指定しているパスワードがMySQLに設定される模様。
- `-d`
    バックグラウンド実行`
- `イメージ名`

### 実行中のコンテナ一覧
```bash
$ docker ps
$ docker ps -a
```

### コンテナ内でコマンド実行
```bash
$ docker exec -it mysql1 bash
```
引数
- `exec`
    execコマンド
- `-i`
    インタラクティブモード
- `-t`
    ttyモード
- `コンテナ名`
- `実行するコマンド`

### コンテナの起動、停止
```bash
$ docker start mysql1
$ docker stop mysql1
```

### コンテナの削除、イメージ削除
```bash
$ docker rm mysql1
$ docker rmi mysql
```

## 動作確認(MySQL)

### イメージ取得＆コンテナ作成
```bash
$ docker pull mysql
$ docker run --name mysql1 -p 13306:3306 -e MYSQL_ROOT_PASSWORD=pw1 -d mysql
```

### MySQLのコンテナ側
```bash
mysql -u root -p
パスワード入力(docker run時のMYSQL_ROOT_PASSWORDの内容)
mysql> CREATE DATABASE test_db;
mysql> USE test_db;
mysql> CREATE TABLE users ( id INT AUTO_INCREMENT NOT NULL PRIMARY KEY, name VARCHAR(50) );
mysql> INSERT INTO users (name) VALUES ("sample data");
mysql> SELECT * FROM users;
```
    
### ホスト側
```python
import mysql.connector

cnx = mysql.connector.connect(
    user='root',
    password='pw1',
    host='localhost',
    port='13306'
)
cursor = cnx.cursor()

cursor.execute('SELECT * FROM test_db.users')

for id, name in cursor:
    print( f'{id}: {name}' )
```


## 動作確認(PostgreSQL)

### イメージ取得＆コンテナ作成
```bash
$ docker pull postgres
$ docker run --name postgres1 -e POSTGRES_PASSWORD=mysecretpassword -d postgres
```
    
### コンテナ側
```bash
$ psql -h localhost -d postgres -U postgres
```
psqlコマンドの引数
- `-h ホスト名`
- `-d データベース名`
- `-U ユーザー名`

psqlコマンド内の操作
```bash
ユーザ一覧
psql> \du
データベース一覧
psql> \l 
psql> CREATE DATABASE test_db;
データベース選択
psql> \c test_db
psql> CREATE TABLE users ( id SERIAL NOT NULL PRIMARY KEY, name VARCHAR(50) );
テーブル一覧
psql> \dt
psql> INSERT INTO users (name) VALUES ('sample data');
psql> SELECT * FROM users;
```
    
### ホスト側
```python
import psycopg2

db_info = {
    'host': 'localhost',
    'port': 15432,
    'dbname': 'test_db',
    'user': 'postgres',
    'password': 'mysecretpassword',
}

with psycopg2.connect(**db_info) as conn:
    with conn.cursor() as cur:
        cur.execute('SELECT * FROM users;')    
        rows = cur.fetchall()

        for row in rows:
            print(row)
```
