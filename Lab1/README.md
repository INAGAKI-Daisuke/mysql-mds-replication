# Lab 1: コンピュートインタンス・ネットワーク設定等の環境構築

![](images/Lab1-0.png)

## Key Objectives:

- OCIテナントのホストリソースにコンパートメントを作成する方法を学びます
- インターネット接続を備えた仮想クラウドネットワークを作成する方法を学びます
- MySQLデータベースサービスポートを介したトラフィックを許可するためのセキュリティリストに入力ルールを追加する

## Introduction

コンパートメントは、クラウドリソースを整理して隔離するためのOracle Cloud Infrastructureの基本コンポーネントです。 使用法と請求、アクセス（ポリシーの使用など）、および個別管理（あるプロジェクトまたはビジネスユニットのリソースを別のプロジェクトのリソースを分離する）の目的で、それらを使用するためにそれらを使用します。 

**[Compartment Overview](https://docs.oracle.com/en-us/iaas/Content/GSG/Concepts/settinguptenancy.htm#Understa)**


Virtual Cloud Network helps you define your own data centre network topology inside the Oracle Cloud by defining some of the following components (Subnets, Route Tables, Security Lists, Gateways, etc.). Some of these components are set by default automatically within every VCN and can't be deleted:
-	Default route table, with no route rules
-	Default security list, with default security rules
-	Default set of DHCP options, with default values
For an overview of VCNs, allowed size, default VCN components, and scenarios for using a VCN, check **[Networking Overview](https://docs.oracle.com/en-us/iaas/Content/Network/Concepts/overview.htm#network_landing)**.

A security list acts as a virtual firewall for an instance, with ingress and egress rules that specify the types of traffic allowed in and out. Each subnet can have multiple security lists associated with it, and each list can have multiple rules. The default security list comes with an initial set of stateful rules, which should in most cases be changed to only allow inbound traffic from authorized subnets relevant to the region that hosts that VCN or subnet. A list of authorized subnet ranges relevant to each region can be found **[here](https://docs.cloud.oracle.com/iaas/tools/public_ip_ranges.json)**.


## Steps

### **Step 1.1:**
  Log-in to your OCI tenancy. Once you have logged-in, clik on the _**menu icon**_ on the top left corner 
![](images/Lab1-1.png)

### **Step 1.2:**
  Once the menu appears select _**Identity and Security >> Compartments**_
![](images/Lab1-2.png)

### **Step 1.3:**
  From the left menu check that _**Compartments**_ is selected, and then click on _**Create Compartment**_
![](images/Lab1-3.png)

### **Step 1.4:**
  In the name and description field enter _**mds-replication-hol**_ and click the button _**Create Compartment**_
![](images/Lab1-4.png)

### **Step 1.5:**
  Once the compartment is created you will get to a screen like the one below
![](images/Lab1-5.png)

### **Step 1.6:**
  Now that the compartment is created, let's create the VCN.
  From the main menu, select _**Networking >> Virtual Cloud Networks**_ from the _**menu icon**_ on the top left corner

![](images/Lab1-6.png)

### **Step 1.7:**
  In the left bottom pane below the context menu, expand the _**Compartment drop down list**_ and select the _**mds-replication-hol**_ compartment.

![](images/Lab1-7.png)

### **Step 1.8:**
  In the context menu on the top left make sure _**Virtual Cloud Networks**_ is selected, and then click the button _**Start VCN Wizard**_

![](images/Lab1-8.png)

### **Step 1.9:**
  When the wizard starts, select _**VCN with Internet Connectivity**_ and click on _**Start VCN Wizard**_

![](images/Lab1-9.png)

### **Step 1.10:**
  In the _**VCN NAME**_ field enter _**mds-replication-hol-vcn**_, leave the rest as per default, and click _**Next**_

![](images/Lab1-10.png)

### **Step 1.11:**
   Review and click _**Create**_

![](images/Lab1-11.png)

### **Step 1.12:**
  Once the VCN is created, click on  _**View Virtual Cloud Network**_ and you will be taken to the related page as in the picture below. Click on _**Public Subnet-mds-replication-hol-vcn**_

![](images/Lab1-12.png)

### **Step 1.13:**
 Earlier we have set up the subnet to use the VCN's default security list, that has default rules, which are designed to make it easy to get started with Oracle Cloud Infrastructure. 

 Now we will customize the default security list of the VCN to allow traffic through MySQL Database Service ports by clicking on  _**Default_Security_List_for_mds-replication-hol-vcn**_

![](images/Lab1-13.png)

### **Step 1.14:**
  Click on _**Add Ingress Rules**_

![](images/Lab1-14.png)

### **Step 1.15:**
 Add the necessary rule to the default security list to enable traffic through MySQL Database Service port. 

Insert the details as below:
- Source CIDR:  _**0.0.0.0/0**_
- IP Protocol: _**TCP**_
- Destination Port Range: _**3306,33060**_

At the end click the blue button _**Add Ingress Rules**_

![](images/Lab1-15.png)

### **Step 1.16:**
  Once the Ingress Rules are added, you will see the following result:

![](images/Lab1-16.png)

## Conclusion

Now that you have created the compartment, the VCN and added the additional Ingress rules to the Security list, you can proceed to the next lab!

Learn more about **[Compartments](https://docs.oracle.com/en-us/iaas/Content/GSG/Concepts/settinguptenancy.htm#Understa)**
Learn more about **[VCN and Subnets](https://docs.oracle.com/en-us/iaas/Content/Network/Tasks/managingVCNs_topic-Overview_of_VCNs_and_Subnets.htm)**

**[Home](README.md)** | **[Go to Lab 2 >>](../Lab2/README.md)**
