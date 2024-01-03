DMS Project-Driver Monitoring System

졸음감지 및 이상행동 감지 운전자 모니터링 시스템

최근 3년간 졸음, 주시 태만으로인한 연평균 사망자가 매년 8명정도 증가한다는 뉴스를 통해 모니터링 시스템의 필요성을 느꼈습니다.
그리고 2025년부터 유럽의 모든 차량에는 모니터링 솔루션 탑재가 법률화되어 국내에서 유럽으로 수출되는 차량에
는 DMS가 탑제되어야하고 그로 인해 발생하는 수익은 1.5조로 예상된다고합니다. 
또한 국내도 25년에 법안이 개정될 수 있다는 것을 확인 할 수 있었습니다.


<br>
# Demo 영상
# 클릭해서 영상 보기
[![main Demo](https://img.youtube.com/vi/VrDQqiV3zwU/0.jpg)](https://www.youtube-nocookie.com/embed/VrDQqiV3zwU)
<br>
[![Pose_Estimation, Eye-estimation](https://img.youtube.com/vi/6gZBNyvD2YM/0.jpg)](https://www.youtube.com/embed/6gZBNyvD2YM)
<br>
[![Face3D](https://img.youtube.com/vi/mPX-4aKvrRo/0.jpg)](https://www.youtube.com/embed/mPX-4aKvrRo) 



<br>
# 기능 소개 

프로젝트의 section은 총 3가지로 분류할 수 있습니다. 
1. 운전자 졸음감지
	- 1단계: 휴게소 및 쉼터 안내 음성서비스
	- 2단계: 전방주시 음성안내 및 이미지 캡처 
	- 3단계: 일정간격 사이렌 알림 

2. 운전자 이상행동감지 
	- 운전 중 통화 감지
	- 운전 중 문자 휴대폰 사용 감지
	- 운전 중 흠연 감지

3. 지관적인 시각화 
	- 1. Face3D
	- 2. 시선추적(Eye-estimation)
	- 3.  행동 추적(Pose_Estimation)

<br>
# 설계 과정

> 졸음 감지 Eye-Detection
- 아키텍처 설계 과정 및 단계별 알림 설정

Step 1
- 졸음 감지시스템 설계 과정 운전자의 졸음을 판단할 수 있도록 눈에 대한 Data Set을 활용하여 붓꽃(Iris) 회기 분류를 활용해 눈을 감을 때, 뜰 때 상황을 감지할 수 있도록 모델을 설계.

	-  FaceMesh를 활용하여 눈의 크기를 종 횡비로 계산하기 위해, 눈 영역을 검출 

	-  눈 영역을 Landmarks로 검출하여 Landmarks의 눈을 6개의 point로 지정 

	-  6개의 점을 선분을 만들어 비율을 계산하고 눈이 감겨있는 정도를 파악하여 종횡비를 계산

	-  운전자의 눈 크기의 표준사양을 고려하여 종횡비를 0.3 ~ 0.35 임계치 설정  
	
<img width="669" alt="main_drowsy" src="https://github.com/sangbeom1/sangbeom1.github.io/assets/145951445/817c14e7-41d4-4def-adc8-f0a0e689ac34">


<br>
Step 2 
- 졸음감지 요구사항 및 추가 사양 단계 별 알림 설정-사람이 졸 때 눈을 뜨는 시간보다 감는 시간이 길다는 점을 고려하여 아키텍처 설계

	- 1단계 : 눈 감음 +1, 눈 뜬 상태 -1 frame 으로 누적 10회 시 1차 알림 전방 5km 이내 있는 졸음 쉼터 및 휴게소 안내

	- 2단계 : 1단계 졸음 감지 후 계속해서 졸음 감지 시 +,- 누적되어 15회 달성 시 졸음 감지 음성 일림 치 캡쳐 기능

	- 3단계: 2단계 이 후 지속적으로 졸음 감지 시 3초 간격으로 사이렌 알림 제공
 
	* 직관적으로 시선을 추적하는 모습을 볼 수 있게 눈 안에 Draw 함수 추가
	* AI 제공 서비스 음성활용

![1](https://github.com/sangbeom1/sangbeom1.github.io/assets/145951445/a2f3eeed-feca-4f9b-a43b-197c3bae5d8f)

# Data Set 
- haarcascade_frontalface_alt.xml / haarcascade_frontalface_alt2.xml / shape_predictor_68_face_landmarks.dat


<br>
>이상 감지 Anomaly detection
- 다음으로 이상 행동 감지를 하기 위해서 YOLOv8 모델 활용. 

step 1 
- 데이터 셋<br>
	1.Roboflow에서 필요한 데이터 셋을 찾아 Annotate를 지정, class를 지정한 후 YOLOv8형식으로 만장 정도의 데이터 셋을 가공

<br>

Step 2
- 전이학습<br>
	1.YOLOv8 s, n, m, l, x 등 다양한 모델활용 

				Ex
				'metrics/precision(B)': 0.9766057367644005,    // 정밀도(정확도)
				'metrics/recall(B)': 0.9684228889682308,   // 재현율 : 높을 수록 놓치는게 낮다는 뜻
				'metrics/mAP50(B)': 0.9815056992918651,    // 평균 정밀도 : 50%이상인 경우 평균 정밀도
				'metrics/mAP50-95(B)': 0.7014963036097031,    / /mAP50을 더 엄하게 평가했을 경우의 정밀도, 탐지 정확도가 높은 모델일수록 높은 값을 가짐
				'fitness': 0.7314972431779194         // 모델의 전반적인 성능을 나타내는 단일 지표, 문제에 얼마나 적합한지를 평가

				mAP50이 높으면 좋은 모델 
				mAP50-95도 높으면 더욱 좋은 모델


	 CNN 예측(prediction)의 정확도(Accuracy)향상 

	<br>
		method 1

				EX
				model.train(data = '/content/bebeface-4/data.yaml',
				epochs=1,
				patience=30,
				batch=32,
				imgsz=640,
				project='/content/drive/MyDrive/KOSA3/project/result',
				name='sample',
				exist_ok=True)


	- 기능설명
		data: 데이터 구성을 정의하는 YAML 파일의 경로
		epochs: 전체 데이터셋에 대한 학습 반복 횟수
		patience: 조기 종료를 적용할 때의 기다리는 에폭 수. 즉, 검증 손실이 개선되지 않은 상태에서 몇 번의 에폭을 더 학습할 것인지를 결정.
		*[케라스] 딥러닝 모델 학습 조기 종료시키기(early stopping) 동일 효과*
		batch: 각 미니배치의 크기.
		imgsz: 입력 이미지의 크기.
		project: 학습 중 생성되는 파일들이 저장될 프로젝트 디렉토리의 경로.
		name: 학습 중 생성되는 파일들의 이름(prefix).
		exist_ok: 지정된 프로젝트 및 이름의 디렉토리가 이미 존재할 경우에도 오류를 발생시키지 않고 계속 진행할지 여부를 나타냄.

	<br>
		 method 2 
	
			1. 데이터 증강
			Augmentation settings
			augment:
				flipud: 0.0  # flipud (상하 반전)
				fliplr: 0.5  # fliplr (좌우 반전)
			mosaic: 1.0  # mosaic (모자이크 증강)
				mixup: 0.2  # mixup (믹스업 증강)  mixup: 0.2  # mixup augmentation

			2. Pytorch 데이터 증강 코드
			transform = transforms.Compose([
			transforms.RandomResizedCrop(640),
			transforms.RandomHorizontalFlip(),
			transforms.ColorJitter(brightness=0.2, contrast=0.2, saturation=0.2, hue=0.2),
				transforms.ToTensor(),
			])

<br>
<br>
Step 3
- 모델 적용 
	- 통화중 : YOLOv8n 모델 활용, 학습 된 데이터 셋을 Ultralytics YOLO로 Detection하여 처음 감지 임계치 0.65를 시작으로 0.4 이하까 지 임계치를 측정하여 핸드폰 통화 감지

	- 문자중 : YOLOv8n 모델 활용, 학습 된 데이터 셋을 Ultralytics YOLO로 Detection하여 처음 감지 임계치 0.65를 시작으로 0.4 이하까 지 임계치를 측정하여 핸드폰 문자, 휴대폰 사용 감지

	- 흡연 : YOLOv8n 모델 활용, 학습 된 데이터 셋을 Ultralytics YOLO로 Detection하여 처음 감지 임계치 0.65를 시작으로 0.4 이하까 지 임계치를 측정하여 흡연 감지

	  **3초 동안 행동 유지시 안내 음성 제공**
<img width="669" alt="main_yolo" src="https://github.com/sangbeom1/sangbeom1.github.io/assets/145951445/646b2caa-8940-4be5-ac8e-64e7d07eb96f">
	



<br>
>얼굴 탐지 Face Mesh

- Mediapipe를 활용하여 얼굴 메쉬를 탐지 

- FaceMesh를 통해 눈 종속성을 지정 종횡비를 계산하여 트리거 생성

- Face3D로 구현  

![3](https://github.com/sangbeom1/sangbeom1.github.io/assets/145951445/fd98c048-e0d8-49cf-b749-7e8aff54ab8c)



<br>
# 향후 발전 가능성 
>Face 3D
- FaceMesh를 활용하여 향 후 운전자의 모습을 다양한 기능의 형태로 보여질 수 있도록 서비스를 제공하며 웹캠에서 실시간으로 얼굴을 감지하고, 직관적으로 시각화 할 수 있도록 적용 예정 
![4](https://github.com/sangbeom1/sangbeom1.github.io/assets/145951445/da4b5b14-9aec-452f-914e-04f5f11128f9)

<br>
>Pose_Estimation, Eye-estimation
- 운전자의 주의력 상태, 머리 자세와 각도, 눈 종횡비, 시선, 눈 감음 지속 시간, 정밀한 휴대폰 및 행동 감지를 추적할 수 있도록 모델에 적용 예정
![5](https://github.com/sangbeom1/sangbeom1.github.io/assets/145951445/04f34a23-533d-4dfd-a774-42a39a144c8a)












