---
layout: post
title: API_XML_SEOUL(2022_10_18)
date: 2022-10-18
tags: [TIL, XML, API]
toc:  true
---
서울시 데이터 중 실시간 도시정보 api를 받아와 xml 크롤링<br/>
{: .message }

## xml을 파싱하는것
### json xml 파싱은 데이터 줍기의 기본!

```
import pandas as pd
import requests
from bs4 import BeautifulSoup
import xmltodict
from __myapi__ import *

location_list=['강남 MICE 관광특구', '동대문 관광특구', '명동 관광특구', '이태원 관광특구']
               '잠실 관광특구', '종로·청계 관광특구', '홍대 관광특구', '경복궁·서촌마을', '광화문·덕수궁', '창덕궁·종묘',
               '가산디지털단지역', '강남역', '건대입구역', '고속터미널역', '교대역', '구로디지털단지역', '서울역', '선릉역',
               '신도림역', '신림역', '신촌·이대역', '역삼역', '연신내역', '용산역', '왕십리역',
               'DMC(디지털미디어시티)', '창동 신경제 중심지', '노량진', '낙산공원·이화마을', '북촌한옥마을', '가로수길',
               '성수카페거리', '수유리 먹자골목', '쌍문동 맛집거리', '압구정로데오거리', '여의도', '영등포 타임스퀘어', '인사동·익선동',
               '국립중앙박물관·용산가족공원', '남산공원', '뚝섬한강공원', '망원한강공원', '반포한강공원', '북서울꿈의숲', '서울대공원',
               '서울숲공원', '월드컵공원', '이촌한강공원', '잠실종합운동장', '잠실한강공원']

attrs = ['live_ppltn_stts','road_traffic_stts', 'prk_stts', 'sub_stts', 'bus_stn_stts', 'sbike_stts', 'weather_stts']

api_key = my_api()

response = requests.get(f"http://openapi.seoul.go.kr:8088/{api_key}/xml/citydata/1/5/{location}")


def response2df(_response,_attr):
    soup=BeautifulSoup(response.content,"lxml")
    attr_xml=soup.find('citydata').find(_attr)
    attr_dict = xmltodict.parse(str(attr_xml))[_attr]
    try:
        return pd.DataFrame(attr_dict[_attr])
    except:
        return pd.DataFrame(attr_dict)

info_by_loc= list()

for location in location_list:
    _list=list()
    response = requests.get(f"http://openapi.seoul.go.kr:8088/{api_key}/xml/citydata/1/5/{location}")
    for attr in attrs:
        _list.append(response2df(response,attr))
    info_by_loc.append(_list)

```
이렇게 하면 info_by_loc 에 location_list 순서로 실시간 데이터가 파싱되어서
attrs(실시간 인구 관련 이야기, 실시간 주위 교통정보, 실시간 주차장 정보 등등)이 적절하게 저장된다.
흠 근데 이런 데이터는 어떤식을 저장 하는것이 좋을까
어쩌면 파싱하지않고 xml자체를 저장하는것이 더 좋을 수도 있겠구먼
아 그러니까 예전 사람들이 xml을 저장매체로 사용한거 겠지.


