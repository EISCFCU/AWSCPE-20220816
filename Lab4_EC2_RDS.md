



# 情境

森森購物考量到未來資訊服務的使用彈性，預計公司自建的庫存管理系統(Web-based)遷移到 Amazon Web Services。
昨日專案啟動會議，資訊主管指示A團隊小組將庫存管理系統伺服器架設於AWS後，下一步要建置庫存資料庫，並啟動資料庫伺服器的異地備援機制(Multi-AZ) ， 並連結網站伺服器，請協助進行庫存管理系統的雲端搬遷。

# 適用環境：AWS Academy Learner Lab

# 實施步驟


![image](https://user-images.githubusercontent.com/103306835/166850981-6d2ec593-13cd-49f7-bbb6-7e479df3df52.png)


# Step 1:建立VPC

1.搜尋 VPC

![image](https://user-images.githubusercontent.com/103306835/166851051-5adbfcbf-2c98-4cdf-be3a-882c74b4c25d.png)

2.找到VPC

![image](https://user-images.githubusercontent.com/103306835/166851072-9bbddccc-4a13-469d-b8c0-c8c41d0ed268.png)

3.重新命名為Lab VPC

![image](https://user-images.githubusercontent.com/103306835/166851089-ba2550e5-3b3f-4dfe-a828-137430b99571.png)

# Step 2:建立網頁伺服器(EC2)

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

6.選擇Lap VPC->選擇Subnet=us-east-1a->選擇security group=default

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

# Step 3:EC2與RDS連線安全設定

1.搜尋 VPC

![image](https://user-images.githubusercontent.com/103306835/166851563-218e5566-9ab9-4075-995e-2df7638a20c8.png)

2.選擇 Security Groups->點擊 Create security groups

![image](https://user-images.githubusercontent.com/103306835/166851570-e5a6f47a-7dea-4728-9f61-0fe1a359f31e.png)

3.設定配置

Security group name：DB Security Group

Description：Permit access from Web Security Group

VPC：Lab VPC

![image](https://user-images.githubusercontent.com/103306835/166851618-329faf16-a1ee-40c0-b15b-bb0ecab1bbf6.png)

4.Add rule

![image](https://user-images.githubusercontent.com/103306835/166851634-0ac0fec2-bf75-410d-a77e-8fe492d2056a.png)


# Step4:建立雲端資料庫


1.搜尋 RDS

![image](https://user-images.githubusercontent.com/103306835/166851699-99f21789-3024-4a5a-838d-c29f75605f5a.png)


2.Create DB Subnet Group

![image](https://user-images.githubusercontent.com/103306835/166851724-5e455b7a-d763-4832-ac83-c2175f2786cc.png)

![image](https://user-images.githubusercontent.com/103306835/166851742-94cbbca5-3da6-4788-b869-bd70fe47c36f.png)

3.Add Subnets

![image](https://user-images.githubusercontent.com/103306835/166851765-ab01da76-fa80-43e2-9475-464a61409320.png)

4.Subnets 選擇 172.31.16.0/20 和 172.31.32.0/20->點擊 Create

![image](https://user-images.githubusercontent.com/103306835/166851792-4647610b-55c2-4afe-80ce-daf25d1e03a8.png)

5.選擇 Databases->點擊 Create database

![image](https://user-images.githubusercontent.com/103306835/166851850-0ac75d84-0c5a-4100-980c-594c16f98359.png)

6.選擇 MySQL

![image](https://user-images.githubusercontent.com/103306835/166851863-0d74cad0-fafa-46d9-93ed-97875242100f.png)

7.選擇Free Tier

![image](https://user-images.githubusercontent.com/103306835/166851891-d4bdc05f-55ae-41f1-ae49-ef24dd1a9894.png)

8.設定資料庫

'''
DB instance identifier ：lab-db
Master username：main
Master password：lab-password
Confirm password：lab-password
'''

![image](https://user-images.githubusercontent.com/103306835/166851904-f63ad32d-8a37-4119-92ec-c4d562fa20e9.png)

9.Burstable classes (includes t classes)->選擇 db.t3.micro

![image](https://user-images.githubusercontent.com/103306835/166851986-10cbb5d4-3c2b-46c4-9d6b-9f68a8c4ef50.png)

10.Storage type：General Purpose (SSD)->Allocated storage：20

![image](https://user-images.githubusercontent.com/103306835/166852007-4323f0ce-6fc9-4889-8e67-e156baf38cf7.png)

11.Virtual Private Cloud (VPC)：Defalt(Lab) VPC

![image](https://user-images.githubusercontent.com/103306835/166852047-11c77aaf-9349-4d51-bad4-39e8d06a38ff.png)

12. DB Security Group (資料庫安全組)->選擇DB Subnet Group

![image](https://user-images.githubusercontent.com/103306835/166852111-38cbb393-3545-449d-9c38-967e45ddd027.png)

13.展開  Additional configuration (其他配置)->Initial database name：lab

![image](https://user-images.githubusercontent.com/103306835/166852134-8c733217-343b-47f2-a461-3578545457c7.png)

14.資料庫備份監控設定

取消選取 Enable automatic backups (啟用自動備份)

取消選取 Enable Enhanced monitoring (啟用增強監控)

點擊 Create database (創建數據庫)

![image](https://user-images.githubusercontent.com/103306835/166852184-a3222568-0874-4fec-a350-eed970b6293c.png)



直到狀態變為Modifying（正在修改）或 Available（可用）


# Step5:網站與資料庫連線測試

1.點擊 lab-db

![image](https://user-images.githubusercontent.com/103306835/166852259-5e7c970d-1892-448d-b991-042b2d0a91e1.png)

2.複製 Endpoint（終端節點）字段

![image](https://user-images.githubusercontent.com/103306835/166852280-cd177cda-0ecf-4601-801e-85cee96fb6bd.png)

3.開啟網頁->並點選[Settings]

![image](https://user-images.githubusercontent.com/103306835/166852303-f5db5fee-8ff3-4be0-8990-34f3efd3f388.png)

4.輸入資料庫連線資料 並點選[Save]

![image](https://user-images.githubusercontent.com/103306835/166852321-1d460352-ecec-42af-ba94-5f314bc19e5d.png)

5.成功連線到資料庫

![image](https://user-images.githubusercontent.com/103306835/166852349-bb29a122-b085-4adc-92aa-52c141e1a8b7.png)

# Step6:設定資料庫副本

1.選擇lab-db 並點選[Modify]

![image](https://user-images.githubusercontent.com/103306835/166852381-b16fd998-3c1e-4324-a015-2b1e860fa4a3.png)

2.選擇Create a standby instance

![image](https://user-images.githubusercontent.com/103306835/166852414-40e9f8b7-99dd-4099-92fa-b775d1bfc5b0.png)

3.選擇Public accessible(為了讓同學能本機連到資料庫，故設定Public access)

![image](https://user-images.githubusercontent.com/103306835/166852452-1dc6748e-8f76-4ab4-a3a8-626f38fa079d.png)

4.點選[Continue]

![image](https://user-images.githubusercontent.com/103306835/166852469-b113c415-7e6b-4573-bf81-d68cacd14b58.png)

5.點選[Modify DB instance]

![image](https://user-images.githubusercontent.com/103306835/166852482-51ee89b2-1c04-49aa-9c45-88963018f0ae.png)

6.等待修改

![image](https://user-images.githubusercontent.com/103306835/166852496-2eb747fe-bfae-4f96-bfe7-556b11665188.png)

7.成功修改

![image](https://user-images.githubusercontent.com/103306835/166852522-4d3d8138-d91d-400b-8ea6-31a46ea6a4cd.png)

# Step7：修改雲端資料庫設定

1.點選EC2 Security Group

![image](https://user-images.githubusercontent.com/103306835/166852564-f4f469cb-532b-4b03-8028-998108879b40.png)

2.點選[Inbound rules]

![image](https://user-images.githubusercontent.com/103306835/166852588-22803174-8dd7-48d5-bc50-8a7a8ea6c44e.png)

3.點選[Edit inbound rules]

![image](https://user-images.githubusercontent.com/103306835/166852603-2187cbbe-2a5a-47cd-9f21-a1bec55d81b3.png)

4.點選[Delete]

![image](https://user-images.githubusercontent.com/103306835/166852629-779f0d34-d2f1-4b24-a563-1612d131d8a5.png)

5.點選[Add rule]

![image](https://user-images.githubusercontent.com/103306835/166852654-24a4bec2-2abb-423e-8c6b-0df40c059fa2.png)

6.選擇Type 為MYSQL/Aurora->Source：0.0.0.0/0->點選Save rules

![image](https://user-images.githubusercontent.com/103306835/166852692-e188c83f-6582-4d3f-ad93-fd6d15ca212f.png)

7.成功更改設定

![image](https://user-images.githubusercontent.com/103306835/166852707-ed54d6ab-91e1-4352-9c5b-82928cc250e7.png)

8.回到前一個RDS畫面->重整Security group

![image](https://user-images.githubusercontent.com/103306835/166852739-91eb9067-f9ac-4473-b249-e87967b6b049.png)

# Step8：遠端連線到雲端資料庫

1.下載SQLECTRON

連結：https://sqlectron.github.io/

2.點選Download GUI

![image](https://user-images.githubusercontent.com/103306835/166852811-4feb17c4-de3a-4ae6-9707-86d4d924385c.png)

3.點選[Add]

![image](https://user-images.githubusercontent.com/103306835/166852849-b3a844c1-af4c-4210-9261-d70319514fdc.png)

4.輸入資料->並點選Save

![image](https://user-images.githubusercontent.com/103306835/166852901-86042efb-3cd4-4172-8792-2597f2955972.png)

5.完成新增資料庫

![image](https://user-images.githubusercontent.com/103306835/166852925-058d7b87-66e0-459c-88db-bfd126c43a55.png)

6.點選[Connect]

![image](https://user-images.githubusercontent.com/103306835/166852939-cd67de6f-f1da-494e-9b5f-c6bfa06eb231.png)

7.成功連線

![image](https://user-images.githubusercontent.com/103306835/166852967-8f8f033a-8274-414a-8c4e-3de63efe2479.png)

8.查看資料庫內容

![image](https://user-images.githubusercontent.com/103306835/166852979-ae88ecef-0127-4177-8cdc-e8b0da91e7cc.png)

