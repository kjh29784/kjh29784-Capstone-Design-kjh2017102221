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
 1. Yolov5 모델을 기반으로 MOT16 dataset과 mask착용 이미지 데이터셋을 사용하여 학습
 2. 학습시킨 방문객 검출 Yolov5모델을 DeepSort구조를 이용하여 추적할 수 있도록 개발
 3. Tracking 모델을 통해 기록된 보행자의 box 정보를 바탕으로 OpenCV를 통해 방문자 출입 Counting 알고리즘 개발
 4. 1번에서 학습시킨 mask 검출 모델을 3번 모델과 결합

## 세부 사항
1. Yolov3 -> yolov5로 모델 변경
    Tracking모델의 작동 과정을 고려했을때 Detector의 성능이 매우 중요하다고 판단하였습니다. 이에 pytorch로 구현하여 더욱 쉽게 환경 구성이 가능하고, 지난 모델과는 다른(Yolov3) CSPNet기     반의 BackBone을 이용하여 더욱 높은 성능을 고려 할 수 있는 Yolov5로 모델을 변경하였습니다. 또한 방문객만을 검출하는 것이 목표임으로 보행자 이미지 데이터 셋인 MOT16 데이터셋을 가지고     방문자에 국한하여 모델을 Train시켰습니다. MOT16의 훈련 데이터 셋은 5316개의 프레임, 110407개의 annotation된 박스를 가지고 있습니다. 이는 다양한 방문객의 모습을 반영하기에 충분하다고     판단하였습니다. yolov5로의 모델 변경은 mask detector에도 똑같이 적용되었습니다. 
2. DeepSort모델을 통한 보행자 추적 모델 개발
    기존 DeepSort가 보행자를 기반으로 하여 Track matching을 진행했기 때문에 큰 수정없이 DeepSort Repository를 fork하여 FeatureDescriptor의 Input shape이 2:1 비율로 하여 id매칭을 진행     하였습니다.
3. 출입자 Counting 알고리즘 개발
    검출한 보행자 박스 데이터를 이용, track.py OpenCV를 통해 데이터를 Y 좌표 기준으로 5등분 하여 센터 라인과의 비교 및 이중 확인으로 count를 증가 혹은 감소시킵니다.
      ```
          flag_t = False
          flag_b = False
          flag_tm = False
          flag_bm = False

          if y1 == center_line:
              flag_t = True
          if y2 == center_line:
              flag_b = True

          if (y1 + (y2 - y1) * (0 / 5)) < center_line and center_line < (y1 + (y2 - y1) * (1 / 5)):
              flag_tm = True
          if (y1 + (y2 - y1) * (4 / 5)) < center_line and center_line < (y1 + (y2 - y1) * (5 / 5)):
              flag_bm = True

          if len(counting_id) == 0:
              if flag_t == True:
                  counting_id.append([id, 1])
              if flag_b == True:
                  counting_id.append([id, 2])
          else:
              for j, temp in enumerate(counting_id):

                  index = -1
                  if temp[0] == id:
                      index = j
                  if index != -1:
                      if counting_id[j][0] == id and counting_id[j][1] == 1:
                          if flag_tm == True:
                              counting_id[j][1] = 3
                              counting -= 1
                              print("2,", counting)
                      elif counting_id[j][0] == id and counting_id[j][1] == 2:
                          if flag_bm == True:
                              counting += 1
                              counting_id[j][1] = 4
                              print("3,", counting)
              if index == -1:
                  if flag_t == True:
                      counting_id.append([id, 1])
                  if flag_b == True:
                      counting_id.append([id, 2])

      cv2.putText(img, "counting: " + str(counting), (10, 50), cv2.FONT_HERSHEY_PLAIN, 2, [255, 255, 255], 2)
      f2.close()
      return img, counting, counting_id
      ```
## 4. 마스크 검출 모델과 방문자 추적 및 카운팅 모델 결합
  마스크 검출 모델의 detect.py에서 .mp4 파일으로부터 마스크 정보를 txt파일로 추출하고, 이를 트래킹 모델의 track.py에서 받아와 이에 맞는 ID의 보행자에게 데이터를 부여합니다.
        
      ```
      mask_flag = False
      print(frame_idx)
      all_mask = []
      try:
          f = open("/content/yolov5/runs/detect/exp/labels/mask_test_" + str(frame_idx) + ".txt", 'r')
          line = f.readlines()
          mask_flag = True
          all_mask = []

          for mask in line:
              temp = mask.split(" ")
              temp[0] = int(temp[0])
              temp[1] = int(temp[1])
              temp[2] = int(temp[2])
              temp[3] = int(temp[3])
              temp[4] = int(temp[4])
              all_mask.append(temp)
      except Exception:
          mask_flag = False
      f2 = open("ouputs.txt", 'a')

      for i, box in enumerate(bbox):
          x1, y1, x2, y2 = [int(i) for i in box]
          x1 += offset[0]
          x2 += offset[0]
          y1 += offset[1]
          y2 += offset[1]
          id = int(identities[i]) if identities is not None else 0
          color = compute_color_for_labels(id)
          label = '{}{:d}'.format("", id)
          if mask_flag == True:
              for mask in all_mask:
                  if x1 < mask[1] and y1 < mask[2]:
                      if y1 > mask[3] and y2 > mask[4]:
                          cv2.rectangle(
                              img, (mask[1], mask[2]), (mask[3], mask[4]), color,1)
                          if mask[0] == 1:
                              mask_txt = 'no mask'
                          elif mask[0] == 0:
                              mask_txt = 'mask'
                          print(mask_txt)
                          cv2.putText(img, mask_txt, (mask[1],mask[2]), cv2.FONT_HERSHEY_PLAIN, 2, [255, 255, 255], 2)

                          f2.write(str(frame_idx)+ ","+label+","+mask_txt+"\n")
          # box text and bar

          t_size = cv2.getTextSize(label, cv2.FONT_HERSHEY_PLAIN, 2, 2)[0]
          cv2.rectangle(img, (x1, y1), (x2, y2), color, 3)
          cv2.rectangle(
              img, (x1, y1), (x1 + t_size[0] + 3, y1 + t_size[1] + 4), color, -1)
          cv2.putText(img, label, (x1, y1 +
                                   t_size[1] + 4), cv2.FONT_HERSHEY_PLAIN, 2, [255, 255, 255], 2)
          cv2.line(img, (0, center_line), (img.shape[1], center_line), [255, 255, 255], 2)
      ```
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
카운팅하는 방식에 대해서도 새로운 방식을 도출해낼 필요가 있음.

## Reports
[소프트웨어융합캡스톤디자인02(중간보고서_김지현).pdf](https://github.com/kjh29784/Capstone-Design-Example/files/6793098/02._.pdf)

[소프트웨어융합캡스톤디자인03(최종보고서_김지현_2017102221).pdf](https://github.com/kjh29784/Capstone-Design-Example/files/6793102/03._._2017102221.pdf)
![output_1 (2)](https://user-images.githubusercontent.com/68112175/125926058-95861104-6758-46d8-a746-4ea0b88c212d.gif)

