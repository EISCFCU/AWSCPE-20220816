

# 情境

森森購物考量到未來資訊服務的使用彈性，預計第一階段先將公司自建的POS系統資料庫遷移到 Amazon Web Services平台。
由於POS資料庫從SQLite資料庫更換為MYSQL資料庫(RDS)，資訊主管指示A團隊小組將先將既有POS資料匯出(CSV檔)，上傳雲端儲存桶(S3)做備份，並同時直接存入MYSQL資料庫。


另外，這一系列的上傳備份與資料庫異動的動作(Log)，主管希望記錄在No sql的資料庫(Dynamodb)中，方便後續追蹤備份與轉換，請協助進行POS系統的資料庫雲端搬遷。




# 使用環境(請先Start Lab)
AWS Academy Learner Lab-Foundation Services

AWS Academy登入連結：https://awsacademy.instructure.com/login/canvas


# 使用資源(請先下載)

1.sqlectron：https://sqlectron.github.io/

1-1.下載sqlectron，連結：https://sqlectron.github.io/

1-2.點選GUI

![image](https://user-images.githubusercontent.com/103306835/167972790-e833bc5d-f8e2-4208-8ffc-98ed2127361c.png)

1-3.選擇對應的GUI版本

![image](https://user-images.githubusercontent.com/103306835/167972992-370792de-f0c7-4cdd-ac17-d7da570a00ef.png)


1-4.下載後解壓縮

1-5.開啟sqlectron

![image](https://user-images.githubusercontent.com/103306835/167973456-485b1160-8c1f-49a7-96b2-e061cd966c9c.png)


2.csv檔：https://github.com/EISCFCU/AWSCPE-20220816/blob/main/product_sale.csv

![image](https://user-images.githubusercontent.com/103306835/167974685-38cd4e04-5d51-4f16-a0e3-8990c2e3b567.png)


3.lambda.zip：https://github.com/EISCFCU/AWSCPE-20220816/blob/main/s3-csv-trans-cf42bb56-a71d-49da-961d-684300dea41a.zip

![image](https://user-images.githubusercontent.com/103306835/167975240-880be2da-44b8-4036-af1b-f4e56ccf6e28.png)


# 實作架構

![image](https://user-images.githubusercontent.com/103306835/167766069-215b877d-fb67-447d-8b91-e377b066bc69.png)

# 操作步驟

![image](https://user-images.githubusercontent.com/103306835/167766102-f3d54bac-f440-480f-a0fc-1d053e320537.png)


# Step1：建立雲端資料庫

1.點選[RDS]

![image](https://user-images.githubusercontent.com/103306835/167766127-cae8ab04-b83c-4f5d-a791-3f059dadcf85.png)

2.點擊[創建資料庫]

![image](https://user-images.githubusercontent.com/103306835/167766236-ae9ba7e5-fa3b-4c57-ab36-c644b9d9489f.png)

3.選擇[標準建立]

![image](https://user-images.githubusercontent.com/103306835/167766288-839d96b3-6185-45e1-af76-a413e9ad8e85.png)

4.選擇 MySQL

![image](https://user-images.githubusercontent.com/103306835/167766328-da406717-53bf-411f-9bf1-cc7ab553c738.png)

5.選擇Free Tier

![image](https://user-images.githubusercontent.com/103306835/167766349-95792490-a82f-47df-894a-c56089a15933.png)

6.資料庫帳密設定

資料庫實例標識符 ：lab3-db
主要使用者名稱：admin
主要密碼：pass1234
確認密碼：pass1234

![image](https://user-images.githubusercontent.com/103306835/167766434-0131cdc5-b98e-4661-90bb-396f7e95ba04.png)

7.資料庫執行個體類別->選擇 db.t3.micro

![image](https://user-images.githubusercontent.com/103306835/167766461-60aa2d0f-4599-49f3-b5bb-7bed15197297.png)

8.儲存體類型：General Purpose (SSD)  配置儲存：20

![image](https://user-images.githubusercontent.com/103306835/167766490-fd724508-e8bc-4f97-83b7-6f23a6897e60.png)

9.Virtual Private Cloud (VPC)：Defalt VPC

![image](https://user-images.githubusercontent.com/103306835/167766517-57553bc5-d19a-4419-823b-97c70a9ab943.png)

10.公開存取：是(方便後續本機連線資料庫)

![image](https://user-images.githubusercontent.com/103306835/167766566-0839807e-e34f-48f5-85e1-53fd180f35cb.png)

11.選擇現有的VPC安全群組：default

![image](https://user-images.githubusercontent.com/103306835/167766581-808d6a88-a1fc-425e-8624-7f9ecddb1897.png)

12.展開其他組態

![image](https://user-images.githubusercontent.com/103306835/167766611-15a78b1f-23f2-4386-864f-77cd9843dbac.png)

13.初始資料庫名稱：lab

![image](https://user-images.githubusercontent.com/103306835/167766656-79427aef-0124-4a53-8286-a33dbbd1db66.png)

14.取消勾選[啟用自動備份]與[啟用增強監控]

![image](https://user-images.githubusercontent.com/103306835/167766694-8d49547d-79a8-470d-9907-117f4952f289.png)

15.點選[建立資料庫]

![image](https://user-images.githubusercontent.com/103306835/167766717-a2cb1fbe-46bf-4352-80bf-f392b3b30337.png)


16.等待建立完成

![image](https://user-images.githubusercontent.com/103306835/167766745-8e681435-d121-4c79-8327-c5d1175bf730.png)


#  Step2：建立儲存桶(S3) 

1.搜尋S3

![image](https://user-images.githubusercontent.com/103306835/167769668-4eaab0f3-5402-4649-8e7b-4fbd3ebe00dd.png)

2.點選[建立儲存貯體]

![image](https://user-images.githubusercontent.com/103306835/167769872-91f14e76-8858-4ea0-8ec7-5f5c0adf6e86.png)

3.輸入儲存貯體名稱(自訂)

![image](https://user-images.githubusercontent.com/103306835/167769912-c9e36a53-975e-4f91-9583-32093c65ffd4.png)

4.取消勾選封鎖 並勾選我確認…

![image](https://user-images.githubusercontent.com/103306835/167769984-2ee35fc9-242e-4e29-a871-7812da42c6fd.png)

5.點選[建立儲存貯體]

![image](https://user-images.githubusercontent.com/103306835/167770037-bfd865b8-5a7d-480d-bac5-f32d2fdcad18.png)

6.成功建立儲存貯體

![image](https://user-images.githubusercontent.com/103306835/167770060-f7506a67-a51d-468b-9328-bb5dbb7f017b.png)


# Step3：建立No sql 資料庫(Dynamodb)

1.搜尋Dynamodb

![image](https://user-images.githubusercontent.com/103306835/167770107-cb1f5f48-b051-4df0-afb7-d33eb7f8a519.png)

2.點選Dynamodb

![image](https://user-images.githubusercontent.com/103306835/167770124-1929017b-ab6d-427b-b387-d22d9478cbad.png)

3.點選[建立資料表]

![image](https://user-images.githubusercontent.com/103306835/167976455-b65b225b-7d82-40f4-8062-91fcab93fbdd.png)

4.輸入資料表名稱：S3-log-20220512

分區索引：StartTime(字串) 
排序索引：FileName(字串)

![image](https://user-images.githubusercontent.com/103306835/167770212-50a8ee33-eb8d-4cea-93a5-2aeec8d45916.png)

5.點選[建立資料表]

![image](https://user-images.githubusercontent.com/103306835/167770238-292d66e0-7a03-4704-a1e5-032dccd2f8dd.png)

6.成功建立S3-log-20220512資料表(狀態：作用中)

![image](https://user-images.githubusercontent.com/103306835/167770292-95bdb192-5e3e-42db-983d-9bcb3da1ad7d.png)

# Step4：建立 Lambda

1.搜尋Lambda 並點選

![image](https://user-images.githubusercontent.com/103306835/167771397-181ee0c8-8457-4796-bcb9-b905566e6f0f.png)

2.點選[建立函式]

![image](https://user-images.githubusercontent.com/103306835/167771424-bd70e728-803a-47d2-ae74-4ab7e38ec6bc.png)

3.點選從頭開始撰寫

![image](https://user-images.githubusercontent.com/103306835/167771451-18b25a7e-a9ad-4402-a749-1bd38287184a.png)

4.函數名稱自訂

![image](https://user-images.githubusercontent.com/103306835/167771470-47c79641-ed89-462c-b70c-7a2b9dd4a8a9.png)

5.執行時間：Python3.8

![image](https://user-images.githubusercontent.com/103306835/167771488-7511facc-0732-4e5e-bad9-a5439f968f56.png)

6.選擇使用現有的角色：LabRole

![image](https://user-images.githubusercontent.com/103306835/167771514-fa65c179-449b-4296-b4e4-35f5794b7861.png)

7.點選[建立函式]

![image](https://user-images.githubusercontent.com/103306835/167771541-d9908fd1-f5ae-433a-a0fd-fbe0e695446e.png)

8.成功建立函式

![image](https://user-images.githubusercontent.com/103306835/167771595-d50159a0-ff44-47e8-b8ac-56c8fb7307a0.png)

9.點選[新增觸發]

![image](https://user-images.githubusercontent.com/103306835/167771622-3bffe8a5-3a0d-41be-93bc-2a36353a3a40.png)

10.選擇S3

![image](https://user-images.githubusercontent.com/103306835/167771658-22e7a041-7f0c-4bd6-b0d0-6f308dfc4c02.png)

11.選擇步驟2建立的S3

![image](https://user-images.githubusercontent.com/103306835/167771692-6ed70d5a-f84d-4633-8773-1911d3852adf.png)

12.尾碼為.csv

![image](https://user-images.githubusercontent.com/103306835/167771779-78c578aa-cf99-466e-9855-232f4e374444.png)

13.勾選我確認

![image](https://user-images.githubusercontent.com/103306835/167771809-d086e195-17b1-49f6-a220-36639cab1a3e.png)

14.點選[新增]

![image](https://user-images.githubusercontent.com/103306835/167771831-ac1ee642-3b2a-4725-ab9c-8c9f51a0b913.png)

15.完成觸發設定

![image](https://user-images.githubusercontent.com/103306835/167771852-0e7b6781-56d9-43e9-bce2-f97c9720edc2.png)

16.往下滑動視窗

![image](https://user-images.githubusercontent.com/103306835/167771889-6e77b295-983e-45d8-908c-36e61a0a01b3.png)

17.找到上傳按鈕 並點選

![image](https://user-images.githubusercontent.com/103306835/167771921-21dabddc-641d-4a82-b5db-0ee8ac4abaaa.png)

18.選擇.zip 檔案

![image](https://user-images.githubusercontent.com/103306835/167771965-92f7aa09-8e48-42cc-8d64-a7d6d8561885.png)

19.選擇[上傳]

![image](https://user-images.githubusercontent.com/103306835/167771989-34a8ca5f-e89e-4acf-b914-01cb101ec941.png)

20.選擇[儲存]

![image](https://user-images.githubusercontent.com/103306835/167772024-21e9b193-33af-4c6f-bdec-f0ca0247e5a4.png)

21.更改6、12、19行資料(RDS Endpoint、 S3名稱、Dynamodb資料表名稱)

![image](https://user-images.githubusercontent.com/103306835/167772055-bd19dddc-6886-4914-8901-8b547fa7a862.png)

22.點選[Deploy]

![image](https://user-images.githubusercontent.com/103306835/167772081-bfdb61a0-71f1-48c9-94d8-14f629be5c6e.png)

23.點選[Test]

![image](https://user-images.githubusercontent.com/103306835/167772105-5fc3f12f-4dbd-414c-9227-95f64cbbbaef.png)

24.輸入事件名稱：test

![image](https://user-images.githubusercontent.com/103306835/167772135-3dd96163-7fc2-471e-9580-8368fa4f4e63.png)

25.貼上測試語法(刪除下方的1-5行程式碼再貼上測是語法)

![image](https://user-images.githubusercontent.com/103306835/167772156-7a65780a-db24-492f-8272-abc8647b60b7.png)

```
{
  "Records": [
    {
      "eventVersion": "2.0",
      "eventSource": "aws:s3",
      "awsRegion": "us-east-1",
      "eventTime": "1970-01-01T00:00:00.000Z",
      "eventName": "ObjectCreated:Put",
      "userIdentity": {
        "principalId": "EXAMPLE"
      },
      "requestParameters": {
        "sourceIPAddress": "127.0.0.1"
      },
      "responseElements": {
        "x-amz-request-id": "EXAMPLE123456789",
        "x-amz-id-2": "EXAMPLE123/5678abcdefghijklambdaisawesome/mnopqrstuvwxyzABCDEFGH"
      },
      "s3": {
        "s3SchemaVersion": "1.0",
        "configurationId": "testConfigRule",
        "bucket": {
          "name": "my-s3-bucket",
          "ownerIdentity": {
            "principalId": "EXAMPLE"
          },
          "arn": "arn:aws:s3:::example-bucket"
        },
        "object": {
          "key": "product_sale.csv",
          "size": 1024,
          "eTag": "0123456789abcdef0123456789abcdef",
          "sequencer": "0A1B2C3D4E5F678901"
        }
      }
    }
  ]
}

```

26.修改23、27行的s3 bucket name及30行的object name(product_sale.csv)

![image](https://user-images.githubusercontent.com/103306835/167772264-ca6b2c49-b1ec-4a0e-87ac-02b7b44303d6.png)

27.點選[儲存]

![image](https://user-images.githubusercontent.com/103306835/167772291-4a1aab91-173e-46bd-be5b-401ca7d816d3.png)

28.成功儲存測試事件

![image](https://user-images.githubusercontent.com/103306835/167772311-65e1c064-35c8-48d3-bfd4-50a74880d99a.png)


# Step5： RDS連線到雲端資料庫 

1.點選lab-3

![image](https://user-images.githubusercontent.com/103306835/167766745-8e681435-d121-4c79-8327-c5d1175bf730.png)

2.點選Type為[EC2 Security Group-Inbound]

![image](https://user-images.githubusercontent.com/103306835/167972480-8e5b8732-c203-4f4f-bfa0-a0f59d9bcdc9.png)

3.點選輸入規則

![image](https://user-images.githubusercontent.com/103306835/167972529-ad4723f2-af17-49db-a326-281f6707863d.png)

4.修改輸入規則來源為0.0.0.0/0(任何一台電腦都可以登入RDS)


5.下載sqlectron(已經下載sqlectron請忽略此步驟)

https://sqlectron.github.io/

6.點選GUI

![image](https://user-images.githubusercontent.com/103306835/167972790-e833bc5d-f8e2-4208-8ffc-98ed2127361c.png)

7.選擇對應的GUI版本

![image](https://user-images.githubusercontent.com/103306835/167972992-370792de-f0c7-4cdd-ac17-d7da570a00ef.png)


8.下載後解壓縮

9.開啟sqlectron

![image](https://user-images.githubusercontent.com/103306835/167973456-485b1160-8c1f-49a7-96b2-e061cd966c9c.png)

10.啟動sqlectron

![image](https://user-images.githubusercontent.com/103306835/167772383-1ef8b6b7-cf0b-4e5e-8adb-f7b20a8af95d.png)

11.點選[Add]

![image](https://user-images.githubusercontent.com/103306835/167772414-864e76d5-2668-4d9f-9156-9c608e249a16.png)

12.輸入資料庫連線資訊

![image](https://user-images.githubusercontent.com/103306835/167973751-038a2063-7c4e-464d-b90a-97d5dae6149c.png)

13.點選[Save]

![image](https://user-images.githubusercontent.com/103306835/167772462-cc9df0e3-a37b-440b-b39b-8908b3fc7339.png)

5.點選[Connect]

![image](https://user-images.githubusercontent.com/103306835/167772490-8df9b4ea-8487-4406-815e-e8f0ff6e5f37.png)

6.輸入SQL語法

![image](https://user-images.githubusercontent.com/103306835/167772541-fd18e851-2f38-4801-ac0e-f396cce2463a.png)

```
CREATE TABLE IF NOT EXISTS product_sale_summary(
  product_name varchar(255),
  quantity int,
  amount double,
  revenue double,
  file_name varchar(255)
  );
```

7.點選[Execute]

![image](https://user-images.githubusercontent.com/103306835/167772581-757715bc-4e4c-4cee-9783-c5a58556d56a.png)

8.在lab資料庫按右鍵 並選擇Refresh Database

![image](https://user-images.githubusercontent.com/103306835/167772601-749f3a01-2cae-4c52-9675-45493e82ba9e.png)

9.更新後 顯示資料表

![image](https://user-images.githubusercontent.com/103306835/167772615-ecb083b8-0707-4d9f-8e03-eb7844f481f5.png)

# Step6： S3 上傳CSV

1.搜尋S3 並點選

![image](https://user-images.githubusercontent.com/103306835/167772642-4b999c7a-5f85-4b12-9a6f-acec77ddb86d.png)

2.點選步驟2新增的S3

![image](https://user-images.githubusercontent.com/103306835/167772737-c5e4f736-a068-4bca-a6b6-cfb629d4cf43.png)

3.點選[上傳]

![image](https://user-images.githubusercontent.com/103306835/167772765-6dd21faa-1af6-45a1-9593-087ebaf9f863.png)

4.點選[新增檔案]

![image](https://user-images.githubusercontent.com/103306835/167772792-e4ee8757-03bc-4157-a7ef-d7ce7154fff1.png)

5.將product_sale.csv上傳

6.點選[上傳]

![image](https://user-images.githubusercontent.com/103306835/167772812-3a734625-868c-4968-beaf-9046ce3f3473.png)

7.成功上傳

![image](https://user-images.githubusercontent.com/103306835/167772844-2c4e36f9-d1e0-4e7d-849f-d4e3bf877d08.png)

# 查看資料庫

1.點選Reconnect圖示

![image](https://user-images.githubusercontent.com/103306835/167772911-af089701-9582-436b-8c93-fc3ee5101f4b.png)

2.點選資料表

![image](https://user-images.githubusercontent.com/103306835/167772927-b1e96832-045f-4e10-b1b6-3d00830993cb.png)

3.查詢目前資料表的資料欄位

![image](https://user-images.githubusercontent.com/103306835/167773066-11940d04-575b-4cb0-a776-e2762b580d4d.png)

# 查看Dynamodb

1.點選步驟3建立的Dynamodb資料表

![image](https://user-images.githubusercontent.com/103306835/167773105-30b08b48-181c-4091-ab75-353abbec99a7.png)

2.點選[探索資料表項目]

![image](https://user-images.githubusercontent.com/103306835/167773128-707b652c-9a8e-4895-895d-e11a5ee6f46d.png)

3.點選[執行]

![image](https://user-images.githubusercontent.com/103306835/167773156-125c1051-8d4f-4226-9f59-f856effd9d83.png)

4.查看結果

![image](https://user-images.githubusercontent.com/103306835/167773174-a684f16c-48b1-4e00-a9b0-d79ae0168a5b.png)

# 程式運作錯誤測試

1.回到Lambda點選Test 查看錯誤訊息

![image](https://user-images.githubusercontent.com/103306835/167773229-c95a3713-4023-40eb-afdf-429baf0d57a9.png)

2.目前測試出來的錯誤訊息

=>No suchkey 表示S3找不到檔案或是桶子名字輸入錯誤

=>PutItem error表示Dynamodb資料表名字或欄位輸入錯誤















