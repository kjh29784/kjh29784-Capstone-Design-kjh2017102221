# Deep learning-based visitor detection & counting system (Capstone design 2020-2)

팀명 : 김지현 

팀구성 총인원 : 총 1명

* Student list

|구분|성명|학번|소속학과|학년|연락처|이메일|
|------|-----|------------|-------|---|------------|---------------------|
|대표 학생|김지현|2017102221|도예학과|3|01021895742|kjhlolo9784@gmil.com|

Yolov5와 DeepSort를 통한 방문객 탐지 및 추적과 feature extration을 통해 영상에서 방문객의 특징을 추출하는 추적하는 모델을 제작하였습니다.
현재는 방문객의 마스크 착용 유무를 판단합니다.

## 동작영상

![output_1 (2)](https://user-images.githubusercontent.com/68112175/125926253-b9f306f0-f417-42a9-91f3-aa135778c8a9.gif)

![output_2 (2)](https://user-images.githubusercontent.com/68112175/125926096-01bf8f6a-9e52-4a2d-9a80-aac76b323c7f.gif)

## Overview
* Needs, problems

  잠시만요(신분 인증 방법 및 시스템)의 한계점 극복

   이전 연구의 한계
    -무단 출입 발생 
    ex)손님 중 한 명만 신분증을 인식하고 문이 열리면 일행들이 무단으로 입장하는 문제 발생

   이에 매장 내 카메라를 설치하여 출입자를 카운팅함으로써 무단 출입을 검출하는 솔루션 개발이 필요함

## 주요내용
Yolov3 구조를 기반으로WIDERFACE dataset을 사용하여 학습한다.
제작된 네트워크를 기반으로 실시간 Detection알고리즘을 제작한다.
Deepsort 구조를 개선하여, 얼굴 추적에 맞도록 Feature Descriptor를 Arcface로 대체하여 얼굴에 맞는 feature로 추적 알고리즘을 제작한다. 이 때 임의의 사람에 대해서 고유한 id를 주어 추적하도록 한다.
정적인 사람 이미지에 대해서 Feature 정보를 추출하고, 이를 기반으로 영상에 존재하는 사람을 추적하는 알고리즘을 개발한다. 알고있는 사람의 얼굴 정보가 주어진다면, 해당 사람을 계속해서 추적하도록 한다.

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
![output_1 (2)](https://user-images.githubusercontent.com/68112175/125926058-95861104-6758-46d8-a746-4ea0b88c212d.gif)

