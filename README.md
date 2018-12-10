# SMS_SPAM_Filtering

# 프로젝트 개요
- 대상
  * 베이즈 정리를 활용한 SMS 스팸 필터링

- 목적
  * 무분별한 스팸 문자로 인한 피해를 줄이고자 함

- 일정
  * 2018 - 06 - 04 ~ 2018 - 06 - 18

- 도구
  * Excel, Python


## 요약
 오늘 날 스팸 문자로 많은 사람들이 피해를 보고 있습니다. 그래서 본 프로젝트는 조건부 확률 및 베이즈 정리를 활용하여 SMS 스팸 필터링을 하는 것을 목표로 합니다. 이를 위해 Microsoft Excel과 Python을 프로그래밍 도구로 사용하여 해결할 것 입니다.


## Introduction

 현대 사회는 정보화 시대가 발전함으로써, 많은 사람들이 스마트폰을 활용하여 서로 소통하고 있습니다. 이에 따라 무분별하게 많은 스팸 문자, 메일로 인한 피해가 증가하고 있는 추세입니다.[1] 그래서 본 프로젝트는 그 중에서도 스팸 문자 필터링을 하는 것을 목표로 합니다.
 과제 해결 방법으로는 조건부 확률 및 베이즈 정리를 활용할 것입니다.[2] 실험에 쓰이는 데이터는 캘리포니아 대학교 어바인에서 제공하는 ‘SMS Spam Collection Data Set’을 사용할 것입니다.[3] 본 데이터는 총 5574개의 SMS(문자) 데이터를 가지고 있으며, 그 중 Spam SMS은 747개 (약 13.40%), Spam SMS가 아닌 것은 4827개 (약 86.59%)로 이루어져 있습니다.
 또한 본 실험에서는 데이터 처리를 하기 위해 Python과 정규 표현식[4] 이용하여 데이터 집합의 단어 빈도수를 파악하고, Excel의 여러 가지 함수들을 사용하여 자주 나오는 단어들이 스팸 문자일 확률을 구합니다.


## 단어 빈도수 분석
 기존의 데이터는 순수하게 문자 데이터가 스팸인지 아닌지에 대한 정보 밖에 없으므로, 어떤 단어의 빈도수가 많은지에 대해 분석이 필요합니다. 이에 따라 Excel에는 해당 기능이 없으므로, 아래 [그림 1]과 같이 Python에서 정규 표현식[4]을 사용하여 단어 빈도수를 분석 했습니다.

![Image_1](https://github.com/HyunIm/SMS_SPAM_Filtering/blob/master/1_Materials/2_Image/%EA%B7%B8%EB%A6%BC%201.PNG)

그림 1. 단어 빈도수 분석 프로그램 소스 코드

 본 프로그램에서는 데이터를 읽어서, 정규 표현식을 쉽게 표현하기 위해 대문자를 모두 소문자로 바꾸었습니다. 그 후 a부터 z까지 해당되는 3글자 이상, 20글자 이하의 단어들을 찾았습니다. 글자 수를 제한시키며 단어들을 찾은 이유는 2글자 이하의 단어들에는 전치사 같은 불필요한 단어들을 제외하기 위해서이며(예 : if, or, in, ...) 21글자 이상의 단어들은 단어로써 의미를 가지고 있지 않다고 판단하여 제외하였습니다.
 그렇게 해서 아래 [그림 2]는 스팸 데이터에서 빈도수 높은 단어 20개를 추출하여 그래프로 나타낸 모습입니다. 그림과 같이 전체 데이터와 스팸 데이터에서 빈도수가 차이가 많이 나는 데이터와 차이가 거의 없는 데이터가 섞여있는 모습입니다.

![Image_2](https://github.com/HyunIm/SMS_SPAM_Filtering/blob/master/1_Materials/2_Image/%EA%B7%B8%EB%A6%BC%202.png)

그림 2. 단어 빈도수


## 단순 스팸 확률
 본 주제에서는 앞서 선정한 단어들의 단순 스팸 확률을 구해보고자 합니다. 방법은 Excel에서 COUNTIF 함수를 활용하여 전체 데이터에서의 빈도수와 스팸 데이터에서의 빈도수를 구합니다. 그 후 스팸/전체 빈도수를 하여 스팸일 확률을 구합니다.
 결과는 아래 그림과 같으며 스팸 확률은 약 20%부터 100%까지 다양한 분포로 구성되어 있습니다. 하지만 아래 확률로만은 몇 % 이상을 스팸으로 추정할 것 인지 판단할 수 없고, 주어진 데이터 집합에 종속적인 결과가 도출될 수 있으므로 아래 추가적인 실험을 통해 베이즈 정리를 활용해 스팸 추정 여부를 판단 할 것 입니다.

![Image_3](https://github.com/HyunIm/SMS_SPAM_Filtering/blob/master/1_Materials/2_Image/%EA%B7%B8%EB%A6%BC%203.png)

그림 3. 단순 스팸 확률


## 베이즈 정리를 활용한 스팸 확률
 베이즈 정리를 활용하여 스팸 확률을 구하고자 합니다.[2] 실험에 쓰이는 데이터는 전체 문자 중 스팸 문자의 비율과 정상 문자의 비율, 정상 문자와 스팸 문자에 특정 단어가 있을 확률입니다.
 이하 Ai는 각 단어들, B는 Spam 문자일 확률, 는 Spam 문자가 아닐 확률로 보겠습니다. 우선 P(B)와 P()는 앞서 Introduction에서 서술한대로 각각 약 13.40%, 약 86.59%의 확률을 가지고 있습니다. P(Ai|B)와 P(Ai|)는 Excel의 COUNTIF 함수를 사용해 계산한 결과 값을 사용합니다.
 그렇게 해서 P(Ai|B)와 P(B)를 곱하여 P(B|Ai)를 구하고, P(Ai|)와 P()를 곱하여 P(|Ai)를 구합니다. 그 후 P(|Ai) - P(B|Ai)가 음수가 나오는 단어들은 스팸으로 추정합니다. 결과 값은 약 23%부터 약 –0.27%까지 넓은 분포로 결과가 나왔습니다. 아래 [그림 4]는 스팸 문자로 추정된 단어는 붉은색으로 음영 처리를 해놓은 모습입니다. 단어 목록으로는 mobile, txt, www, prize, claim으로 총 5개입니다.

![Image_4](https://github.com/HyunIm/SMS_SPAM_Filtering/blob/master/1_Materials/2_Image/%EA%B7%B8%EB%A6%BC%204.png)

그림 4. 스팸 문자로 판단된 단어


## Simulation results
 실험 결과 앞서 선택했던 20개의 단어들에 대한 결과가 아래 [표 1]과 같이 나왔습니다. 표는 특정 단어가 나왔을 때 스팸일 확률이 높은 단어 순으로 정렬되어 있으며, 본 실험에서는 P(|Ai) - P(B|Ai)가 음수가 나오는 단어들인 claim, prize, www, txt, mobile이 나올 경우 스팸으로 간주한다는 가정 하에 모집단으로 검증 할 예정입니다.

표 1. 베이즈 이론을 활용한 특정 단어가 스팸 메일일 확률 결과.

|  Word  | P(Ai|B) | P(Ai|B^c) | P(B|Ai) | P(B^c|Ai) |  Result |
|:------:|:-------:|:---------:|:-------:|:---------:|:-------:|
|  claim |  2.08%  |   0.00%   |  0.28%  |   0.00%   | -0.279% |
|  prize |  1.60%  |   0.00%   |  0.21%  |   0.00%   | -0.214% |
|   www  |  1.76%  |   0.05%   |  0.24%  |   0.04%   | -0.193% |
|   txt  |  3.19%  |   0.29%   |  0.43%  |   0.25%   | -0.176% |
| mobile |  2.64%  |   0.27%   |  0.35%  |   0.23%   | -0.120% |
|  cash  |  1.22%  |   0.23%   |  0.16%  |   0.20%   |  0.036% |
|  stop  |  2.06%  |   0.79%   |  0.28%  |   0.68%   |  0.408% |
|  reply |  1.76%  |   0.79%   |  0.24%  |   0.68%   |  0.448% |
|  free  |  3.57%  |   1.18%   |  0.48%  |   1.02%   |  0.543% |
|  text  |  2.46%  |   1.56%   |  0.33%  |   1.35%   |  1.021% |
|   won  |  1.36%  |   1.63%   |  0.18%  |   1.41%   |  1.229% |
|  only  |  1.53%  |   2.32%   |  0.21%  |   2.01%   |  1.804% |
|  send  |  1.29%  |   2.53%   |  0.17%  |   2.19%   |  2.018% |
|  call  |  6.23%  |   5.20%   |  0.83%  |   4.50%   |  3.668% |
|  just  |  1.42%  |   5.24%   |  0.19%  |   4.54%   |  4.347% |
|  your  |  4.34%  |   6.87%   |  0.58%  |   5.95%   |  5.367% |
|   get  |  1.74%  |   6.98%   |  0.23%  |   6.04%   |  5.811% |
|  have  |  2.30%  |   8.24%   |  0.31%  |   7.14%   |  6.827% |
|   now  |  3.75%  |   9.42%   |  0.50%  |   8.16%   |  7.654% |
|   you  |  7.32%  |   27.60%  |  0.98%  |   23.90%  | 22.918% |

 총 50개의 무작위 데이터를 추출해서 테스트 해본 결과 50개 중 8개의 데이터가 스팸 데이터이며, 앞서 말한 5개의 단어가 나올 경우 스팸이라고 판단해보겠습니다.
 결과는 스팸이 아닌 데이터에서 위 단어가 나온 경우는 0%였고, 스팸인 데이터에서 위 단어가 나온 경우는 8번 중 6번으로, 75%였습니다. 스팸 데이터에서 각 단어가 나온 빈도는 아래 [그림 5]와 같습니다.

![Image_5](https://github.com/HyunIm/SMS_SPAM_Filtering/blob/master/1_Materials/2_Image/%EA%B7%B8%EB%A6%BC%205.png)
그림 5. 검증 결과


## Conclusions
 현재 방송통신위원회에서 오차단‧과차단 등 발생 우려로 80~90% 수준의 차단율을 유지하도록 권장하는 것을 감안한다면[1], 본 실험을 통한 알고리즘은 검증 결과 약 75%의 차단율을 가지고 있으므로, 성공적인 결과라고 볼 수 있습니다.


## 참고문헌
[1] 방송통신위원회, “2017년 하반기 스팸 유통현황”, 2018.
[2] Hoon Yoo, “조건부 확률 및 베이즈 정리”, 2018.
[3] University Califonia Irvine, “SMS Spam Collection Data Set”, 22 June 2012, https://archive.ics.uci.edu/ml/datasets/SMS+Spam+Collection (2018년 06월 04일 검색).
[4] Kenneth C. Louden, “Compiler Construction Principles and Practice”, PWS　Publishing Company, pp.34-46, 1997.