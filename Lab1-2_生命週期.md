#


# 情境

資訊主管會藍到月底就會查看公司帳單，發現存儲服務(AWS S3)的費用佔了帳單費用的75% ，主要存放交易資料備份檔案，於是會藍開始研究如何降低存儲費用，發現AWS S3上面有分很多類別，價錢也不同。
在此同時，會藍觀察到公司有一個現象。公司營運交易備份30天後便很少使用，除非突發的備援需要，60天後便不在使用。會藍發現公司交易資料備份檔如果照上述規則進行對應的設定，可以省下一筆可觀的費用。
於是吩咐小王進行以下的設定，存放在雲端上的資料，隔30天放置在S3 IA，基於公司資料忠實定期紀錄，隔60天不進行刪除而是移到glacier 裡面供留存紀錄。
請協助小王完成S3的設定。

# 實施架構

![image](https://user-images.githubusercontent.com/103306835/184654332-bf4e88ed-3814-452f-82ff-e90a10cfc74e.png)


# 實施流程


#

1.點選fcu-20210302(名稱自訂)的Bucket name

![image](https://user-images.githubusercontent.com/103306835/184655902-20c4882b-4123-410b-aa1c-c48087420c74.png)

2.點選[Management]

![image](https://user-images.githubusercontent.com/103306835/184656021-17eb7a3c-45fe-41ea-8daf-07cb8893fa71.png)

3.點選[Create lifecycle rule]

![image](https://user-images.githubusercontent.com/103306835/184656070-3cce7fa7-0c04-49fa-9cc1-0eee038e751d.png)

4.輸入Lifecyle rule name(自訂)

![image](https://user-images.githubusercontent.com/103306835/184656091-643249d8-c67f-4a49-83e7-4dd57edb5427.png)

5.點選This rule….

![image](https://user-images.githubusercontent.com/103306835/184656144-ea9532ce-80d1-4829-9304-9e1061b4ddc4.png)


6.勾選[I acknowledge…]

![image](https://user-images.githubusercontent.com/103306835/184656180-88719721-9c70-4ede-8576-f54f3754f56a.png)

7.點選[Transition current versions of objects between storage class]

![image](https://user-images.githubusercontent.com/103306835/184656236-47a39459-e34d-495c-afaf-c67b8325f435.png)

8.設定Storage class

![image](https://user-images.githubusercontent.com/103306835/184656282-c8002d66-70e1-4ebf-b203-ccd3f4d4a56c.png)

9.勾選[I acknowledge….]

![image](https://user-images.githubusercontent.com/103306835/184656336-1c7dd542-f902-4c54-9ad3-4fe065b29f50.png)

10.點選[Create rule]

![image](https://user-images.githubusercontent.com/103306835/184656429-7ee0eeaa-b5f9-4240-9904-7634d96b23e1.png)


11.結果(S3 Standard->IA->Glacier)
![image](https://user-images.githubusercontent.com/103306835/184656397-5b28e7b9-49a8-4df6-a1b6-8cb0693fe723.png)
