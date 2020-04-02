# forked from zmdsjtu/Hand_gesture

# 제스처 인식 및 세분화

손바닥, 주먹, V자 등 네 가지 제스처의 인식 및 분할을 지원합니다.

- fhog 기능을 기반으로 한 제스처 추출 및 인식

- 제스처 Bouding Box 및 범주를 기반으로 특징점 위치 지정
- boudingbox 추적의 안정성을 보장하는 MedianFlow 기반 추적 알고리즘

# Requirements

- [OpenCV3.1 + contribute](http://opencv.org/downloads.html)

- [dlib19.9](http://dlib.net/)



## 프로그램 운영 효율성


- 프레임 속드는 35fps+
환경은 Intel I5-4210M 프로세서, 단일 스레드 단일 프레임 사진 28ms

감지 시간은 18ms이고 추적은 2ms이며, 특징점은 5ms이며, 나머지 인터페이스 및 이미지 작업은 3ms입니다.

## 프로그램 실행 화면

- 프로그램 실행을위한 기본 인터페이스


![Demo](/snapshot/main3.png)

- 프로그램 비디오 demo

[비디오Demo](/snapshot/video_demo.avi)

- 특징점 위치 결정 효과

손바닥-36포인트

![palm](/snapshot/palm.png)

주먹-14포인트

![fist](/snapshot/fist.png)

V자-14포인트

![scissor](/snapshot/scissor.png)

엄지손가락-14포인트

![thumb](/snapshot/thumb.png)

스켈레톤 그리기와 같은 기능 점을 기반으로 많은 확장을 만들 수 있습니다.

![skeleton](/snapshot/skeleton.png)

## 프로그램 작동 지침

- 콘솔 입력 비디오 디렉토리
- 숫자를 입력하면 해당 카메라 수를 켭니다.
- ESC 종료

## 프로그램 설정 지침
- Threshold: 다양한 제스처 감지를위한 임계 값 조정
- 2X、3X、4X: 이미지 감지 시간을 줄이기 위해 다른 이미지 스케일
- show time: 각 사진의 촬영 시간을 표시할지 여부
- Bouding(음성): Bouding Box 표시 여부
- Recognition: 감지 결과 표시 여부
- Segmentation: 제스처 세그먼테이션 결과를 표시할지 여부 (왼쪽 아래 모서리)
- Points: 제스처의 특징점을 표시할지 여부

## 프로그램 흐름
순서도에 표시된대로
- 프로그램 시작시 fHog 기능 피라미드를 기반으로 여러 제스처 모델을 일치시켜 가장 높은 점수와 부딩 상자가있는 제스처를 얻었습니다.
- MedianFlow 추적 알고리즘으로 현재 box 및 이미지 초기화
- 추적 알고리즘에 의해 획득 된 이미지가 특징점 회귀 모듈에 출력되어 특징점을 얻는 bouding box
- 특징점은 각 제스처의 윤곽 점이며, 제스처가있는 영역을 분할하는 데 사용할 수 있습니다.
- 추적이 실패하면 초기 추적 알고리즘이 탐지 결과와 함께 재사용됩니다.
- 또한, 검출 모듈과 추적 알고리즘의 인식 결과가 다르거 나 부딩 박스의 중첩 영역이 70 % 미만인 경우, 추적 실패가 결정된다.
![Flowchart](/snapshot/flowchart.png)

## 알고리즘 특징
- fHog기능은 손의 윤곽 특징을 잘 특성화 할 수 있습니다.
- 피라미드는 여러 스케일을 감지하는 문제를 해결합니다.
- 제스처를 식별하기 위해 점수를 감지
- 사진 크기 조정으로 사진을 이동하는 데 시간이 많이 걸리는 문제를 완화 할 수 있습니다.
- MedianFlow알고리즘은 추적 손실 또는 실패를 어느 정도 결정할 수 있습니다.
- MedianFlow알고리즘이 더 효율적이며 스케일링되지 않은 사진의 단일 프레임 추적 시간은 10ms 정도입니다.
- bouding box기반의 특징점 회귀는 단일 손가락 끝, 손바닥 등 각 제스처의 특정 위치에 위치 할 수 있습니다.
- 피처 포인트 기반 세분화는 피부색 기반 세분화보다 조명 조건을보다 강력하게 처리 할 수 ​​있습니다.

=================다른 보조 프로그램==================
## 샘플 수집
[Collection 폴더](/src/Collection)


해당 버튼을 눌러 해당 제스처의 파일 디렉토리를 캡처하십시오.
비디오 녹화는 비슷합니다

![Collection](/snapshot/Collection.png)

## 제스처 감지 라벨링 프로그램
[Label 폴더](/src/Label)


손이 그림에있는 사각형 상자를 표시하고 crop 폴더에 저장

![Label](/snapshot/Label.png)

## 특징점 주석
dlib의 imglab 도구로 주석 달기
손바닥을 예로 들어 봅시다.

![Palm_points](/snapshot/Palm_points.png)

## 탐지 모델 훈련

[Train_detection](/src/Train_detection)



## 특징점 회귀 모델 훈련

[Train_landmark](/src/Train_landmark)



## Reference

1. [dlib환경구성（이전에 작성된 블로그）](http://blog.csdn.net/zmdsjtu/article/details/53454071)

2. [OpenCV contribute 환경 구성（이전에 작성된 블로그）](http://blog.csdn.net/zmdsjtu/article/details/78069739)

3. Object detection with discriminatively trained partbased models. IEEE Trans. PAMI, 32(9):1627–1645, 2010.（제스처 현지화 및 인식，fHog기능）
4. One Millisecond Face Alignment with an Ensemble of Regression Trees by Vahid Kazemi and Josephine Sullivan, CVPR 2014.（특징 위치）
5. [Opencv3_contribute 알고리즘 패키지](https://www.learnopencv.com/object-tracking-using-opencv-cpp-python/)
