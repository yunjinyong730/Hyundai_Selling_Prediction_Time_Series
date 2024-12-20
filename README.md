# Hyundai_Selling_Prediction_Time_Series
🛻 ARIMA와 LSTM을 활용한 2030년 현대차 판매량 예측

## ARIMA 예측

### 예측 방법

ARIMA 모델을 사용해 자동차 모델(Avante, Sonata, G80)의 월별 판매량을 예측함

1. **데이터 준비**: 데이터를 불러와 'Date'를 인덱스로 설정하고 각 모델의 판매량을 분석 대상으로 사용함
2. **모델 하이퍼파라미터 튜닝**:
    - ARIMA 모델의 `(p, d, q)` 파라미터 값을 자동으로 튜닝하기 위해 여러 조합을 시도함
    - `itertools.product`를 이용해 다양한 `(p, d, q)` 조합을 테스트하고, AIC(Akaike Information Criterion) 값이 가장 낮은 조합을 최적의 파라미터로 선택함
3. **모델 학습 및 예측**:
    - 데이터의 80%를 학습 데이터로, 나머지 20%를 테스트 데이터로 사용해 학습함
    - 학습 데이터에 대해 ARIMA 모델을 학습시키고, 학습된 모델로 테스트 데이터의 판매량을 예측함
    - 예측 값과 실제 값의 RMSE(Root Mean Squared Error)를 계산하여 모델의 정확도를 평가함
4. **미래 예측**:
    - 최적의 `(p, d, q)` 파라미터로 전체 데이터에 대해 다시 학습하고, 2024년 10월부터 2030년까지의 월별 판매량을 예측함

[Avante_Sonata_G80_monthly_yearly_국내_판매량_ARIMA_예측.xlsx](https://prod-files-secure.s3.us-west-2.amazonaws.com/3de099d9-93f3-4629-a573-b788330d4c5a/bf1ac049-860e-4070-bbee-71b29d8e9d45/Avante_Sonata_G80_monthly_yearly_%E1%84%80%E1%85%AE%E1%86%A8%E1%84%82%E1%85%A2_%E1%84%91%E1%85%A1%E1%86%AB%E1%84%86%E1%85%A2%E1%84%85%E1%85%A3%E1%86%BC_ARIMA_%E1%84%8B%E1%85%A8%E1%84%8E%E1%85%B3%E1%86%A8.xlsx)

- 연도별 판매량

[DATA_ALL_ARIMA.xlsx](https://prod-files-secure.s3.us-west-2.amazonaws.com/3de099d9-93f3-4629-a573-b788330d4c5a/d5525feb-ea2c-425b-885f-af26f1e56f60/DATA_ALL_ARIMA.xlsx)

- 모델에서 직접 뽑은 예측량 기록

[sales_forecast.txt](https://prod-files-secure.s3.us-west-2.amazonaws.com/3de099d9-93f3-4629-a573-b788330d4c5a/0dd5e5ed-cc17-4db0-9bf6-3c9482b6b875/sales_forecast.txt)

- 추세선
![G80_arima_dynamic](https://github.com/user-attachments/assets/14f2478e-b3c8-4775-85d9-bab2f3f14e74)



- 원가절감 (원)

| Year | Avante | Sonata | G80 |
| --- | --- | --- | --- |
| 2024 | 740,834,137 | 141,269,793 | 692,692,759 |
| 2025 | 993,638,790 | 220,469,733 | 703,523,825 |
| 2026 | 1,155,495,408 | 296,049,790 | 725,496,285 |
| 2027 | 1,319,092,625 | 371,629,847 | 747,468,744 |
| 2028 | 1,482,694,662 | 447,209,904 | 769,441,204 |
| 2029 | 1,646,296,713 | 522,789,961 | 791,413,663 |

## LSTM 예측

### 예측 방법

데이터 불러오기 및 전처리:

- 엑셀 파일에서 판매 데이터 불러와서 날짜를 인덱스로 설정함.
- LSTM 모델에 맞게 MinMaxScaler를 사용해 0과 1 사이로 정규화함.

데이터셋 준비:

- look_back 기간을 12개월로 설정해서, 주어진 기간 동안의 데이터를 기반으로 다음 달 데이터를 예측하는 형태로 학습 데이터를 준비함.
- 80%는 훈련 데이터, 20%는 테스트 데이터로 나누고, LSTM 모델에 입력할 수 있게 3차원 형태로 재구성함.

LSTM 모델 구축 및 학습:

- LSTM 모델을 사용해 각 자동차 모델의 판매량 예측함.
- Sequential 모델로 LSTM 레이어와 Dense 레이어 추가하여 구성.
- epochs=50, batch_size=16으로 총 50번 학습 진행.

미래 판매량 예측:

- 학습된 모델로 2024년 11월부터 2030년 12월까지의 월별 판매량 예측함.
- 마지막 데이터를 이용해 다음 달 예측하고, 이를 다시 입력으로 사용해 반복적으로 미래 데이터 생성하는 방식.

예측값 복원 및 시각화:

- 예측 결과를 MinMaxScaler로 원래 값으로 복원함.
- 예측한 판매량을 DataFrame에 저장하고, 실제 판매량과 함께 그래프로 시각화함.

- LSTM 예측량 포함 데이터

[month_updated_transposed_cleaned.xlsx](https://prod-files-secure.s3.us-west-2.amazonaws.com/3de099d9-93f3-4629-a573-b788330d4c5a/beb1de20-fb56-4996-8535-4c5f364878aa/month_updated_transposed_cleaned.xlsx)

- 현대 화성 갈끄니까

![LSTM_2030_prediction](https://github.com/user-attachments/assets/32209217-c4ea-49cc-a29d-c537397d0077)


- 2025 상세 예측
    
    
    | 날짜 | Avante | Sonata | G80 |
    | --- | --- | --- | --- |
    | 2024-11-30 | 4,684 | 804 | 4,222 |
    | 2024-12-31 | 4,719 | 801 | 4,312 |
    | 2025-01-31 | 4,760 | 799 | 4,348 |
    | 2025-02-28 | 4,945 | 843 | 4,538 |
    | 2025-03-31 | 5,131 | 890 | 4,698 |
    | 2025-04-30 | 5,200 | 894 | 4,725 |
    | 2025-05-31 | 5,271 | 902 | 4,764 |
    | 2025-06-30 | 5,369 | 923 | 4,825 |
    | 2025-07-31 | 5,454 | 944 | 4,869 |
    | 2025-08-31 | 5,565 | 977 | 4,944 |
    | 2025-09-30 | 5,613 | 992 | 4,952 |
    | 2025-10-31 | 5,622 | 996 | 4,931 |
    | 2025-11-30 | 5,619 | 995 | 4,907 |
    | 2025-12-31 | 5,676 | 1,015 | 4,963 |
    ![Monthly Sales Data](https://github.com/user-attachments/assets/c273ab4e-5806-4892-8797-54cae4b2889d)

    

- 2031년까지 상세 예측 데이터

[predictions_2024_2025_log (1).txt](https://prod-files-secure.s3.us-west-2.amazonaws.com/3de099d9-93f3-4629-a573-b788330d4c5a/c6796ea3-353f-4841-a869-82cfd3631e87/predictions_2024_2025_log_(1).txt)

- 연도별 총합 데이터
    
    [DATA_ALL.xlsx](https://prod-files-secure.s3.us-west-2.amazonaws.com/3de099d9-93f3-4629-a573-b788330d4c5a/1543a983-fb60-40e2-b997-ca056e50fa59/DATA_ALL.xlsx)
    

![output (8).png](https://prod-files-secure.s3.us-west-2.amazonaws.com/3de099d9-93f3-4629-a573-b788330d4c5a/a942727f-315e-496b-9de9-74b17a686f84/output_(8).png)

| Year | Avante | Sonata | G80 |
| --- | --- | --- | --- |
| 2022 | 48791 | 4423 | 44479 |
| 2023 | 55675 | 7611 | 42199 |
| 2024 | 49037.1605 | 8974.04121 | 47039.1846 |
| 2025 | 64225.735 | 11168.9419 | 57464.8342 |
| 2026 | 67356.9927 | 13227.2156 | 53647.9407 |
| 2027 | 75565.9772 | 15961.066 | 58908.0059 |
| 2028 | 89495.8123 | 20819.1081 | 67520.1447 |
| 2029 | 125920.311 | 34395.8028 | 88406.8069 |

- ***연도별 원가절감 예측 표 (원)***

| Year | Avante | Sonata | G80 |
| --- | --- | --- | --- |
| 2025 | 919,907,949 | 167,199,098 | 747,706,186 |
| 2026 | 1,010,354,891 | 198,408,234 | 804,719,111 |
| 2027 | 1,133,489,659 | 239,415,989 | 883,620,089 |
| 2028 | 1,342,437,185 | 312,286,621 | 1,012,802,170 |
| 2029 | 1,888,804,666 | 515,937,042 | 1,326,102,104 |
| 2030 | 9,070,123,001 | 2,803,979,594 | 3,768,517,223 |

[연도별_원가절감_비용_표.csv](https://prod-files-secure.s3.us-west-2.amazonaws.com/3de099d9-93f3-4629-a573-b788330d4c5a/f1eda2e0-b530-4db7-a7e1-24851824d756/%E1%84%8B%E1%85%A7%E1%86%AB%E1%84%83%E1%85%A9%E1%84%87%E1%85%A7%E1%86%AF_%E1%84%8B%E1%85%AF%E1%86%AB%E1%84%80%E1%85%A1%E1%84%8C%E1%85%A5%E1%86%AF%E1%84%80%E1%85%A1%E1%86%B7_%E1%84%87%E1%85%B5%E1%84%8B%E1%85%AD%E1%86%BC_%E1%84%91%E1%85%AD.csv)

1.**원가절감 추정액** **(2025년 현대차 자동차 판매대수 예측해서 원가 절감이 어느정도 가능한지?)**

- **2022, 2023, 2024(지금까지 데이터 + 예측), 25 ~ 30 (차트)**
