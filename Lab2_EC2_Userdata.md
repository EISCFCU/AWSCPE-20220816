# 使用EC2搭配自動化指令架設Wordpress
PeggyWang 20220414 Update

Contact mail:eiscfcu@gmail.com

# 步驟1：新增EC2執行個體
1.搜尋EC2

![image](https://user-images.githubusercontent.com/103306835/163394416-ae8cb55f-5498-4f54-bc86-9873bb2d0831.png)

2.點選[EC2]

![image](https://user-images.githubusercontent.com/103306835/163394462-976d94b2-0f0b-4c5d-a4a5-e33cdfe31584.png)

3.點選[Instances執行個體]

![image](https://user-images.githubusercontent.com/103306835/163394525-504888e6-32a2-40b2-b4a2-f9b33483a5bd.png)

4.點選[Launch Instances啟動新執行個體]

![image](https://user-images.githubusercontent.com/103306835/163394805-610e7c8b-9731-4155-afb2-a0a75b6b5837.png)

5.選擇作業系統(這裡稱為AMI映像檔)

![image](https://user-images.githubusercontent.com/103306835/163394898-f1b46df6-45ad-4c2d-8a0f-6c33438aace8.png)

6.選擇機器型態

![image](https://user-images.githubusercontent.com/103306835/163395011-a6d59598-5708-4b43-bac4-f912e86a3046.png)

# 步驟2：個體存放位置與安全性設定
放在雲端上的電腦的安全設定；Subnet設定所屬us-east-1a
![image](https://user-images.githubusercontent.com/103306835/163397822-791fd52c-9a95-498e-9c48-46dd03af293c.png)


# 步驟3：上傳網站檔案

1.至Advanced Details 點選[As  file]

![image](https://user-images.githubusercontent.com/103306835/163395634-fc48c632-8fea-4b04-9d74-f6b721e6791f.png)

2.點選[選擇檔案]

![image](https://user-images.githubusercontent.com/103306835/163395671-818f8b16-e514-458c-b277-b9eb24d255dd.png)

3.選擇wordpress.aws.sh(檔案連結:https://github.com/EISCFCU/wordpress_on_aws_EC2/blob/main/wordpress.aws.sh)

![image](https://user-images.githubusercontent.com/103306835/163395733-1b9e1d90-54e5-4c63-b50b-8def15d500b6.png)

Userdata裡的指令
.Update packages and install LAMP server

.Download WordPress and extract files to var/www/html folder

.Install secure MySQL , assign root password, create database ,user and password

.Update wp-config.php with provided database value

.Enable .htaccess in Apache config httpd.conf

.Enable auto start of Apache and MySQL server

4.點選[Next：Add Storage]

![image](https://user-images.githubusercontent.com/103306835/163395804-e6c5567a-a48d-4e05-aed3-65a85539fea6.png)

5.點選[Next：Add Tags]
增加硬碟(EBS)，EBS可以視為電腦的 D槽或是E槽。
![image](https://user-images.githubusercontent.com/103306835/163397961-29406d0f-9ddc-41ad-b6dd-a1163b4ad86f.png)

# 步驟4：命名電腦名稱

1.增加標籤(Tags)

![image](https://user-images.githubusercontent.com/103306835/163396152-15aaefdf-6263-4357-bed8-a4fca274c222.png)

2.點選 [Next：Configure Security Group]

![image](https://user-images.githubusercontent.com/103306835/163396240-b1a06423-6e84-4609-8b46-95e34d1713af.png)

# 步驟5：安全群組與開通網頁瀏覽權限

1.輸入安全群組(Security Group)

![image](https://user-images.githubusercontent.com/103306835/163396374-9dd8fdf0-5f19-4ab1-94ea-7ddb6f28e3de.png)

2.點選[Add Rule]

![image](https://user-images.githubusercontent.com/103306835/163396460-13b61da2-316a-4465-a00b-a53c46f901e9.png)

3.新增Type=HTTP,HTTPS

![image](https://user-images.githubusercontent.com/103306835/163396564-c2d616fc-dd0e-4697-a0d8-3d4f0673e554.png)

4.點選[Review and Launch]

![image](https://user-images.githubusercontent.com/103306835/163396633-63c07c2f-9001-4ea6-b976-9fc9d51164f9.png)

# 步驟6：啟動Wordpress伺服器

1.Launch EC2
![image](https://user-images.githubusercontent.com/103306835/163396764-455e4898-76f9-4b61-be20-f8b9647cd775.png)

2.勾選I acknowledge….
![image](https://user-images.githubusercontent.com/103306835/163396828-c3ee4b27-4c85-469e-bf89-528c1b341f40.png)

3.點選[Launch instances]
![image](https://user-images.githubusercontent.com/103306835/163399504-ecba57f5-554c-4b08-8cce-155c35fd6f30.png)

4.建置狀態
![image](https://user-images.githubusercontent.com/103306835/163399584-4dc8b797-a8fd-42d7-a1b1-8f59e149087a.png)

5.點選[View Instances]
![image](https://user-images.githubusercontent.com/103306835/163399664-dc7b7de1-f18b-4ca9-81b6-7ecc2954603f.png)

6.等待Status轉為2/2 checks
![image](https://user-images.githubusercontent.com/103306835/163399726-b86aa254-6f7a-4a81-a009-1fdab2012d0f.png)

# 步驟7：登入你的Wordpress管理後臺

1.勾選Wordpress
![image](https://user-images.githubusercontent.com/103306835/163399889-1a9b3e2e-8107-4540-9493-c75408ac6551.png)

2.點選Public IPv4 address的open address連結
![image](https://user-images.githubusercontent.com/103306835/163400022-b06e5736-adbb-4d9b-ae43-216e6dc4f57c.png)

# 有可能會出現以下錯誤畫面
![image](https://user-images.githubusercontent.com/103306835/163400067-6d06dda1-2f50-46c1-8ea7-875947f4b42f.png)

# 解決方法
將https的s拿掉 網址最後方打上:80
![image](https://user-images.githubusercontent.com/103306835/163400184-57376b83-bae9-46f0-9441-87f4ec1c0be1.png)

3.出現以下畫面即成功開啟wordpress
![image](https://user-images.githubusercontent.com/103306835/163400333-0f10b86d-667d-43ce-9c15-ab3503eceda2.png)

4.語系選擇繁體中文，並按繼續
![image](https://user-images.githubusercontent.com/103306835/163400400-0d7d5a46-e289-41eb-ba61-38f96885af6d.png)

5.網站標題、使用者名稱及密碼可以自訂
![image](https://user-images.githubusercontent.com/103306835/163400479-28f2ef3c-f09e-489d-ae96-0b6fd2eb98d9.png)

6.勾選確認密碼
![image](https://user-images.githubusercontent.com/103306835/163400543-042e4310-7214-4e95-b6b3-641f32b400e8.png)

7.輸入電子郵件
![image](https://user-images.githubusercontent.com/103306835/163400604-72b0527b-c3b3-4cfb-97ac-ea9843a96857.png)

8.點選[安裝wordpress]
![image](https://user-images.githubusercontent.com/103306835/163400670-e68f1812-d9b2-4e60-a4a6-0fea00949c8e.png)

9.點選[登入]
![image](https://user-images.githubusercontent.com/103306835/163400713-07b6d360-52a6-4d5c-834c-d82aa68b2286.png)

10.輸入帳密並登入
![image](https://user-images.githubusercontent.com/103306835/163400766-31c08108-ff7b-4ad1-8f98-bfa2a92ba1b1.png)

11.點選[文章]
![image](https://user-images.githubusercontent.com/103306835/163400837-9f6814da-8532-467b-afad-188d7b170ab8.png)

12.滑鼠滑動至[網站第一篇文章]
![image](https://user-images.githubusercontent.com/103306835/163400884-d0339522-11d4-428b-8092-90c01041b999.png)

13.點選[檢視]
![image](https://user-images.githubusercontent.com/103306835/163400927-9cb48656-8a68-4e0a-be51-e6016dcb431c.png)

14.你的Wordpress網站首頁
![image](https://user-images.githubusercontent.com/103306835/163400979-786c03df-182c-46b6-9f7f-e887e09ad195.png)
