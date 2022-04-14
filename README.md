��ӭʹ��[����**PUSH** SDK For .NetCore](https://docs.getui.com/getui/server/rest_v2/introduction/)��

`����PUSH SDK For .NetCore`����ҪĿ����������������**�����**���ɸ������ͷ���Ŀ���Ч�ʡ�
�����߲���Ҫ���и��ӱ�̼���ʹ�ø������ͷ���ĸ���ù��ܣ�SDK�����Զ�����������ù���������ļ�Ȩ����װ����������HTTP����ȷǹ�����Ҫ��
ĿǰSDK��ʵ���˵��ƣ������ƣ�Ⱥ�����ַ�ʽ�����ͷ���


## ����Ҫ��
1. ֧��.NET CORE 3.X��5.0��6.0��7.0��

2. ʹ��`����PUSH SDK`ǰ������Ҫ��ǰ��[���ƿ���������](https://dev.getui.com) ��ɿ����߽����һЩ׼������������Ӧ�á���ϸ��[��������](https://docs.getui.com/getui/start/devcenter/#1)

3. ׼��������ɺ�ǰ��[���ƿ���������](https://dev.getui.com)��ȡӦ�����ã���������Ϊʹ��SDK�����롣��ϸ��[��������](https://docs.getui.com/getui/start/devcenter/#11)


## ��װ����
```
    Install-Package GeTuiPushApiV2.ServerSDK.Core
```

## ���ٿ�ʼ
��SDK֧��3�ֵ��÷�ʽ��������Լ���ҵ���������ѡ��

### 1.ֱ�ӵ��ø���RestAPI V2�Ľӿڷ���
���ַ�ʽ��Ҫ�Լ��Խӿڵ������������Ӧ���д����Լ�Ȩtoken��CID���л���ȡ�
##### ʹ��ʾ��
###### 1.**��ȡ��Ȩtoken**
```C#
            string AppID = "Ny3b4Umv7882X0UheVwCU4";//Ӧ��ID
            string AppKey = "dY1BXGSHys8TPKeCqU3ilA"; //Ӧ��key
            string MasterSecret = "GAZTCU0hyO69XjC9u5pSb2"; //����Կ

            long _timestamp = (DateTime.Now.ToUniversalTime().Ticks - 621355968000000000) / 10000;
            var indto = new ApiAuthInDto()
            {
                appkey = AppKey,
                timestamp = _timestamp,
                sign = SHA256Helper.SHA256Encrypt(AppKey + _timestamp + MasterSecret),
                appId = AppID
            };

            var result = await GeTuiPushV2Api.HttpPostGeTuiApiAsync<ApiAuthInDto, ApiAuthOutDto>($"https://restapi.getui.com/v2/{AppID}/auth", indto);
            string token=result.data.token;
```
###### 2.**����cid���е���**
```C#
               var apiInDto = new ApiPushToSingleInDto()
                {
                    request_id = Guid.NewGuid().ToString(),
                    audience = new audience_cidDto()
                    {
                        cid = new string[] { "123456789" }
                    },
                    push_message = new push_messageDto()
                };
                //֪ͨ��Ϣ
                apiInDto.push_message.notification = new notificationDto()
                {
                    title = "ͣ������",
                    body = "��ͣ�����뼰ʱ����",
                    click_type = "payload",
                    payload = JsonConvert.SerializeObject(new
                    {
                        msgId = new string[] { Guid.NewGuid().ToStr() },
                        text = $"ͣ��ʱ�䣺{DateTime.Now}"
                    }),
                    badge_add_num = 1,
                    channel_id = "Push",
                    channel_name = "Push",
                    channel_level = 4
                };
                apiInDto.token = token;
                apiInDto.appId = AppID;
                await HttpPostGeTuiApiAsync<ApiPushToSingleInDto, ApiPushToSingleOutDto>($"https://restapi.getui.com/v2/{AppID}/push/single/cid", apiInDto);
```


### 2.ʹ���ѷ�װ�õ�ͳһ������Ӧ������
���ַ�ʽ���ڵ�1�ַ�ʽ�Ͻ�����һ���װ��ͳһ�Խӿ�������Ӧ�����˴�������Ȼ��Ҫ�Լ�Ȩtoken��CID���л���ȡ�

### 3.ʹ���ѷ�װ�õĸ������ͷ����Ƽ���
���ַ�ʽ�£�����Ҫ׼�����ͷ���������������ɽ������͡������ֶ�ѡ�����ͽӿ�������ʹ�õ��ƣ�Ⱥ�ƣ������ƣ��������ݲ����Զ�ѡ�����ͷ�ʽ��

### ��ͨ����





##### ʹ��ʾ����**����API**

```C#

```

##### ʹ��ʾ����**����API**_����cid���е���

```C#

```



## ��֧�ֵ�API�б�
��������Ϣ���͹���������API�Ķ�Ӧ��ϵ

| API���      |      ����       | ���õ�API����                                              |
|-----------|-----------------|-----------------------------------------------------------|
| ��ȨAPI | [��Ȩ](https://docs.getui.com/getui/server/rest_v2/token/#0)              | com.getui.push.v2.sdk.api.AuthApi.auth                                  |
| ����API | [cid����](https://docs.getui.com/getui/server/rest_v2/push/#1)            | com.getui.push.v2.sdk.api.PushApi.pushToSingleByCid                     |
| ����API | [tolist������Ϣ](https://docs.getui.com/getui/server/rest_v2/push/#5)      | com.getui.push.v2.sdk.api.PushApi.createMsg                             |
| ����API | [cid������](https://docs.getui.com/getui/server/rest_v2/push/#6)           | com.getui.push.v2.sdk.api.PushApi.pushListByCid                         |                  
| ����API | [Ⱥ��](https://docs.getui.com/getui/server/rest_v2/push/#8)                | com.getui.push.v2.sdk.api.PushApi.pushAll                               |                                

> ע������API���������У������ڴ���


## ��API�ӿڿ���ָ��
1. �½�api�ӿ��࣬ʹ����ע��`com.getui.push.v2.sdk.anno.GtApi`���Ϊ���ƽӿ���

2. �ӿڣ�ʹ��`com.getui.push.v2.sdk.anno.method`���µķ���ע��`GtGet`/`GtPost`/`GtPut`/`GtDelete`�������ʽ���ֱ����`GET`��`POST`��`PUT`��`DELETE`����HTTP����ʽ

3. ������ʹ��`com.getui.push.v2.sdk.anno.param`���µĲ���ע��`GtPathParam`/`GtHeaderParam`/`GtQueryParam`/`GtBodyParam`��ǲ������ͣ��ֱ��ʾHTTP�����е����ֲ����� ·������/header����/query����/body����

## ��������
[���Ʒ����SDK RestAPI V2�ĵ�����](https://docs.getui.com/getui/server/rest_v2/service_sdk/)