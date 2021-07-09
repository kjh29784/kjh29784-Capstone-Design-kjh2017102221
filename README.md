# Deep learning-based visitor detection & counting system (Capstone design 2020-2)

팀명 : 김지현

팀구성 총인원 : 총 1명

* Student list

|구분|성명|학번|소속학과|학년|연락처|이메일|
|------|-----|------------|-------|---|------------|---------------------|
|대표 학생|김지현|2017102221|도예학과|3|01021895742|kjhlolo9784@gmil.com|



## Overview
* Needs, problems

  잠시만요(신분 인증 방법 및 시스템)의 한계점 극복

   이전 연구의 한계
    -무단 출입 발생 
    ex)손님 중 한 명만 신분증을 인식하고 문이 열리면 일행들이 무단으로 입장하는 문제 발생

   이에 매장 내 카메라를 설치하여 출입자를 카운팅함으로써 무단 출입을 검출하는 솔루션 개발이 필요함

* Goals, objectives (evaluation)

|    구분    |      지표      |             목표             |   Progress   |
|------------|---------------|------------------------------|--------------|
| 정량적 목표 |   인식 정확도  |     recall rate 90% 이상     |              |
|            |    인식 속도   |          300ms 이내          |              |

|    구분    |                    목표                      |   Progress   |
|------------|----------------------------------------------|--------------|
| 정성적 목표 |청소년을 유해요소로부터 보호                    |              |
|            |소상공인의 억울한 피해사례 사전예방             |              |
|            |4차 산업혁명 시대에 맞추어 무인점포 발달 기여    |              |
|            |출입자 관리로 코로나 방역 도움                  |              |

## Schedule
|           Contents         | March | April |  May  | June  |   Progress   |
|----------------------------|-------|-------|-------|-------|--------------|
| 과제 기획 및 파이프라인 생성 |   O   |       |       |       |              |
|         데이터 수집         |   O   |   O   |       |       |              |
|        프로그램 개발        |       |   O   |   O   |       |              |
|         데이터 학습         |       |       |       |   O   |              |
|   Final test 및 성과보고    |       |       |       |   O   |              |



## Results
- MOT 16 데이터셋 벤치마크 결과

![image](https://user-images.githubusercontent.com/68112175/125117329-0fce0200-e129-11eb-99b4-d78692da8e6b.png)

- mask detection 성능 그래프

![image](https://user-images.githubusercontent.com/68112175/125117102-a948e400-e128-11eb-97b1-fa95dd3309bf.png)


## Conclusion
   가. 기대효과
청소년을 유해요소로부터 보호
소상공인의 억울한 피해사례 사전예방
4차 산업혁명 시대에 맞추어 무인점포 발달 기여
출입자 관리로 코로나 방역 도움 

   나. 활용방안
잠시만요(신분 인증 방법 및 시스템) 등과 같은 출입 시스템에 visitor detection & counting system을 함께 사용함으로써 무단 출입 방지 및 손님의 feature 수집을 통한 데이터 구축을 가능케 함

  다. 결론 및 제언
벤치마크의 전반적인 점수가 예상치보다 낮게 나옴에 따라 앞으로 모델 변경과 추가적인 데이터셋 구축, 및 학습시켜 모델의 성능을 향상시켜야 함.
또한 더욱 많은 방문자의 특징을 추출함으로 방문자 통계 분석에 추가적으로 사용할 수 있도록 발전시켜야 함.

## Reports
[소프트웨어융합캡스톤디자인02(중간보고서_김지현).pdf](https://github.com/kjh29784/Capstone-Design-Example/files/6793098/02._.pdf)

[소프트웨어융합캡스톤디자인03(최종보고서_김지현_2017102221).pdf](https://github.com/kjh29784/Capstone-Design-Example/files/6793102/03._._2017102221.pdf)

![image](https://user-images.githubusercontent.com/68112175/125117557-620f2300-e129-11eb-830b-d0dc6ae01eb5.png)
