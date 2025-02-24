## 프로젝트명 : 데이터 분석 기반 2025 국내 프로 스포츠 직관 관중수 예측 및 스포츠 팬 맞춤 마케팅 전략 제안

---
## 1. 프로젝트 개요

### 1.1 프로젝트 배경
최근 **Z세대**를 중심으로 **스포츠 경기 직관**, 이른바 ‘직관코어’ 트렌드가 빠르게 확산되고 있습니다.<br>
스포츠 관람은 단순한 취미를 넘어 **개인의 라이프스타일과 정체성을 표현**하는 중요한 요소로 자리 잡고 있습니다.<br>
그리고 이는 스포츠 산업에 **새로운 기회**를 창출하고 있습니다.<br>
이에 국내 프로스포츠 4대 종목[배구, 야구, 농구, 축구]의 지난 3년간 데이터를 수집하여 **2025년의 관중수와 스포츠 선호 트렌드를 예측**하고자 합니다.<br>
분석에 사용된 데이터는 다음과 같습니다.
- **각 지역별 경기장 관중수 데이터**
- **성별.연령대별 스포츠 종목 선호도 조사 데이터**
- **일별 네이버 검색량 데이터**

### 1.2 프로젝트 목적
이 프로젝트는 **연령대, 성별, 지역 특성에 따른 스포츠 소비 특성**을 파악하고 특히 Z세대를 포함하여 **각 연령층에 맞춤형 마케팅 전략**을 수립하는 것을 목표로 합니다.<br>
이를 통해 **프로스포츠 산업의 팬 유치 및 지속적 성장 동력 확보**에 기여하고자 합니다.

### 1.3 가설
- 가설 1. Z세대 성별간 종목별 선호도 차이가 있다.
- 가설 2. 연령대, 성별에 따라 스포츠 선호도에 차이가 있다.
- 가설 3. **지역에 따른 관중수**와 **선호 종목**간에는 유의미한 상관관계가 존재한다.

---
## 2. 데이터 수집

### 2.1 오픈 데이터
- 연령대별 스포츠 관람 선호도 조사 (2021.11 - 2024.11)
  *출처: 문화 빅데이터 플랫폼*
  [바로가기](https://www.bigdata-culture.kr/bigdata/user/data_market/detail.do?id=7180de30-eb98-11ec-a6e8-cdf27550dc0d)

- 프로스포츠 관람객 성향 조사(2021.11 - 2024.11)
  *출처: 프로스포츠 정보광장*
  [바로가기](http://data.prosports.or.kr/page/board/tendency)

- [야구/축구/농구/배구] 월간 검색어량 (2021 - 2024)
  *출처: 네이버 데이터랩*
  [바로가기](https://datalab.naver.com/keyword/trendSearch.naver)

### 2.2 웹크롤링을 통한 데이터 수집
- 각 종목별 경기장 주소
  [바로가기](http://data.prosports.or.kr/page/statistic/stadium)

- 각 종목별 관중수
  [바로가기](http://data.prosports.or.kr/page/schedule/schedule)

---
## 3. 데이터 전처리

### 3.1 종목별 선호도 조사 데이터

#### 3.1.1. 데이터 병합
- **2021년 1월 - 2024년 11월까지** 월별로 분산된 **총 37개의 선호도 조사 데이터**를 수집하여 하나의 데이터셋으로 병합했습니다.
#### 3.1.2 데이터 추가 생성 및 삭제
- **연도와 월별 분석**을 위해 날짜 데이터를 연도와 월로 분리했습니다.
- **분석에 불필요한 열(*ex. 소득수준, 응답ID*)은 삭제하고, **'야구','축구','배구','농구'외의 값**은 '없음'으로 처리했습니다.
#### 3.1.3 결측치 처리
- 선호도 1,2,3 순위의 응답 전부 '없음'인 경우는 노이즈 데이터를 줄이기 위해 삭제했습니다.
#### 3.1.4 1차 정제 결과 보고
- **초기 데이터 행**: 84,840
- **최종 데이터 행**: 30,126
  - 응답하지 않은 데이터의 비율이 높아 **유의미한 데이터를 추출**하여 **분석 신뢰성을 확보**하였습니다.

### 3.2 종목별 경기장, 관중수 데이터

#### 3.2.1 데이터 병합
- **종목별 관중수 데이터**를 로드한 후 **여자농구 데이터를 제거**하였습니다.
  - 여자농구 관중수 정보가 **선호도 데이터에 포함되지 않고**, 누락값이 많아 분석에 미치는 영향이 적다고 판단했습니다.
- **경기장 데이터**를 로드하여 잘못 기재된 **경기장명, 주소를 정제**하고, 경기장명과 주소를 매핑하여 **데이터를 병합**하였습니다.
#### 3.2.2 결측치 처리
- **날씨 데이터가 누락된 행**은 **수작업**으로 보완했습니다.
- **관중수가 누락된 행**은 데이터 신뢰도를 위해 삭제하였습니다.
- **주소가 누락된 행**은 해당 경기장 주소에 맞게 **정확히 기입**하였습니다.
#### 3.2.3 2차 정제 결과 보고
- **관중수 누락값**: 삭제 완료
- **날씨 데이터 보완**: 누락된 데이터를 수작업으로 검색 및 입력
- **주소 데이터 보완**: 페퍼스타디움, 군산월명체육관 결측치 수정
- **결론**:
  - 여자 농구 데이터를 제외하여 **분석의 일관성을 확보**했습니다.
  - 누락값 보완 및 삭제를 통해 **데이터 신뢰성과 정확도**를 높였습니다.
  - 경기장별 관중수 데이터 완성도가 높아져 **지역별 패턴 분석이 용이**해졌습니다.

### 3.3 데이터 스케일링 및 정규화

#### 3.3.1 선호도 조사 데이터 가중치 부여 
- **선호도 조사 데이터**에 **순위별 가중치를 적용**하여 일관된 척도로 분석할 수 있도록 했습니다.
  - **1순위: 3점,  2순위: 2점, 3순위: 1점**으로 점수를 부여했습니다.
- **지역별 선호도 점수**가 동일할 경우 **정확도를 높이기 위해**가중치를 적용했습니다.
- **결론**:
  - 가중치 적용을 통해 **지역별 선호도 차이를 정량화**하여 분석 신뢰도를 높였습니다.

### 3.4 데이터 변환

#### 3.4.1 경기장 주소 데이터와 관중수 데이터 병합
-**경기장 주소 데이터**와 **관중수 데이터**를 결합하여 **지역 변수를 생성**하였습니다.
- **날짜, 시간, 결과, 단체/구단, 주소 등** 분석에 불필요한 열은 삭제했습니다.
- **요일 변수**를 추가하여 요일별 패턴 분석이 가능하도록 했습니다.
- **결론**:
  - **지역 변수 생성**으로 **지역별 관중수 및 선호도 분석**이 가능해졌습니다.
  - **요일 변수 추가**로 **요일별 관중수 변화**를 분석할 수 있는 인사이트가 확보되었습니다.

### 3.5 최종 정제 보고
1) 스포츠 직관 선호도 데이터
   - 최종 행: 5,413행
   - 최종 열: 9열
   - **가중치 적용 데이터**와 결합하여 **점수화된 데이터**로 활용했습니다.
2) 경기장별 관중수 데이터
   - 최종 행: 4,077행
   - 최종 열: 11열
   - 각 종목별로 **경기장별 관중수 데이터**를 직관적으로 확인할 수 있으며, 가독성이 향상되었습니다.

---
## 4. 탐색적 데이터 분석(EDA)
탐색적 데이터 분석(EDA)를 통해 **연령대별 스포츠 선호도**, **경기장별 관중수**, **검색량 추이** 등을 분석하였습니다.<br>
이를 바탕으로 **연령대 및 성별에 따른 스포츠 선호도 차이**와 **지역별 관중수 패턴**을 파악하고, **마케팅 전략 수립을 위한 인사이트**를 도출했습니다.

### 4.1 전연령대 스포츠 직관 선호도 분포
#### 목적:
- 연령대와 성별에 따른 스포츠 직관 선호도 분포를 파악하여, **종목별 타겟층을 식별**하고 마케팅 전략에 활용합니다.

#### 결과 및 해석:
- 종목별 선호도 합계 비율
  - 야구: 42.1%
  - 축구: 41.1%
  - 농구: 9.8%
  - 배구: 7.0%
<img width="272" alt="image" src="https://github.com/user-attachments/assets/71e9ff44-afb9-4b49-95f3-820223b34587" />

#### 인사이트:
  - **야구와 축구**의 선호도 비율이 비슷한 수치로 높게 나타났으며, 이는 두 종목의 **주요 타겟층이 겹칠 가능성**이 높음을 의미합니다.
  - **농구와 배구**는 상대적으로 낮은 선호도를 보였지만 **특정 팬층**을 공략하면 **충성도 높은 관중**을 확보할 수 있습니다.야구와 축구에 비해 현저히 떨어지는 수치를 확인할 수 있습니다.

#### 마케팅 전략:
- 야구, 축구:
  - 공통 타겟층을 대상으로 **크로스오버 마케팅** 추진
    (ex. 공동 이벤트, 패키지 티켓 활용)
- 농구, 배구:
  - 특정 팬층을 겨냥한 **차별화된 마케팅** 진행
    (ex. 소규모 팬미팅, SNS 커뮤니티 활성화 등)

### 4.2 2023 - 2024 종목별 월 평균 관중수
#### 목적: 
- **종목별 관중수 추이**를 분석하여 **2025년 관중수 예측 및 계절별/이벤트별 마케팅 전략** 수립에 활용합니다.

#### 결과 및 해석:
- 2023 --> 2024년 종목별 월 평균 관중수 및 증감률
<img width="418" alt="image" src="https://github.com/user-attachments/assets/9f2e5c52-299b-42e5-9b61-d712eabfbdf1" />

| 스포츠 | 2023년 | 2024년 | 증감률 |
|--------|--------|--------|--------|
| 야구   | 66.8만 | 89.9만 | +34.6% |
| 축구   | 25.1만 | 28.4만 | +12.9% |
| 농구   | 9.4만  | 11.6만  | +23.3%|
| 배구   | 8.0만  | 8.3만  | +4.6%  |

#### 인사이트:
- **야구**는 가장 높은 +34.6%의 성장률을 기록하며, **압도적인 인기**를 확인했습니다.
- **축구**는 +12.9%의 안정적인 증가세를 보이며 **고정 팬층**을 확보하고 있습니다.
- **농구와 배구**는 상대적으로 낮은 증가율을 보이나, 고정 팬층을 대상으로 한 **팬 커뮤니티 강화 전략**이 유효할 것이라 예상됩니다.

#### 마케팅 전략:
- 야구:
  - 성장률이 가장 높아 **대규모 이벤트 및 프로모션**이 효과적
    (ex. 정규 시즌 및 포스트 시즌 연계 마케팅 강화)
- 축구:
  - **고정 팬층**을 겨냥한 **팬 로열티 프로그램** 강화
    (ex. 시즌권 혜택 확대, 클럽 멤버십 강화)
- 농구&배구:
  - 특정 계층을 겨냥한 **니치 마케팅 전략**
    (ex. 대학생 및 직장인 대상 할인 이벤트, 사회인 리그 연계 마케팅)

### 4.3 종목별 관중수와 검색량 추이
#### 목적: 
- **종목별 관중수**와 **검색량**의 상관관계를 분석하여 검색량 데이터를 **관중수 예측 모델 및 디지털 마케팅 전략 수립**에 활용하고자 합니다.
<img width="533" alt="image" src="https://github.com/user-attachments/assets/633b2745-3185-4f5b-82d6-02df14829ebf" />

#### 결과 및 해석:
- **2023년 1월 - 2024년 11월** 동안의 **종목별 관중수와 검색량 상관계수**
  - 야구(0.975): 매우 강한 양의 상관관계
  - 축구(0.944): 매우 강한 양의 상관관계
  - 농구(0.841): 강한 양의 상관관계
  - 배구(0.641): 중간 정도의 양의 상관관계

#### 인사이트:
- **야구와 축구**는 **검색량**과 **관중수**간에 매우 강한 상관관계를 보였습니다.
  - **검색량 데이터** 활용을 통해 **관중수 예측 정확도**를 높일 수 있습니다.
- **농구와 배구**는 상대적으로 낮은 상관관계를 보였지만 팬층의 충성도가 높은 종목으로 **타겟 마케팅**에 유리합니다.

#### 마케팅 전략:
- **야구&축구**:
  - 검색량 급증 시기에 맞춘 **디지털 캠페인** 추진
    (ex. 경기 일정 공지 및 하이라이트 영상 광고 집행)
- **농구&배구**:
  - 타겟층에 집중한 SNS 마케팅 및 커뮤니티 활성화 전략
    (ex. SNS 챌린지, 팬 참여형 콘텐츠 제작)

### 4.4 Z세대 남성 / 여성 선호 스포츠
#### 목적: 
- **Z세대의 성별에 따른 스포츠 선호도 차이**를 파악하여 맞춤형 콘텐츠 마케팅 전략을 수립하고자 합니다.

#### 4.4.1 20대 남성 월별 스포츠 선호도
<img width="411" alt="image" src="https://github.com/user-attachments/assets/85bd0147-7195-4413-83be-80bb132a97c0" />

#### 4.4.2 20대 여성 월별 스포츠 선호도
<img width="410" alt="image" src="https://github.com/user-attachments/assets/a3123abd-a6dd-466c-9576-afef209d47a3" />

#### 결과 및 해석:
- 20대 남성 스포츠 선호도:
  - 축구 > 야구 > 농구 > 배구 순으로 나타납니다.
- 20대 여성 스포츠 선호도:
  - 야구 > 배구 > 축구 > 농구 순으로 나타납니다.

#### 인사이트: 
- **남성 Z세대**는 **축구와 야구**에 대한 충성도가 매우 높아 **디지털 콘텐츠 마케팅**에 유리합니다.
- **여성 Z세대**는 **야구와 배구**에 대한 선호도가 높아 **감성 마케팅**과 **SNS 기반 팬 커뮤니티 강화 전략**이 효과적입니다.

#### 마케팅 전략:
- 남성 Z세대:
  - 축구 및 야구 관련 디지털 콘텐츠 마케팅 강화
    (ex. 하이라이트 영상 클립, SNS 밈 콘텐츠)
- 여성 Z세대:
  - 야구와 배구에 감성 마케팅 및 커뮤니티 기반 마케팅 전개
    (ex. 인플루언서 콜라보, 팬 커뮤니티 활성화)

---
## 5. 가설 검증 & 분석 시각화
가설 검증을 통해 **연령대, 성별, 지역별 스포츠 선호도 차이**를 확인하고, 마케팅 전략 수립에 필요한 **데이터 기반 인사이트를 도출**했습니다.
**통계 검정**과 **시각화**를 통한 **결과 해석**을 통해 구체적인 전략을 제안합니다.

### 5.1 Z세대 성별간 종목별 선호도 차이가 있다.
#### 가설 설정 및 배경:
- 가설:
  - **Z세대 성별에 따라 농구와 배구의 선호도 차이가 있다.**
- 배경:
  - Z세대는 스포츠 콘텐츠 소비 방식과 선호 종목에서 **성별 간 차이가 클 것**으로 예상됩니다.
  - 특히 **게임, e스포츠와의 연관성이 높아** 성별 차별화 전략이 필요합니다.

#### 검증 방법:
- 정규성 검증:
  - **Shapiro-Wilk Test**를 사용하여 **정규분포 여부를 확인**하였습니다.
  - 결과: **p<0.05** --> 정규성 만족 x
- 비모수 통계 검정:
  - 정규성을 만족하지 않아 **Mann-Whitney U Test(두 집단 간 차이 비교)로** 선호도 차이를 검정했습니다.
  - 유의수준: 0.05
<img width="412" alt="image" src="https://github.com/user-attachments/assets/fb7a6e9f-6e2a-4ba2-9eb0-c077c7c7788b" />

#### 결과 및 해석:
- Mann-Whitney U Test:
  - 농구: p<0.05 --> 성별에 따른 선호도 차이가 유의미합니다.
  - 배구: p<0.05 --> 성별에 따른 선호도 차이가 유의미합니다.

#### 그래프 해석:
- 농구: 남성 Z세대의 선호도 > 여성 Z세대의 선호도
- 배구: 여성 Z세대의 선호도 > 남성 Z세대의 선호도
#### 인사이트:
- 남성 Z세대는 농구, 여성 Z세대는 배구를 선호하는 뚜렷한 성별 차이가 확인되었습니다.
- Z세대 타겟팅 마케팅 전략 수립 시 **성별에 따른 맞춤형 콘텐츠**가 필요합니다.

#### 마케팅 전략 제안:
- 남성 Z세대(농구):
  - 디지털 콘텐츠 마케팅 강화
    (ex. 농구 하이라이트 영상 클립 및 SNS 밈 콘텐츠 제작)
  - **e스포츠와 연계**한 크로스오버 마케팅
    (ex. NBA 게임 이벤트 및 e스포츠 팀 콜라보)
- 여성 Z세대(배구):
  - **감성 마케팅** 및 **커뮤니티 강화**
    (ex. 인플루언서 콜라보 및 팬 커뮤니티 활성화)
  - **현장 직관 이벤트** 강화
    (ex. 경기장 체험형 콘텐츠 및 팬 참여형 이벤트)

### 5.2 연령대별/성별 간 스포츠 선호도에 차이가 있다.
#### 가설 설정 및 배경:
- 가설:
  - **연령대 및 성별 간 스포츠 선호도에 유의미한 차이가 있다.**
- 배경:
  - 스포츠 선호도는 연령대와 성별에 따라 뚜렷한 차이를 보이며, 특히 **2030세대는 축구와 야구**, **4050세대는 야구** 선호도가 높을 것으로 예상됩니다.

#### 검증 방법:
- 정규성 검증:
  - **Shapiro-Wilk Test** 결과 **p<0.05** --> 정규성을 만족하지 않습니다.
- 비모수 통계 검정:
  - **Kruskal-Wallis H Test(세 집단 이상의 차이 비교)로** **연령대별/성별 선호도 차이**를 검정했습니다.
  - 유의수준: 0.05
<img width="409" alt="image" src="https://github.com/user-attachments/assets/b45b290e-54a3-4448-9373-0f0dafea0aeb" />

#### 결과 및 해석: 
- Kruskal-Wallis H Test 결과:
  - 연령대별: p<0.05 --> 연령대별 선호도 차이가 유의미합니다.
  - 성별: p<0.05 --> 성별 간 선호도 차이가 유의미합니다.
- 그래프 해석:
  - 2030 남성: 축구 선호도 압도적
  - 4050 남성: 야구 선호도 높음
  - 2030 여성: 배구 선호도 상대적으로 높음
  - 4050 여성: 야구 선호도 우세

#### 인사이트:
- **2030 남성은 축구**, **4050 남성은 야구 선호도**가 뚜렷하여 연령대별 차별화 전략이 필요합니다.
- 여성층은 **연령대별 차이가 크지 않으나** **배구와 야구에 대한 선호도**가 상대적으로 높았습니다.

#### 마케팅 전략 제안:
- **2030 남성(축구)**:
  - 디지털 기반 실시간 마케팅
    (ex. **라이브 스트리밍 광고** 및 **실시간 소통 콘텐츠** 강화)
  - 경기장 경험 강화
    (ex. **팬 체험형 이벤트** 및 **VR/AR** 콘텐츠)
- **4050 남성(야구)**:
  - **패밀리 마케팅** 및 **직관** 이벤트
    (ex. 가족 단위 경기 관람 패키지 및 기업 단체 관람 프로모션)
  - 고급화 전략:
    (ex. **VIP석 및 프리미엄 서비스** 강화
- **여성층(배구/야구)**:
  - 감성 마케팅 및 SNS 캠페인
    (ex. 인플루언서 마케팅 및 팬 커뮤니티 중심 콘텐츠)
  - **배구: 연예인 응원단 및 현장 직관 이벤트 강화**
  - **야구: 패밀리 이벤트 및 굿즈 마케팅 강화**

### 5.3 지역에 따른 관중수와 선호하는 종목 간에는 유의미한 상관관계가 존재한다.
#### 가설 설정 및 배경:
- 가설:
  - **지역별 관중수와 선호 종목 간에 유의미한 상관관계가 존재한다.**
- 배경:
  - **서울특별시, 경기도 등 수도권**에서는 **야구와 축구**, **부산과 대구 등 영남권**에서는 특히 **야구가 인기있을 것**으로 예상됩니다.

#### 검증 방법:
- 정규성 검증:
  - Shapiro-Wilk Test 결과 p<0.05 --> 정규성을 만족하지 않습니다.
- 비모수 통계 검정:
  - Spearman 상관계수(순위 기반 상관관계 분석)을 사용하여 지역별 관중수와 선호 종목 간의 상관관계를 분석했습니다.
  - 유의수준: 0.05
<img width="412" alt="image" src="https://github.com/user-attachments/assets/b6e0f45e-6279-4c6b-8cfa-5cd6de2511cd" />

#### 결과 및 해석:
- 야구와 축구는 **수도권에서 강한 양의 상관관계**를 보입니다.
- 야구는 **부산, 대구에서 가장 높은 상관관계**를 보입니다.
 
#### 그래프 분석:
- **그래프 1: 지역/종목별 관중수와 검색량 비교**
  - **야구**:
    - 서울과 경기도에서 검색량과 관중수가 매우 유사한 패턴을 보입니다.
    - **디지털 검색량**이 경기 **직관율에 큰 영향**을 미침을 시사합니다.
  - **축구**:
    - 서울과 경기도에서 **검색량 급등 시기에 관중수도 동반 상승**하는 추세를 보였습니다.
  - **농구&배구**:
    - 서울과 경기도를 제외한 지방권역에서는 관중수와 검색량 간 상관관계가 낮습니다.
    - 연고지 팬덤보다는 디지털 마케팅 영향력이 더 크다는 것을 시사합니다.
- **그래프 2: 지역별 선호도와 관중수 비교**
  - **야구**:
    - 부산과 대구에서 선호도 및 관중수가 일치하며, **연고지 팬덤의 영향**을 강하게 받는 것을 확인할 수 있습니다.
  - **축구**:
    - 수도권 중심의 높은 선호도와 관중수가 일치하며, **인프라 접근성**이 주요 요인임을 시사합니다.
  - **농구&배구**:
    - **특정 지역**에서만 **관중수 상승**이 나타나며, **이벤트 및 특정 스타 선수**의 영향일 가능성이 높습니다.

#### 인사이트:
- **야구**는 전국적인 인기를 보이지만 특히 **영남권**에서 연고지 팬덤이 강하게 작용합니다.
  - **부산, 대구**에서 **야구 이벤트 및 팬미팅** 등의 **오프라인 마케팅**이 효과적입니다.
- **축구**는 **수도권 중심**으로 인기가 높으며, 디지털 검색량이 관중수에 큰 영향을 미침을 확인했습니다.
  - 서울특별시와 경기도에서 디지털 마케팅 캠페인을 **집중 전개**해야합니다.
- **농구**와 **배구**는 특정 지역의 **이벤트성 인기**에 의존하고 있으며, 연고지 팬덤보다는 **SNS 및 디지털 콘텐츠의 영향**이 큽니다.
  - SNS 중심의 바이럴 마케팅이 효과적이며, **스타 선수 활용 및 이벤트성 콘텐츠**가 필요합니다.

#### 마케팅 전략 제안:
- **야구: 지역 연고 팬덤 강화 및 오프라인 이벤트**
  - 부산, 대구: **연고지 팬덤** 강화
    - 팬미팅, 사인회 및 지역 연고 이벤트를 통해 팬 충성도 강화
    - **지역 밀착형** 마케팅
      (ex. 지역 소상공인과 연계한 프로모션 및 이벤트)
  - 수도권: **대규모 프로모션** 및 디지털 마케팅
    - 디지털 검색량과 관중수 연계 프로모션 전개
      (ex. **검색 이벤트를 통해 할인 쿠폰 제공** 및 **디지털 광고** 캠페인)
- **축구: 디지털 중심 실시간 마케팅 및 팬 커뮤니티 강화**
  - 서울, 경기도: **디지털 중심** 마케팅 강화
    - **검색량 급등 시기**에 맞춘 **디지털 광고 캠페인**
    - 실시간 마케팅: ex. 라이브 스트리밍 중간 할인 프로모션 코드 제공
  - 팬 커뮤니티 활성화
    - **SNS 기반 팬 커뮤니티**를 통한 팬 소통 강화
    - 팬 참여형 콘텐츠: ex. 팬 응원 영상 공모전 및 Meme(밈)콘텐츠 제작
- **농구&배구: SNS 바이럴 마케팅 및 이벤트성 콘텐츠**
  - **SNS 바이럴 마케팅** 강화:
    - 스타 선수 및 인플루언서와 협업한 콘텐츠 제작
    - 짧고 강렬한 **하이라이트 클립 및 챌린지** 영상 제작
  - 이벤트성 콘텐츠 및 현장 체험 마케팅
    - **현장 직관 이벤트** 강화: ex. 선수와 함께하는 팬미팅, 현장 추첨
    - SNS **실시간 라이브**를 통해 **현장 감성 공유** 및 **디지털 상호작용 강화**

##### 결론 및 전략 요약:
- **지역별 특화 마케팅**: 야구는 부산과 대구에서 연고지 팬덤 강화, 축구는 수도권에서 디지털 마케팅 강화
- **디지털 중심 실시간 마케팅**: 축구와 농구/배구에서 SNS 중심 실시간 마케팅 및 팬 커뮤니티 활성화
- **이벤트성 콘텐츠 및 오프라인 이벤트**: 농구와 배구에서 이벤트성 콘텐츠 및 현장 체험형 마케팅 강화

---
## 6. 머신러닝

### 6.1 개요
연령대별 스포츠 검색률과 선호도 간의 높은 상관관계를 확인한 결과, 연령대별 스포츠 선호도를 예측하기 위한 데이터 분석을 진행하게 되었습니다.
이를 위해 **두 가지** 접근 방식을 설정했습니다:

**경기장별 관중 수 예측**

관중 수가 스포츠 선호도와 검색률에 직결된다고 판단하여 경기장별 관중 수 데이터를 분석하고, 이를 토대로 향후 관중 수 변화를 예측합니다.

**종목별 온라인 검색량 예측**

종목별 온라인 검색량 데이터를 기반으로 향후 검색 트렌드를 예측합니다.

해당 예측을 위해 채택한 데이터는 **'22년 - 24년 종목별 검색률 데이터'**, **'21년-24년 4개종목 경기장별 관중수 데이터**입니다.

#### 6.1.1 알고리즘 선택 과정

경기장별 관중 수 데이터 및 종목별 온라인 검색량 데이터는 모두 연속형 결과치가 필요하여 **회귀 알고리즘**과 **시계열 알고리즘**을 선택하게 되었습니다.

#### 6.1.2 계획 및 모델 선정

**1. 경기장별 관중 수 예측 모델**

경기장별 관중 수 예측은 **스포츠 선호도 및 검색량의 상관관계 분석 결과**를 기반으로 진행되었습니다.
예측을 위해 독립변수와 종속변수를 구분해야 했기에, **시계열 알고리즘이 아닌 회귀 알고리즘**을 적용하였습니다.

다양한 모델 중 아래 네 가지 모델로 성능 평가를 진행하였습니다.

**선형 데이터에 특화된 모델**: Linear Regression, MLP Regressor<br>
**비선형 데이터에 특화된 모델**: Random Forest, XGBoost<br>

그 결과 비선형 모델의 성능이 우수하게 나타났으며,
최종 평가 결과 **Random Forest 회귀** 모델이 가장 높은 예측 성능을 보여 해당 모델을 채택하였습니다.

**2. 종목별 온라인 검색량 예측 (출처: 네이버 데이터 랩)**

종목별 검색량이 많을수록 **온라인 마케팅 전략**을 강화할 수 있다는 점에 착안하여, **2025년 검색량을 예측**하였습니다.<br>

검색량 데이터는 일자별로 세분화되어 있어 **시계열 알고리즘**을 적용하였으며,<br>
데이터 양이 적을 때 최적화된 모델: **ARIMA**<br>
스포츠의 **시즌별 패턴 반영**이 가능한 모델: **SARIMA**<br>
두 가지 모델을 활용하여 **종목별 검색량 변화**를 예측하였습니다.

### 6.2 경기장별 관중수 예측

#### 6.2.1 모델 데이터 전처리
1. 경기장(location): 각 경기장에 대해 관중수 평균값을 대표값으로 사용하여 **타깃 인코딩**을 진행합니다.
2. 월 데이터: 월의 경우 달이 많지 않기 때문에 범주형 변수로 변환하고자 **원-핫 인코딩**을 진행하였습니다.
3. 검색률, 선호도의 성능을 높이기 위해서 **연속형 변수 스케일링**을 진행하였습니다.
4. 필요없는 열은 제거하고 순서를 재정리하였습니다.

#### 6.2.2 모델 학습 및 평가
- 선형 회귀, 비선형 회귀 정확도 비교를 위해 선형데이터 특화 모델과 비선형 데이터 특화모델로 평가를 진행하여 RMSE, R2 값을 확인해보았습니다.
- 그 결과:
  - Linear Regression - RMSE: 3248.91, R²: 0.7422
  - Random Forest - RMSE: 2892.38, R²: 0.7957
  - XGBoost - RMSE: 2977.3, R²: 0.7835
  - MLP Regressor - RMSE: 3405.0, R²: 0.7168
- 네 종류의 모델 중 랜덤 포레스트의 성능값이 가장 우수하여 해당 알고리즘을 채택하게 되었습니다.
- 이후 랜덤포레스트 회귀에 하이퍼파라미터 튜닝을 진행하였고 조정 결과 정확도가 더 높게 나오게 되어 조정 모델을 채택하게 되었습니다.

#### 6.2.3 모델 예측 및 결과 값
<img width="367" alt="image" src="https://github.com/user-attachments/assets/f7eaa9c1-8fe4-47e6-a197-42cfb171b57b" />

최종 정확도가 83퍼센트 정도로 확인되어 해당 Gridsearch RFR 모델에 기존 데이터의 검색량고 선호도 점수를 임의로 기입하였고 각 종목별로 2025년 경기일정을 다시 수집하여 해당 경기의 일자별 관중수를 도출하였습니다.
그리고 실제 관중수를 수집하기 어려운 농구와 달리 배구의 경우 1월 24일까지 실제 관중수를 수집할 수 있었으며, 해당 데이터를 통해 1월 배구 경기의 일자별 관중수와 비교하여 예측값과 비교해보았습니다.
그 결과**오차의 최소값은 144명이었으나 최대 1857명까지 오차가 발생하는 것을 확인**할 수 있었으며, 더 많은 데이터를 축적하여 정확도를 높일 필요가 있다고 판단하였습니다.

### 6.3 종목별 온라인 검색량 예측

#### 6.3.1 모델 데이터 전처리
- 4개 종목을 각각 나누어 동일 코드로 예측을 진행하였습니다.

#### 6.3.2 데이터 형태 파악
- 정상성 테스트를 진행하여 정상성을 확인하고 정상성을 띄지 않을 경우 차분을 진행한 후 정상성을 띄게하여 p, d, q값을 추출하였습니다.
- 정상성 테스트의 경우 ADF 테스트를 통해 p값을 추출하여 p값이 0.05 미만일 경우 차분을 진행하고, 이상일 경우 차분 진행 없이 바로 PACF/ACF 테스트를 진행하였습니다.
- 진행 결과 각 종목별 p,d,q값은 다음과 같습니다.
  - 야구 : order = (8, 1, 2)
  - 축구 : order = (4, 0, 2)
  - 농구 : order = (1, 1, 3)
  - 배구 : order = (5, 0, 21)
 
#### 6.3.3 모델 학습 및 평가
- 각 종목별로 평가를 진행하여 성능을 알아보았습니다.
- 그 결과 다음과 같은 결과값을 확인할 수 있었습니다.
  - 야구 )
  - MAPE: 80.82044636579982%
  - Mean Squared Error: 1059.45
  - Root Mean Squared Error: 32.55
  - 축구 )
  - MAPE: 99.18878641250922%
  - Mean Squared Error: 115.87
  - Root Mean Squared Error: 10.76
  - 농구 )
  - MAPE: 78.04688230192588%
  - Mean Squared Error: 7.57
  - Root Mean Squared Error: 2.75
  - 배구 )
  - MAPE: 454.87302567607594%
  - Mean Squared Error: 25.81
  - Root Mean Squared Error: 5.08
- 이 값들을 통해 MAPE값과 RMSE값이 양호한 것을 확인하였으나 시각적으로 예측이 잘 반영되지 않았고, 계절성으로 패턴을 파악할 수 있는 SARIMAX로 전환하여 다시 학습을 진행하였습니다.

#### 6.3.4 모델 예측 및 결과 값
<img width="388" alt="image" src="https://github.com/user-attachments/assets/df7b248f-3d17-435e-a758-dde00889872d" />

- 원본 데이터의 양질이 떨어지거나 검색량에 미치는 변수의 종류가 많을 경우 검색률 데이터가 복잡해져 시계열 예측 정확도가 떨어지는 문제가 발생하게 되는데, 해당 데이터의 경우 야구 뿐 아니라 **모든 종목의 예측도가 떨어져 30일의 예측만 유효**하다는 결론이 도출되었습니다.
- 30일 예측의 경우 전체 시즌성을 파악하는 것에 어려움이 있어 기존에 예측하고자 했던 분기별 검색량 추이를 파악할 수 없으므로 해당 예측을 통해 인사이트를 얻는 것은 어려울 것이라 판단하게 되었고, 해당 데이터는 최종 분석에서 사용하지 않게 되었습니다.



   




