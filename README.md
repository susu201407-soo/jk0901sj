import requests, time
import pandas as pd

url = 'http://apis.data.go.kr/1613000/ApHusEnergyUseInfoOfferServiceV2/getWntyAvrgEnergyUseAmountInfoSearchV2'
serviceKey = f16aeef69f1eec8df8ca3c1bd1283735da6fcb9d98ab0d3a120e4ff2b4687fe7

items = []  # 연월별 전국 평균 에너지 사용금액 데이터
for year in range(2015, 2025):  # 2015년부터 2024년까지
    for month in range(1, 13):
        searchDate = f'{year}{month:02d}'  # 날짜형식: 201501
        params ={'serviceKey' : serviceKey, 'searchDate': searchDate}
        response = requests.get(url, params=params)
        if response.status_code == 200:
            # '\r': 현재 라인의 맨앞으로 이동
            # end='': 출력시 마지막 문자열은 빈문자열로 하여 줄바꿈 방지
            print("API 호출 성공", '\r', end='')
            item = response.json()['response']['body']['item']
            item['날짜'] = searchDate
            items.append(item)
            time.sleep(0.05)
        else: 
            print("API 호출 실패", '\r', end='')
