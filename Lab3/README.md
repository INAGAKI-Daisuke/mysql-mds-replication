# Lab 3: MDSの構築

![](images/Lab3-0.png)

## 目的:
- スタンドアロンMySQLデータベースサービスをデプロイおよび構成する方法を学習します
- MySQLデータベースサービスの管理者ユーザーを作成する方法を学びます

## Introduction

MySQLデータベースサービスは、フルマネージドのOracle Cloud Infrastructureネイティブサービスであり、OracleのMySQLチームによって開発、管理、およびサポートされています。 Oracleは、バックアップとリカバリ、データベースとオペレーティングシステムのパッチ適用などのすべてのタスクを自動化します。 お客様は、データ、スキーマ設計、およびアクセスポリシーの管理のみを担当します。
**[MySQL Database Service overview](https://docs.oracle.com/en-us/iaas/mysql-database/doc/overview-mysql-database-service.html)**


## Steps

### **Step 3.1:**
- 左メニューより _**データベース >> DBシステム**_ を選択する

![](images/Lab3-1.png)

### **Step 3.2:**
- 前の手順で、DBシステムの作成ページに移動します。左側のコンパートメントセレクターを見て、VCNの作成に使用したのと同じコンパートメントを使用していることを確認します。 完了したら、_**MySQL DBシステムの作成**_ をクリックします。 

![](images/Lab3-2.png)

### **Step 3.3:**
- DBシステムの作成を開始します。 コンパートメントを再度クロスチェックし、DBシステムに名前 _**mds-replication-hol-replica**_ を割り当て、_**スタンドアロン**_ ボックスを選択します。 これにより、レプリカとして使用するスタンドアロンのMySQLDBシステムを作成できます。 他のボックスは無視してください。 

![](images/Lab3-3.png)

### **Step 3.4:**
  _**管理者資格証明書の作成**_ には以下を入力します:
```
username: admin
password: Oracle.123
confirm password: Oracle.123
```

![](images/Lab3-4.png)

### **Step 3.5:**
- _**ネットワーキングの構成**_ にはVCNには _**mds-replication-hol**_ と _**パブリックサブネット**_ の選択肢から (_**パブリック・サブネット-replication-hol-vcn**_)を指定します

- デフォルトの可用性ドメインのままにして、_**ハードウェアの構成**_　セクションに進みます

![](images/Lab3-5.png)

### **Step 3.6:**
- Confirm that in the _**Configure Hardware**_ section, the selected shape is MySQL.VM.Standard.E3.1.8GB, CPU Core Count: 1, Memory Size: 8 GB, Data Storage Size: 50.

- In the _**Configure Backup**_ section leave the default backup window of 7 days.

![](images/Lab3-6.png)

### **Step 3.7:**
- Scroll down and click on _**Show Advanced Options**_ 

- In the Configuration tab click on _**Select Configuration**_

![](images/Lab3-7.png)

### **Step 3.8:**
- In the _**Browse All Configurations**_ window, select _**MySQL.VM.Standard.E3.1.8GB.Standalone**_, and click the button _**Select a Configuration**_ 

![](images/Lab3-8.png)

### **Step 3.9:**
- After the configuration has been selected, go to the _**Networking**_ tab, and in the _**Hostname**_ field enter _**mysql-replication-hol-replica**_ (same as DB System Name). 
Check that port configuration corresponds to the following:
MySQL Port: 3306
MySQL X Protocol Port: 33060
Once done, click the _**Create**_ button.

![](images/Lab3-9.png)

### **Step 3.10:**
- The MySQL DB System will enter _**CREATING**_ state (as per picture below). Meanwhile you can go ahead and proceed to the next Lab.

![](images/Lab3-10.png)

## Conclusion

In this Lab you deployed MySQL Database Service and created the administration user for the database. All set and ready to replicate? Not yet, as you might have noticed the MySQL Database Service does not expose any Public IP, therefore we will need to expose one via a bastion host where we will install MySQL Router
 
Learn more about **[MySQL Database Service](https://docs.oracle.com/en-us/iaas/mysql-database/doc/overview-mysql-database-service.html)**

**[<< Go to Lab 2](../Lab2/README.md)** | **[Home](/README.md)** | **[Go to Lab 4 >>](../Lab4/README.md)**
