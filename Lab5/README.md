# Lab 5: MDSへのReplication

![](images/Lab5-0.png)

## Key Objectives:
- インバウンドレプリケーションを使用するためにOCIでMySQLデータベースサービスレプリケーションチャネルを作成する方法を学習します
- レプリケーションのステータスを確認する方法を学ぶ 

## Introduction

このラボでは、MySQLデータベースサービスインスタンスのインバウンドレプリケーションを設定します。

レプリケーションを使用すると、1つのMySQLデータベースサーバー（ソースと呼ばれる）からのデータを1つ以上のMySQLデータベースサーバー（レプリカと呼ばれる）にコピーできます。 レプリケーションはデフォルトで非同期です。 ソースから更新を受信するために、レプリカを永続的に接続する必要はありません。 In this lab you will set up inbound replication for your MySQL Database Service instance.

**[MySQL Replication Overview](https://dev.mysql.com/doc/refman/8.0/en/replication.html)**

OCIでは、インバウンドレプリケーションには、MySQLデータベースサービスで構成されたレプリケーションチャネルが必要であり、正しく構成されたMySQLソースをDBシステムターゲットに接続します。 

**[MySQL Database Service Inbound Replication Overview](https://docs.oracle.com/en-us/iaas/mysql-database/doc/replication.html)**

**Please Note:** MySQLデータベースサービスインスタンスはパブリックインターネットに直接接続されていないため、レプリケーションチャネルを構成するときは、レプリケーションソースとして**MySQL Router Private Hostname**を使用します。 



## Steps

### **Step 5.1:**
- 左上隅のメインメニューから _**データベース >> Channels** を選択します 

![](images/Lab5-1.png)

### **Step 5.2:**
- _**チャネルの作成**_ をクリック

![](images/Lab5-2.png)

### **Step 5.3:**
-　_**コンパートメントに作成**_　ドロップダウンリストに、これまで使用していたのと同じコンパートメント名が表示されていることを確認します（　_**mds-replication-hol**_ ）
- デフォルトの _**名前**_ のままにします（必要に応じて変更することもできます） 

![](images/Lab5-3.png)

### **Step 5.4:**
- _**ソース接続**_ セクションでは、MySQLソースインスタンスでレプリケーションを設定するためのパラメータを設定できます。 
- _**注意**_: はじめに述べたように、MySQLデータベースサービスDBシステムはインターネットに直接アクセスできないため、_**内部FQDN**_ を _**ソースホスト名**_ として使用します。
次の図のように、MySQLルーターのコンピューティングインスタンスの詳細ページ（ _**メインメニュー>>コンピューティング>>インスタンス>> MySQLルーターインスタンスをクリック**_ ）から取得できます。 

![](images/Lab5-4a.png)

- _**MySQLルーターの内部FQDN**_　を取得したら、_**ソース接続**_　入力フィールドに次の値を入力します（次の図を参照） : 
	- Hostname: _**MySQL Router Internal FQDN**_ 
	- Port: _**3306**_
	- Username: _**root**_
	- Password: _**Oracle.123**_
	- Confirm Password: _**Oralce.123**_

- _**SSL Mode**_　サブセクションで、ボックス　_**無効（DISABLED）**_　を選択します

![](images/Lab5-4b.png)

_**注意**_: これはラボ環境であるため、簡単にするために、レプリケーションチャネルの暗号化を無効にしました。 実際の環境（本番環境、テスト環境、開発環境など）でMySQLデータベースサービスレプリケーションチャネルを構成する場合は、_**常に**_ _**SSLモード**_ を _ **必須（REQUIRED）** _ に設定する必要があります。

### **Step 5.5:**
-　_**ターゲット**_　セクションでは、_**Replica**_　として選択する　_**MySQL Database Service DB System**_　を選択できます。

-　_**Applier ユーザ名**_ 入力ボックスに _**admin**_ と入力します。 _**channel名**_ はデフォルトのままにしておきます。

- サブセクション _**DBシステムの選択**_ に _**DBシステムが選択されていません**_ と表示されている場合は、_**DBシステムの選択**_ ボタンをクリックします。

- サブセクション _**DBシステムの選択**_ に以前に作成されたMySQLデータベースサービスインスタンスが表示されている場合は、ステップ5.7に進みます。 

![](images/Lab5-5.png)

### **Step 5.6:**
- ウィンドウ _**DBシステムの選択**_ で、以前に作成した _**mds-replication-hol-replica**_ を選択し、_**DBシステムの選択**_ ボタンをクリックします

![](images/Lab5-6.png)

### **Step 5.7:**
- _**チャネルの作成**_ をクリック

![](images/Lab5-7.png)

### **Step 5.8:**
- これまでのすべての手順を正しく実行すると、レプリケーションチャネルは問題なくプロビジョニングされ、_**アクティブ**_ 状態になります。 

![](images/Lab5-8.png)

- _**Congratulations!! MySQLデータベースサービスを使用して、レプリケートされた環境を正常にセットアップできました!!**_ 
次のいくつかの手順では、すべてが期待どおりに機能することを確認し、レプリケーションの動作を確認します。 

### **Step 5.9:**
- クラウドシェルに戻ります。クラウドシェルはまだ _**mysql-replication-router**_ インスタンスに接続されているはずです。
- _**mysql-replication-router**_ インスタンスから、_**プライベートIPアドレス**_ を介して _**MySQLデータベースサービスレプリカインスタンス**_ にアクセスし、_**複製ソース**_ は正しく複製されました。

_**注意**_: _**MySQL Database Service**_　に接続するために、その　_**プライベートIP**_　を使用します。 _**MySQLデータベースサービスのプライベートIPアドレス**_　は、MySQLデータベースサービスのDBシステムの詳細ページから取得できます（_ **メインメニュー>>データベース>>（MySQL）DBシステム>> MySQLデータベースサービスDBをクリックします**_ ）、下図参照 :

![](images/Lab5-9a.png)

_**MySQL DatabaseServiceレプリカインスタンス**_ に接続して既存のスキーマを一覧表示するには、次のコマンドを実行します :
```
mysqlsh --uri admin:Oracle.123@<mds-private-ip>:3306 --sql
show databases;
```
- 次の図のように、_**world_x**_ スキーマが表示されます

![](images/Lab5-9b.png)

- _**Optional**_: その他コマンド
```
SHOW REPLICA STATUS\G
```
出力は次のようになります:

![](images/Lab5-9c.png)

_**追加説明**_: レプリケーションが正しく機能している場合は、_**Slave/Replica_IO_State**_ フィールドに _**Waiting for master to send event**_ および _**Last_Error**_ とマークされています。 レプリケーションでエラーが発生した場合、_**Last_Error**_ フィールドにエラーの説明が含まれ、トラブルシューティングに役立ちます。 

### **Step 5.10:**
- _**MySQL Database Service**_ へのMySQLShell接続を終了します :
```
\exit
```

### **Step 5.11:**
- ここで、_**パブリックPublicアドレス**_ を介して _**MySQL Replication Source**_ に接続し、ダミーデータベースを作成します。 

_**注意**_: _**MySQL Replication Source**_ に接続するために、その _**パブリック IP Addrees**_ を使用します。 _**レプリケーションソースパブリックIPアドレス**_ は、レプリケーションソースコンピューティングインスタンスの詳細ページから取得できます（_**メインメニュー>>データベース>>（MySQL）DBシステム>>レプリケーションソースインスタンスをクリックします**_ ）、下図参照 :

![](images/Lab5-11.png)

- _**MySQL Replication Source**_　に接続して新しいスキーマを作成し、コマンドを実行します :
```
mysqlsh --uri root:Oracle.123@<source-public-ip>:3306 --sql
```
- _**MySQL Replication Source**_ に接続した後、次のコマンドを実行します :
```
select @@hostname;
```
...結果のホスト名が _**mysql-replication-source**_ であることを確認してください。
この追加のチェックは、データを書き込もうとしているため、適切なホストに接続されていることを確認するために行われます。 レプリケーションソースに誤ってデータを書き込むと、レプリケーションが中断する可能性があります。 

- 適切なホストに接続していることを確認したら、次のコマンドを実行します :
```
create database test;
\exit
```

### **Step 5.12:**
- レプリケーションの動作を確認します！ _**プライベートIPアドレス**_　を介して　_**MySQLデータベースサービスレプリカインスタンス**_　に接続し、スキーマのリストを再度確認します

_**注意**_: _**MySQL Database Service**_ に接続するために、その _**プライベートIP**_　を使用します。 _**MySQLデータベースサービスのプライベートIPアドレス**_　は、MySQLデータベースサービスのDBシステムの詳細ページから取得できます（　_**メインメニュー>>コンピューティング>>インスタンス>> MySQLデータベースサービスのDBシステムをクリックします**_ ）

![](images/Lab5-9a.png)

_**MySQL DatabaseServiceレプリカインスタンス**_　に再度接続し、以前に作成したスキーマが正常に複製されたかどうかを確認するには、次のコマンドを実行します
commands:
```
mysqlsh --uri admin:Oracle.123@<mds-private-ip>:3306 --sql
show databases;
```
結果は次の画像のようになります
![](images/Lab5-10.png)

## Conclusion

この最後のラボでは、レプリケーションチャネルを正常に作成し、ソースインスタンスからデータをレプリケートしました。 !!

Learn more about **[MySQL Replication](https://dev.mysql.com/doc/refman/8.0/en/replication.html)**
Learn more about **[MySQL Database Service Inbound Replication](https://docs.oracle.com/en-us/iaas/mysql-database/doc/replication.html)**

## Great Work - All Done!

**[<< Go to Lab 4](../Lab4/README.md)** | **[Home](/README.md)**
