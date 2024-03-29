# DACON_Citrus  [(Link)](https://dacon.io/competitions/official/236038/overview/description)

## 🏆 Result
## Public score 3rd 0.07214 | Private score 12th 0.07303

주최 : 제주 테크노파크

주관 : DACON

===========================================================================
  
## 🧐 About
감귤 생산량 추정을 위한 한정된 조사인력이 약 22,000ha에 이르는 방대한 면적을 조사하기에,  
시간이 오래 걸릴 뿐만 아니라 조사인력도 부족하며 조사인력은 관측조사에 의존하기 때문에 일관되지 않은 데이터 수집이 이뤄지는 문제점이 존재함,   
따라서 감귤나무의 나무 생육 상태, 엽록소 및 새순 정보로부터 감귤 착과량을 정확히 예측할 수 있다면 위와 같은 문제를 모두 해결할 수 있기 때문에  
```감귤의 생산량 추정에 도움을 줄 수 있는 감귤 착과량 예측 AI 모델을 개발```하는 대회  


### **대회 느낀점**
train 데이터 2200여개 test 데이터 2200여개로   
과적합 위험이 있었고, 해결하기 위해 앙상블 기법을 사용하였지만   
public score 3rd로 마무리 하였던 점수가  
private score 에서 12등으로 밀려  
최종 10위 안에 들지 못해 수상은 하지 못하였습니다.  
하지만 대회 종료 후 모델만 ET로 바꿔서 돌려보았더니    
private score가 5등까지 올랐습니다.    

이러한 결과를 받기 전까지는 AutoML이 전통적인 ML보다 무조건적으로 우월하다고 생각했으나,   
실제로 이번 대회도 그렇고 여러 측면에서(시간과 자원) 비교해보니 이러한 판단이 절대적이지 않다는 것을 알게 되었다.  

이번 대회를 ML 기술을 선택할 때 단순히 트렌드나 편의성에 의존하는 것이 아니라,   
실제 문제의 특성과 요구사항을 철저히 분석하고 이해해야 함을 깨달았습니다.   
AutoML은 특정 상황에서 매우 유용하고 빠른 결과를 제공할 수 있지만,   
모든 상황에서 가장 최적의 선택이 되지는 않다는 것을 깨달았으며   
특히, 세밀한 조정이나 특정 문제의 복잡성을 고려해야 할 때는 전통적인 ML 접근법의 유연성과 깊이를 활용해야 할 필요성을 느꼈습니다.  

또한, AutoML과 전통적인 ML 간의 장단점을 정확히 인식하고,   
그에 따라 적절한 전략을 세우는 것이 중요하다는 것을 깨달았습니다.   
이러한 인식은 단순히 도구나 기술을 선택하는 데에 그치지 않고,   
전체적인 프로젝트의 방향성과 효율성을 높이는 데 큰 도움이 될 것이라 생각합니다.  

이번 경험을 통해 앞으로의 프로젝트에서는 문제의 본질과 요구사항을 더 깊이 파악하고,   
그에 맞는 최적의 기술과 방법론을 선택하는 데 더욱 신중하게 접근하게 될 것입니다.  

그래서 비록 수상권에 있다가 수상을 아쉽게 못했다는 감정도 분명 존재하지만,  
내가 가지고 있던 AutoML만 고집하는 고정관념을 깰 수 있고  
더욱 유연한 사고를 가지게 되는 계기여서 얻을게 많은 대회라 생각합니다.  


## 📖 Dataset

```

train.csv
ID : 과수나무 고유 ID
착과량(int) : 실제 감귤 착과량 (Target)
나무 생육 상태 Features (5개)
수고(m), 수관폭1(min), 수관폭2(max), 수관폭평균(수관폭1과 수관폭2의 평균)

데이터 기입은 cm 단위

새순 Features (89개)
2022년 09월 01일 ~ 2022년 11월 28일에 일별 측정된 새순 데이터

엽록소 Features (89개)
2022년 09월 01일 ~ 2022년 11월 28일에 일별 측정된 엽록소 데이터



test.csv
ID : 과수나무 고유 ID
나무 생육 상태 Features (5개)
수고(m), 수관폭1(min), 수관폭2(max), 수관폭평균(수관폭1과 수관폭2의 평균)

데이터 기입은 cm 단위

새순 Features (89개)
2022년 09월 01일 ~ 2022년 11월 28일에 일별 측정된 새순 데이터

엽록소 Features (89개)
2022년 09월 01일 ~ 2022년 11월 28일에 일별 측정된 엽록소 데이터

```



## 🔧 Feature Engineering
```
Feature selection.

새순값에 대한 평균 기울기
- 새순은 날짜가 지날수록 점점 감소하는 추세를 보이는 경향
새순 측정 피처 전처리
- 새순 측정 피처의 값은 모두 소수점 한자리
  -> 이는 반올림이나 올림, 내림과 같은 전처리 과정을 거친 값이라 가정
- 새순은 날짜가 지날수록 점점 감소하는 추세를 보이는 경향
  -> t+1시점의 새순값이 t시점보다 새순값이 높다면, t시점의 새순값으로 변경
새순 관련 파생변수
- 새순값에 대한 통계값으로 파생변수 생성
- 새순값에 대한 착과량을 비교하여 구간별 라벨링
수관폭 관련 파생변수
- 수관폭1()과 수관폭2()에서 최댓값과 최솟값이 반대로 되어 있는 경우 처리

```
