# Lab 5: Create the Replication Channel

![](images/Lab5-0.png)

## Key Objectives:
- Learn how to create an MySQL Database Service Replication Channel in OCI to use inbound replication
- Learn how to check the status of the replication

## Introduction

In this lab you will set up inbound replication for your MySQL Database Service instance.

Replication enables data from one MySQL database server (known as a source) to be copied to one or more MySQL database servers (known as replicas). Replication is asynchronous by default; replicas do not need to be connected permanently to receive updates from a source.
**[MySQL Replication Overview](https://dev.mysql.com/doc/refman/8.0/en/replication.html)**

In OCI, Inbound Replication requires a replication channel configured in MySQL Database Service, connecting a correctly configured MySQL Source to a DB System target.
**[MySQL Database Service Inbound Replication Overview](https://docs.oracle.com/en-us/iaas/mysql-database/doc/replication.html)**

**Please Note:** when you will configure the replication channel, you will use the **MySQL Router Private Hostname** as a replication source, since your MySQL Database Service instance is not directly connected with the Public Internet.



## Steps

### **Step 5.1:**
- From the main menu on the top left corner select _**Databases >> Channels**_

![](images/Lab5-1.png)

### **Step 5.2:**
- Click on the _**Create Channel**_ button

![](images/Lab5-2.png)

### **Step 5.3:**
- Check that _**Create in Compartment**_ drop down list shows the same compartment name which you have been using so far (_**mds-replication-hol**_)
- Leave the default _**Name**_ (you can also change it if you wish)

![](images/Lab5-3.png)

### **Step 5.4:**
- The _**Source Connection**_ section allows you to configure the parameters to set the replication with the MySQL Source Instance.

- _**PLEASE NOTE**_: Since the MySQL Database Service DB System _**DOES NOT**_ have direct access to the internet, as mentioned in the introduction, you will be using the _**MySQL Router Internal FQDN**_ as _**Source Hostname**_.
You can retrieve it from the MySQL Router Compute Instance details page (_**Main Menu >> Compute >> Instances >> click on the MySQL Router instance**_), as per below picture:

![](images/Lab5-4a.png)

- Once you have retrieved the _**MySQL Router Internal FQDN**_, enter the following values in the _**Source Connection**_ input fields (as in the below picture): 
	- Hostname: _**MySQL Router Internal FQDN**_ 
	- Port: _**3306**_
	- Username: _**root**_
	- Password: _**Oracle.123**_
	- Confirm Password: _**Oralce.123**_

- Under the _**SSL Mode**_ subsection, select the box _**Disabled (DISABLED)**_

![](images/Lab5-4b.png)

_**PLEASE NOTE**_: This is a Lab environment, therefore, for the sake of simplicity, we have disabled encryption in the replication channel. If you will be configuring MySQL Database Service replication channel on a real environment (production, test, dev... doesn't matter) you should _**ALWAYS**_ set _**SSL Mode**_ to _**REQUIRED (REQUIRED)**_.

### **Step 5.5:**
- The _**Target**_ section allows you to choose the _**MySQL Database Service DB System**_ which you will select as _**Replica**_

- In the _**Applier Username**_ input box enter _**admin**_. Leave the _**Channel Name**_ as per default.

- If the subsection _**Select a DB System**_ shows _**No DB System selected**_, click the button _**Select DB System**_.

- If the subsection _**Select a DB System**_ shows already the previously created MySQL Database Service instance, go to Step 5.7

![](images/Lab5-5.png)

### **Step 5.6:**
- In the window _**Select a DB System**_, select the _**mds-replication-hol-replica**_ which you have previously created and then click the button _**Select DB System**_.

![](images/Lab5-6.png)

### **Step 5.7:**
- Click _**Create Channel**_

![](images/Lab5-7.png)

### **Step 5.8:**
- If you executed correctly all the previous steps, the replication channel will be provisioned with no issues and it will turn into _**Active**_ state.

![](images/Lab5-8.png)

- _**Congratulations!! You managed to succesfully set up a replicated environment using MySQL Database Service!!**_
In the next few steps, you will verify that everyhing works as expected and observe replication in action!!

### **Step 5.9:**
- Go back to the Cloud Shell, which should still be connected the _**mysql-replication-router**_ instance
- From the _**mysql-replication-router**_ instance you will now access the _**MySQL Database Service Replica Instance**_ over the _**Private IP Address**_, to check that the content from the _**Replication Source**_ has been correctly replicated.

_**PLEASE NOTE**_: In order to connect to _**MySQL Database Service**_ we will use its _**Private IP**_. You can retrieve the _**MySQL Database Service  Private IP Address**_ from the MySQL Database Service DB System details page (_**Main Menu >> Databases >> (MySQL) DB Systems >> click on the MySQL Database Service DB System**_), as per below picture:

![](images/Lab5-9a.png)

To connect to the _**MySQL Database Service Replica Instance**_ and list existing schemas, execute the following commands:
```
mysqlsh --uri admin:Oracle.123@<mds-private-ip>:3306 --sql
show databases;
```
- You should see the _**world_x**_ schema, as in the picture below.

![](images/Lab5-9b.png)

- _**Optional**_: Execute the command
```
SHOW REPLICA STATUS\G
```
The output should look as follows:

![](images/Lab5-9c.png)

_**Additional Explanation**_: If replication is working correctly you will see the _**Slave/Replica_IO_State**_ field marked as _**Waiting for master to send event**_ and the _**Last_Error**_ field marked empty.
In case of errors in the replication, the _**Last_Error**_ field will contain an explanation of the error, which is going to be useful for troubleshooting.

### **Step 5.10:**
- Exit the MySQL Shell connection to _**MySQL Database Service**_ typing:
```
\exit
```

### **Step 5.11:**
- You will now connect to the _**MySQL Replication Source**_ over the _**Public IP Addrees**_ and create a dummy database.

_**PLEASE NOTE**_: In order to connect to _**MySQL Replication Source**_ we will use its _**Public IP Addrees**_. You can retrieve the _**Replication Source  Public IP Address**_ from the Replication Source Compute Instance details page (_**Main Menu >> Database >> (MySQL) DB Systems>> click on the Replication Source instance**_), as per below picture:

![](images/Lab5-11.png)

- To connect to the _**MySQL Replication Source**_ and create a new schema, execute the commands:
```
mysqlsh --uri root:Oracle.123@<source-public-ip>:3306 --sql
```
- After you have connected to the _**MySQL Replication Source**_, execute the following command:
```
select @@hostname;
```
...and make sure that the resulting hostname is _**mysql-replication-source**_.
This additional check is done in order to make you sure you are connected to the right host, since we are about to write data. Writing data by mistake into the replication source would cause replication to break!

- Once you have checked that you are connected to the right host, execute the following commands:
```
create database test;
\exit
```

### **Step 5.12:**
- Time to see replication in action! Connect to _**MySQL Database Service Replica Instance**_ over the _**Private IP Address**_ and check again the list of schemas.

_**PLEASE NOTE**_: In order to connect to _**MySQL Database Service**_ we will use its _**Private IP**_. You can retrieve the _**MySQL Database Service  Private IP Address**_ from the MySQL Database Service DB System details page (_**Main Menu >> Compute >> Instances >> click on the MySQL Database Service DB System**_), as per below picture:

![](images/Lab5-9a.png)

To connect again to _**MySQL Database Service Replica Instance**_ and check if the previously created schema has replicated successfully, execute the following commands:
```
mysqlsh --uri root:Oracle.123@<mds-private-ip>:3306 --sql
show databases;
```
The result should be similar to the below image.
![](images/Lab5-10.png)

## Conclusion

In this last lab you have successfully created a replication channel and replicated data from your source instance!!

Learn more about **[MySQL Replication](https://dev.mysql.com/doc/refman/8.0/en/replication.html)**
Learn more about **[MySQL Database Service Inbound Replication](https://docs.oracle.com/en-us/iaas/mysql-database/doc/replication.html)**

## Great Work - All Done!

**[<< Go to Lab 4](../Lab4/README.md)** | **[Home](/README.md)**
