#  Lab 雲端資料庫實作（RDS）

建立雲端資料庫(MariaDB)


# 實施架構

![image](https://user-images.githubusercontent.com/103306835/163801264-6e21b6ee-5fd6-4d29-9299-ce325e50b463.png)


# 實施步驟

![image](https://user-images.githubusercontent.com/103306835/163802973-d8e2b636-f525-43f6-92cd-ffab8469f31b.png)

1.點選[RDS]

![image](https://user-images.githubusercontent.com/103306835/163803017-e1549f7c-5797-45a9-b95b-9d983d68f74a.png)

2.點選[Create database]

![image](https://user-images.githubusercontent.com/103306835/163803047-09733678-90da-4267-9647-d5de43f809f9.png)

3.選擇[Standard create]

![image](https://user-images.githubusercontent.com/103306835/163803092-e27c6bfd-f82f-484a-a42d-922539a348a4.png)

4.點選[MariaDB]

![image](https://user-images.githubusercontent.com/103306835/163803434-ecadcb9d-c670-48d4-aa4d-84ad7764bda5.png)

5.點選[Free tier]

![image](https://user-images.githubusercontent.com/103306835/163803458-9381f738-449e-40a4-a33f-8ada024320c9.png)

6.輸入[Master password]

![image](https://user-images.githubusercontent.com/103306835/163803490-85aa8c9e-91ef-4e07-8c36-093c38a77057.png)

7.點選[Create new VPC]與[Create new DB Subnet Group]

![image](https://user-images.githubusercontent.com/103306835/163803552-e2d51cc8-cf81-4ccd-8bd2-fce73b71c926.png)

8.點選[YES]

![image](https://user-images.githubusercontent.com/103306835/163803587-87e11f82-e3e0-4a95-93a0-276f6390bf13.png)

9.選擇[Password authentication]

![image](https://user-images.githubusercontent.com/103306835/163803616-4bb8530c-d9bf-49a0-97e9-d869ce4d88ca.png)

10.點選[Create database]

![image](https://user-images.githubusercontent.com/103306835/163803655-d7b37a11-acde-42d2-b2eb-bf147814c053.png)

11.等待database status為Available

![image](https://user-images.githubusercontent.com/103306835/163805846-ff2a4e5a-22b9-4274-bf4d-b36cc501bba0.png)

12.點選[database-1]

![image](https://user-images.githubusercontent.com/103306835/163805884-d51d092d-bef0-4069-a411-e24d53ed1ef5.png)

13.點選Type為[EC2 Security Group-Inbound]
更改Security group 輸入規則Port，預設RDS只有EC2可以連進來
![image](https://user-images.githubusercontent.com/103306835/163805949-dae8beaa-858c-40a5-bbf2-462373723b19.png)

14.點選輸入規則

![image](https://user-images.githubusercontent.com/103306835/163805981-32c3c012-a487-4851-ac25-be143120fecc.png)


15.複製Endpoint(資料庫連線網址)

![image](https://user-images.githubusercontent.com/103306835/163806013-ea73e308-5de7-49c3-96c7-73f7b0df2138.png)

16.下載MYSQL使用介面
連結：https://dev.mysql.com/downloads/file/?id=497505
![image](https://user-images.githubusercontent.com/103306835/163809077-0a88f99b-07fc-4dbf-aca0-a851132cee50.png)

17.點選[Next]

![image](https://user-images.githubusercontent.com/103306835/163809112-522455c0-3e7d-4fc1-8f54-5f61db849ea8.png)

18.點選[Next]

![image](https://user-images.githubusercontent.com/103306835/163809166-be1bbc64-e94a-4b71-931a-52fff0f1a641.png)

19.點選[Next]

![image](https://user-images.githubusercontent.com/103306835/163809247-f4e67394-6fa1-4a27-abfa-80946c92178c.png)

20.點選[Install]

![image](https://user-images.githubusercontent.com/103306835/163809279-8f617773-f428-452e-b066-8ea07b289ff1.png)

21.點選[Finish]

![image](https://user-images.githubusercontent.com/103306835/163809326-94a68b71-5d0c-4969-ac1a-e42ab997a380.png)


# 開啟MySQL Workbench

1.點選[Database]

![image](https://user-images.githubusercontent.com/103306835/163809386-0a82047b-3b45-48b2-b988-88466b2ba8d6.png)


2.輸入Hostname

![image](https://user-images.githubusercontent.com/103306835/163809421-05a17d91-f490-4d05-85f8-686775ab3bbd.png)

3.點選[Store in Vault]

![image](https://user-images.githubusercontent.com/103306835/163809455-c94a8738-1d7b-49c1-9707-52a8d75d7f3e.png)

4.輸入密碼

![image](https://user-images.githubusercontent.com/103306835/163809509-2ee56344-a111-4fe4-8417-a35b34e16f36.png)

5.點選OK

![image](https://user-images.githubusercontent.com/103306835/163809557-d5a48f9f-5633-4ffe-8063-f3507b806f0b.png)


6.結果呈現

![image](https://user-images.githubusercontent.com/103306835/163809599-279da6a6-2f29-40ff-8ac3-c36087f5836f.png)

