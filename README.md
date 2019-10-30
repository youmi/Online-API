# Online API

\*The API is only accessible by HTTP GET and returns data in JSON format.

## API Request

### API Request Description

    URL: https://dsp-sg.miadx.net/v1/ol/ads
    Method: POST
    Headers: 'content-type: application/json'

### Request (JSON)

#### Request

| Parameter        | Type                                 | Mandatory | Example | Description                                     |
| ----------- | ------------------------------------ | ---- | ---- | ---------------------------------------- |
| client_info | <a href="#ClientInfo">ClientInfo</a> | Y    |      | Client basic information                           |
| slots       | []<a href="#Slot">Slot</a>           | Y    |      | Basic placement information, requesting up to 10 ad placements at a time |

#### <a name="ClientInfo">ClientInfo</a>

| Parameter     | Type   | Mandatory | Example                                                   | Description                                                              |
| -------- | ------ | ---- | ------------------------------------------------------ | ----------------------------------------------------------------- |
| ip       | String | Y    | 184.34.21.5                                            | Client IP                                                         |
| ua       | String | Y    | Mozilla/5.0 (Linux;) (KHTML) Chrome/69 Mobile Safari/5 | Client user agent string                                          |
| net_type | String | N    | NET_TYPE_WIFI                                          | Network Type， [NET_TYPE_WIFI, NET_TYPE_2G, NET_TYPE_3G, NET_TYPE_4G] |
| gaid | String | N | 975d6d12-07ec-4215-aece-b804debb4c75 | Google Advertising ID |
| idfa | String | N | 975d6d12-07ec-4215-aece-b804debb4c75 | Apple Advertising ID |

#### <a name="Slot">Slot</a>

| Parameter        | Type    | Mandatory | Example   | Description            |
| ----------- | ------- | ---- | ------ | --------------- |
| uid         | Integer | Y    | 1257   | Account unique id     |
| aid         | Integer | Y    | 21541  | Media unique id       |
| slot_id     | Integer | Y    | 356214 | Placement unique id   |
| template_id | Integer | Y    | 15262  | Ad template unique id |

## Response (JSON)

#### Response

| Parameter     | Type                           | Description     |
| -------- | ------------------------------ | ---------------|
| code     | Integer                        | Error code     |
| msg      | String                         | Error message  |
| slot_ads | []<a href="#SlotAd">SlotAd</a> | Ad information |

#### <a name="SlotAd">SlotAd</a>

| Parameter    | Type                   | Mandatory | Example     | Description          |
| ------- | ---------------------- | ---- | -------- | ------------------- |
| slot_id | Integer                | Y    | 84610400 | Placement unique id |
| ads     | []<a href="#Ad">Ad</a> | Y    |          | Ad information      |

#### <a name="Ad">Ad</a>

| Parameter     | Type                               | Mandatory | Example   | Description        |
| ------------- | ---------------------------------------- | ---- | ------ | ------------------ |
| oid           | Integer                                  | Y    | 158462 | Ad unique id         |
| creatives     | <a href="#Creatives">Creatives</a>       | Y    |        | Creative information |
| tracking_urls | <a href="#TrackingUrls">TrackingUrls</a> | Y    |        | Includes impression and click links   |

#### <a name="Creatives">Creatives</a>

| Parameter        | Type   | Mandatory | Example                       | Description           |
| ---------------- | ------ | --------- | ----------------------------- | --------------------- |
| img_url          | String | Y    | https://dsp-sg.miadx.net/v1/ol/ads/a.png | Image link       |
| landing_page_url | String | Y    | https://dsp-sg.miadx.net/v1/ol/ads/go    | Click to jump to the landing page |

#### <a name="TrackingUrls">TrackingUrls</a>

| Parameter      | Type     | Mandatory | Example                                         | Description                                         |
| -------------- | -------- | ----------| ----------------------------------------------- | ------------------------------------------ |
| impression_url | []String | Y    | [https://dsp-sg.miadx.net/v1/ol/ads/log?f=ol&r=ChxDT] | Impression callback address, and callback must be made after the ad is displayed. |
| click_url      | []String | Y    | [https://dsp-sg.miadx.net/v1/ol/ads/log?f=ol&r=mc291] | Click callback address , and callback must be made after the ad is clicked.  |

### Example:

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
