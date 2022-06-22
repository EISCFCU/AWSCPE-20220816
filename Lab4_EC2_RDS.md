# 動態網站與資料庫整合應用



# 情境

森森購物考量到未來資訊服務的使用彈性，預計公司自建的庫存管理系統(Web-based)遷移到 Amazon Web Services。
昨日專案啟動會議，資訊主管指示A團隊小組將庫存管理系統伺服器架設於AWS後，下一步要建置庫存資料庫，並連結網站伺服器，請協助進行庫存管理系統的雲端搬遷。

# 適用環境：AWS Academy Learner Lab/ ACF Sandbox

# 實施架構

![image](https://user-images.githubusercontent.com/103306835/174927690-e10e797d-8f47-4f93-aa5d-b07c9cd58816.png)


# 實施步驟


![image](https://user-images.githubusercontent.com/103306835/174927961-04c594fa-5e03-44d3-ad70-f2c6f9363820.png)


# Step 1:建立網頁伺服器(EC2)

1.選擇EC2

![image](https://user-images.githubusercontent.com/103306835/166851151-08e46b22-8c5b-4cf1-8bec-5b8ae0b2425f.png)


2.點選[Launch instances]

![image](https://user-images.githubusercontent.com/103306835/166851177-42e11a25-9a80-4d61-8f7c-11085b8fc36a.png)

3.輸入名稱WebServer

![image](https://user-images.githubusercontent.com/103306835/166851191-e4b19cba-041b-4de7-8f08-de56eddbf481.png)

4.選擇Key pair=vockey

![image](https://user-images.githubusercontent.com/103306835/166851213-e9f54314-3064-4fad-b4e6-daa2265f3d29.png)

5.點選[Edit]

![image](https://user-images.githubusercontent.com/103306835/166851232-44c31966-fe2f-481d-a178-684eaafad32e.png)

6.選擇Default VPC->選擇Subnet=us-east-1a->選擇security group=default

![image](https://user-images.githubusercontent.com/103306835/166851279-7225c382-71a6-4273-b954-93951cf3dcfa.png)

7.選擇IAM

![image](https://user-images.githubusercontent.com/103306835/166851300-b7d84f14-9ab3-4c77-9f58-f579779841c9.png)

8.選擇LabInstanceProfile

![image](https://user-images.githubusercontent.com/103306835/166851315-b6a27120-3f1a-49bf-a855-97aa1a346e1b.png)

9.輸入User data

![image](https://user-images.githubusercontent.com/103306835/166851328-862ab3ef-638a-4b23-93be-ba9e70de07f1.png)

```
#!/bin/bash
# Install Apache Web Server and PHP
yum install httpd mysql -y
amazon-linux-extras install -y php7.2
# Download Lab files
wget https://us-west-2-tcprod.s3.amazonaws.com/courses/ILT-TF-100-ARCHIT/v6.2.1/lab-1-webapp/scripts/inventory-app.zip
unzip inventory-app.zip -d /var/www/html/
# Download and install the AWS SDK for PHP
wget https://github.com/aws/aws-sdk-php/releases/download/3.62.3/aws.zip
unzip aws -d /var/www/html
# Turn on web server and ensure running on reboot
service httpd start
chkconfig httpd on
```
10.點選[Launch Instance]

![image](https://user-images.githubusercontent.com/103306835/166851413-cf7203ff-c098-4227-8ef8-e2bd07890cf8.png)

11.點選[View all instances]

![image](https://user-images.githubusercontent.com/103306835/166851439-24ac0c9b-633d-4bb9-a2ff-7ff8b24944c9.png)

12.等待虛擬機狀態2/2 checks passed->並記錄Public IPv4 address 及Public IPv4 DNS

![image](https://user-images.githubusercontent.com/103306835/166851460-dc61e27e-a417-455e-b282-f3c98de0e5a2.png)

# Step2:建立雲端資料庫

2-1.選擇 Databases->點擊 Create database

![image](https://user-images.githubusercontent.com/103306835/166851850-0ac75d84-0c5a-4100-980c-594c16f98359.png)

2-2.選擇 MySQL

![image](https://user-images.githubusercontent.com/103306835/166851863-0d74cad0-fafa-46d9-93ed-97875242100f.png)

2-3.選擇Free Tier

![image](https://user-images.githubusercontent.com/103306835/166851891-d4bdc05f-55ae-41f1-ae49-ef24dd1a9894.png)

2-4.設定資料庫

'''
DB instance identifier ：lab-db
Master username：main
Master password：lab-password
Confirm password：lab-password
'''

![image](https://user-images.githubusercontent.com/103306835/166851904-f63ad32d-8a37-4119-92ec-c4d562fa20e9.png)

2-5.Burstable classes (includes t classes)->選擇 db.t3.micro

![image](https://user-images.githubusercontent.com/103306835/166851986-10cbb5d4-3c2b-46c4-9d6b-9f68a8c4ef50.png)

2-6.Storage type：General Purpose (SSD)->Allocated storage：20

![image](https://user-images.githubusercontent.com/103306835/166852007-4323f0ce-6fc9-4889-8e67-e156baf38cf7.png)

2-7.Virtual Private Cloud (VPC)：Defalt VPC

![image](https://user-images.githubusercontent.com/103306835/166852047-11c77aaf-9349-4d51-bad4-39e8d06a38ff.png)

2-8.DB Security Group (資料庫安全組)->選擇Default

![image](https://user-images.githubusercontent.com/103306835/166852111-38cbb393-3545-449d-9c38-967e45ddd027.png)

2-9.展開  Additional configuration (其他配置)->Initial database name：lab

![image](https://user-images.githubusercontent.com/103306835/166852134-8c733217-343b-47f2-a461-3578545457c7.png)

2-10.資料庫備份監控設定

取消選取 Enable automatic backups (啟用自動備份)

取消選取 Enable Enhanced monitoring (啟用增強監控)

2-11.點擊 Create database (創建數據庫)

![image](https://user-images.githubusercontent.com/103306835/166852184-a3222568-0874-4fec-a350-eed970b6293c.png)


直到狀態變為Modifying（正在修改）或 Available（可用）


# Step3:網站與資料庫連線測試

3-1.點擊 lab-db

![image](https://user-images.githubusercontent.com/103306835/166852259-5e7c970d-1892-448d-b991-042b2d0a91e1.png)

3-2.複製 Endpoint（終端節點）字段

![image](https://user-images.githubusercontent.com/103306835/166852280-cd177cda-0ecf-4601-801e-85cee96fb6bd.png)

3-3.開啟網頁->並點選[Settings]

![image](https://user-images.githubusercontent.com/103306835/166852303-f5db5fee-8ff3-4be0-8990-34f3efd3f388.png)

3-4.輸入資料庫連線資料 並點選[Save]

![image](https://user-images.githubusercontent.com/103306835/166852321-1d460352-ecec-42af-ba94-5f314bc19e5d.png)

3-5.成功連線到資料庫

![image](https://user-images.githubusercontent.com/103306835/166852349-bb29a122-b085-4adc-92aa-52c141e1a8b7.png)

