# Online API

\*API 只能通过 HTTP 访问，并以 JSON 格式返回数据。

## API Request

### API 请求说明

    URL: https://dsp-sg.miadx.net/v1/ol/ads
    Method: POST
    Headers: 'content-type: application/json'

### Request (JSON)

#### Request

| 参数        | 类型                                 | 必须 | 样例 | 描述                                     |
| ----------- | ------------------------------------ | ---- | ---- | ---------------------------------------- |
| client_info | <a href="#ClientInfo">ClientInfo</a> | Y    |      | 客户端基本信息                           |
| slots       | []<a href="#Slot">Slot</a>           | Y    |      | 广告位基本信息，一次请求最多 10 个广告位 |

#### <a name="ClientInfo">ClientInfo</a>

| 参数     | 类型   | 必须 | 样例                                                   | 描述                                                              |
| -------- | ------ | ---- | ------------------------------------------------------ | ----------------------------------------------------------------- |
| ip       | String | Y    | 184.34.21.5                                            | 客户端 IP                                                         |
| ua       | String | Y    | Mozilla/5.0 (Linux;) (KHTML) Chrome/69 Mobile Safari/5 | 客户端 user agent 字符串                                          |
| net_type | String | N    | NET_TYPE_WIFI                                          | 连接类型， [NET_TYPE_WIFI, NET_TYPE_2G, NET_TYPE_3G, NET_TYPE_4G] |

#### <a name="Slot">Slot</a>

| 参数        | 类型    | 必须 | 样例   | 描述            |
| ----------- | ------- | ---- | ------ | --------------- |
| uid         | Integer | Y    | 1257   | 媒体帐号唯一 id |
| aid         | Integer | Y    | 21541  | 媒体唯一 id     |
| slot_id     | Integer | Y    | 356214 | 广告位唯一 id   |
| template_id | Integer | Y    | 15262  | 广告模板唯一 id |

## Response (JSON)

#### Response

| 参数     | 类型                           | 描述     |
| -------- | ------------------------------ | -------- |
| code     | Integer                        | 错误码   |
| msg      | String                         | 错误信息 |
| slot_ads | []<a href="#SlotAd">SlotAd</a> | 广告信息 |

#### <a name="SlotAd">SlotAd</a>

| 参数    | 类型                   | 必须 | 样例     | 描述          |
| ------- | ---------------------- | ---- | -------- | ------------- |
| slot_id | Integer                | Y    | 84610400 | 广告位唯一 id |
| ads     | []<a href="#Ad">Ad</a> | Y    |          | 广告信息      |

#### <a name="Ad">Ad</a>

| 参数          | 类型                                     | 必须 | 样例   | 描述               |
| ------------- | ---------------------------------------- | ---- | ------ | ------------------ |
| oid           | Integer                                  | Y    | 158462 | 广告唯一 id        |
| creatives     | <a href="#Creatives">Creatives</a>       | Y    |        | 素材信息           |
| tracking_urls | <a href="#TrackingUrls">TrackingUrls</a> | Y    |        | 包含展示及跳转链接 |

#### <a name="Creatives">Creatives</a>

| 参数             | 类型   | 必须 | 样例                               | 描述           |
| ---------------- | ------ | ---- | ---------------------------------- | -------------- |
| img_url          | String | Y    | https://dsp-sg.miadx.net/v1/ol/ads/a.png | 图片链接       |
| landing_page_url | String | Y    | https://dsp-sg.miadx.net/v1/ol/ads/go    | 点击跳转落地页 |

#### <a name="TrackingUrls">TrackingUrls</a>

| 参数           | 类型     | 必须 | 样例                                            | 描述                                       |
| -------------- | -------- | ---- | ----------------------------------------------- | ------------------------------------------ |
| impression_url | []String | Y    | [https://dsp-sg.miadx.net/v1/ol/ads/log?f=ol&r=ChxDT] | 展示回调地址，在广告展示完后必须进行回调。 |
| click_url      | []String | Y    | [https://dsp-sg.miadx.net/v1/ol/ads/log?f=ol&r=mc291] | 点击回调地址，在点击广告后必须进行回调。   |

### 样例:

#### Request

```
curl -X POST \
  https://dsp-sg.miadx.net/v1/ol/ads \
  -H 'cache-control: no-cache' \
  -H 'content-type: application/json' \
  -d \
'{
    "client_info":{
            "ip":"192.168.1.3",
            "ua":"Mozilla/5.0 (Linux; Android 7.0.0;) AppleWebKit/527 (KHTML, like Gecko) Chrome/69.0.0.0 Mobile Safari/5"
    },
    "slots": [
        {
            "uid": 140488,
            "aid": 1223867,
            "slot_id": 10616,
            "template_id": 11001
        }
    ]
}'
```

#### Response

```
-H 'content-type: application/json'
{
    "slot_ads": [
        {
            "slot_id": 100000400,
            "ads": [
                {
                    "oid": 100,
                    "creatives": {
                        "img_url": "http://x.cdn.net/public/ad/creative/298825232acf53dfb3666b8963f011860c4-.jpg",
                        "landing_page_url": "https://dsp-sg.miadx.net/v1/ol/ads/go"
                    },
                    "tracking_urls": {
                        "impression_url": "https://dsp-sg.miadx.net/v1/ol/ads/log?f=ol&r=ChxDT2RMRUwtY094aUFfZUdTSkNDc0FpajZBV2dEGIHYFiAC",
                        "click_url": "https://dsp-sg.miadx.net/v1/ol/ads/log?f=ol&r=ChxDT2RMRUwt91cmNlPTk3MDMwMzA0MDAYgdgWIAM"
                    }
                }
            ]
        }
    ]
}
```
