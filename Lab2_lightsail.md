# 使用lightsail一鍵部署wordpress


1.搜尋Lightsail並點選

![image](https://user-images.githubusercontent.com/103306835/163427237-175a5b59-8c86-4c67-a5e8-a17fa999349d.png)

2.點選[Create Instance]

![image](https://user-images.githubusercontent.com/103306835/163427306-30626b3f-3d54-41ed-abd3-e26aac6db0b2.png)

3.點選Linux/Unix

![image](https://user-images.githubusercontent.com/103306835/163427368-7b178025-316b-432f-9350-9dec89e9debf.png)

4.選擇區域:us-east-1

![image](https://user-images.githubusercontent.com/103306835/163427664-65638bc2-e669-47ec-ac3d-a9003ead426d.png)

5.選擇wordpress

![image](https://user-images.githubusercontent.com/103306835/163427724-85ab7af0-b42e-427f-96cd-4f36fc360215.png)

6.選擇Create instance

![image](https://user-images.githubusercontent.com/103306835/163427797-61a1a040-7064-48dc-8246-c778da9a4ec1.png)

7.點選SSH Console(如下圖紅框標記處)

![image](https://user-images.githubusercontent.com/103306835/163427889-499a477a-f91d-48bd-90c5-38f62a877a70.png)

8.成功登入後，請輸入指令  
cat /bitnami_application_password

9.取得後臺密碼

![image](https://user-images.githubusercontent.com/103306835/163428286-4c860072-8f32-40bf-8bc4-7bee7bfb619b.png)

10.取得wordpress網站IP

![image](https://user-images.githubusercontent.com/103306835/163428437-4116eea1-dee5-46fb-8186-8640aaf919a7.png)

11.開啟空白網頁，將以下{public-ip-address}更換成wordpress網站IP
http://{public-ip-address}/wp-admin/
  
以本範例舉例，wordpress後臺網址即為http://54.145.230.103/wp-admin/
  
12.開啟登入網頁，並輸入帳密
  
![image](https://user-images.githubusercontent.com/103306835/163429102-55bd7423-6527-4f38-8d51-ed2880133f39.png)

13.點選login 
  
![image](https://user-images.githubusercontent.com/103306835/163429332-02704d24-7682-49b7-b281-27de2d9f4150.png)

14.成功進入管理後臺
  
![image](https://user-images.githubusercontent.com/103306835/163429411-2a95463c-6890-4857-955b-5ba036e81d9a.png)






# 當lightsail內建的ssh console太慢的替代方案

>>>>>Windows 

1.點選Download default key
![image](https://user-images.githubusercontent.com/103306835/163431871-e4dc4aec-96b0-467d-abd6-4624400a2bf7.png)

  
2.下載putty與puttygen，連結：https://www.chiark.greenend.org.uk/~sgtatham/putty/
  
![image](https://user-images.githubusercontent.com/103306835/163430395-89623212-612a-40d0-88a9-158f74aaf133.png)

3.下載putty
  
![image](https://user-images.githubusercontent.com/103306835/163430875-843e6b3b-7e3f-4fba-8f98-c159d72054dd.png)

4.下載puttygen
![image](https://user-images.githubusercontent.com/103306835/163430692-677b1733-5d81-4990-ac39-70fc472a0b33.png)

5.安裝putty及puttygen
  
6.開啟puttygen
  
7.點選Load

![image](https://user-images.githubusercontent.com/103306835/163431202-18007e3b-9cdd-40d9-9425-840af61532da.png)

8.選擇步驟1下載的ppk檔
  
9.點選Save private key

![image](https://user-images.githubusercontent.com/103306835/163431426-89e396bf-3463-4a4b-a225-5d0d3b8fec4d.png)

10.另存成ppk檔，名稱自訂

![image](https://user-images.githubusercontent.com/103306835/163432195-8c9be081-838f-4166-a65f-8f3df5e5f18c.png)

11.開啟putty
  
![image](https://user-images.githubusercontent.com/103306835/163432424-0a1aaabf-5148-48c2-9148-5ad629281157.png)
 
12.輸入wordpress網站的ip

![image](https://user-images.githubusercontent.com/103306835/163432528-060a371a-3fcb-433e-9263-59b2081519df.png)

13.選擇Auth

![image](https://user-images.githubusercontent.com/103306835/163434226-2b49f5b0-a44a-4695-8f73-639ff793eebb.png)

14.選擇Browse

![image](https://user-images.githubusercontent.com/103306835/163434316-c72b0374-6f6d-4204-94ee-933deac04503.png)

15.選擇步驟10產生的ppk檔

![image](https://user-images.githubusercontent.com/103306835/163434561-59e19b82-fc67-4552-b61a-cfd272c72738.png)


16.點選Open

![image](https://user-images.githubusercontent.com/103306835/163432628-80ec56fb-c32a-400f-921c-48cab1727adf.png)

以下步驟請回到實作步驟8
  

# mac連接ssh console的方法

1.點選Download default key
![image](https://user-images.githubusercontent.com/103306835/163431871-e4dc4aec-96b0-467d-abd6-4624400a2bf7.png)

  
2.打開 Mac 內建的終端機（Terminal）

![image](https://user-images.githubusercontent.com/103306835/163433247-df312ad6-cc3d-4037-ab9b-42c1de5e1d60.png)

3.把密鑰檔「拖曳」到終端機的輸入位置放開

以下步驟請回到實作步驟8
  
Reference：

1.lightsail on aws：https://lightsail.aws.amazon.com/ls/docs/en_us/articles/amazon-lightsail-tutorial-launching-and-configuring-wordpress

2.windows putty connect to lightsail ssh console：https://docs.aws.amazon.com/zh_tw/AWSEC2/latest/UserGuide/putty.html

3.使用 Mac 內建的終端機進行連線：https://diary.taskinghouse.com/posts/310691-ssh-connection-amazon-ec2/
