#  Lab 雲端資料庫實作（RDS）

建立雲端資料庫(MariaDB)


# 實施架構

![image](https://user-images.githubusercontent.com/103306835/163801264-6e21b6ee-5fd6-4d29-9299-ce325e50b463.png)


# 實施步驟

![image](https://user-images.githubusercontent.com/103306835/174926615-7b29f3b1-3db5-412d-a479-1b06fdd59d37.png)

# Step1：新增資料庫引擎

1-1.點選[RDS]

![image](https://user-images.githubusercontent.com/103306835/163803017-e1549f7c-5797-45a9-b95b-9d983d68f74a.png)

1-2.點選[Create database]

![image](https://user-images.githubusercontent.com/103306835/163803047-09733678-90da-4267-9647-d5de43f809f9.png)

1-3.選擇[Standard create]

![image](https://user-images.githubusercontent.com/103306835/163803092-e27c6bfd-f82f-484a-a42d-922539a348a4.png)

1-4.選擇[MYSQL]

![image](https://user-images.githubusercontent.com/103306835/174926048-c19567a6-5af9-4754-a07b-a237bce1dbfb.png)

1-5.點選[免費方案]

![image](https://user-images.githubusercontent.com/103306835/174926081-eca6082d-9892-43f8-b13c-6dafc716a4bc.png)

# Step2：設定帳號密碼

2-1.設定資料庫與帳密

![image](https://user-images.githubusercontent.com/103306835/174926144-aa71531c-3760-436e-aef2-c08ef96098f7.png)

2-2.選擇Default VPC

![image](https://user-images.githubusercontent.com/103306835/174926197-d6c88c7b-8147-4b8b-bfb5-533ea82b4d9a.png)

2-3.展開[其他組態]

![image](https://user-images.githubusercontent.com/103306835/174926234-281b0b69-910a-4294-a6d3-072077e7fd74.png)

2-4.資料庫名稱設定為lab

![image](https://user-images.githubusercontent.com/103306835/174926277-7d166d07-25f8-466b-a949-72eca5336a7d.png)

2-5.取消勾選[啟用自動備份]與[啟用加密]

![image](https://user-images.githubusercontent.com/103306835/174926296-2dd11b9e-17c5-4ce0-b1c6-78e2f4ea853b.png)

2-6.點選[建立資料庫]

![image](https://user-images.githubusercontent.com/103306835/174926323-ccd1a9bf-d26b-435b-8aab-a5433f6a2da0.png)

2-7.等待建立

![image](https://user-images.githubusercontent.com/103306835/174926353-0ff68726-c3e2-4eb4-9527-d71c4be41459.png)

# Step3：開啟sqlectron

3-1.下載SQLECTRON
連結：[https://dev.mysql.com/downloads/file/?id=497505](https://sqlectron.github.io/)
![image](https://user-images.githubusercontent.com/103306835/174924724-a297249c-39c2-4e76-85b5-a11428a07f23.png)

3-2.點選[Download GUI]

![image](https://user-images.githubusercontent.com/103306835/174925017-74604d28-cea9-41c6-8cfe-e5c98657b2ba.png)

3-3.選擇合適的版本下載

![image](https://user-images.githubusercontent.com/103306835/174925057-f2f2c93d-0cbc-4f16-90a0-a1fac8bee52a.png)

3-4.點選[Add]

![image](https://user-images.githubusercontent.com/103306835/174925238-05dfdadc-6e89-456d-808e-84cbad1f3a38.png)

3-5.回到RDS 點選資料庫

![image](https://user-images.githubusercontent.com/103306835/174925273-d94118ee-f81d-491d-849b-e83e402bed33.png)

3-6.點選[VPC安全群組]

![image](https://user-images.githubusercontent.com/103306835/174925329-5683913e-8371-4bfb-9bbc-eba72ccb1fc3.png)

3-7.點選[傳入規則]

![image](https://user-images.githubusercontent.com/103306835/174925390-9eb38751-513f-47d8-941d-441e3faf631d.png)

3-8.點選[編輯傳入規則]

![image](https://user-images.githubusercontent.com/103306835/174925424-5558b43e-3803-4f93-b256-1dc39e04e1a7.png)

3-9.點選[刪除]

![image](https://user-images.githubusercontent.com/103306835/174925517-ccbd497d-94ea-4d5f-814e-4811ff8188d9.png)

3-10.點選[新增規則]

![image](https://user-images.githubusercontent.com/103306835/174925548-601f4cfb-bf63-4e9d-9e57-d1ba308236b0.png)


3-11.設定MYSQL類型；來源：0.0.0.0/0

![image](https://user-images.githubusercontent.com/103306835/174925592-b6639c01-ddc0-4e93-9b26-611dab6f0f4c.png)

3-12.點選[儲存規則]

![image](https://user-images.githubusercontent.com/103306835/174925617-349a2e17-a93b-49df-953d-2a68bca69e01.png)

# Step4：輸入連線資訊

4-1.輸入連線資訊

![image](https://user-images.githubusercontent.com/103306835/174925811-4fcef37d-c5e3-45f7-a573-19fc49266add.png)

4-2.點選[Save]

![image](https://user-images.githubusercontent.com/103306835/174925843-589b683b-410a-4623-86b6-a096284c0341.png)

# Step5：進入資料庫

5-1.點選[Connect]

![image](https://user-images.githubusercontent.com/103306835/174925896-9dbf6207-5c6f-434e-8d75-3240c459840b.png)

5-2.成功登入

![image](https://user-images.githubusercontent.com/103306835/174925910-31590e37-b595-45bb-b3dc-3cd64e59a951.png)
