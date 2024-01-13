---
layout: single
title: "YOLOv8 Train"
categories: YOLOv8
tag: false 
toc: true
author_profile: false
sidebar:
  nav: "docs"
toc: true
--- 

# YOLOv8 Train 
오늘은 YOLOv8 Train에 대해서 알아보자.<br>
데이터를 가공했다면 전이학습이 필요하다. 좋은 모델일수록 좋은 학습이 필요한데 오늘은 이런 좋은 모델을 얻기위한 학습에 대해서 알아보도록 하자. Ultralytics에서 제공하는 Classify, Detect, Segment, Track, Pose 등 다양한 방법으로 트레인 할 수 있는 방법을 제공하고 있다. 

Google Colab을 사용해서 학습과정을 진행하였다.<br>

<div style="border: 1px solid #4CAF50; padding: 10px; background-color: #f2f2f2;">
  <a href="https://docs.ultralytics.com/tasks/">Ultralytics 공식 홈페이지</a>에서 확인할 수 있습니다.
</div><br>

<img width="600" alt="KakaoTalk_20240104_153645134" src="https://github.com/sangbeom1/sangbeom1.github.io/assets/145951445/ffd3b761-65b7-4725-9cc7-f85fd051e842"><br>

> 1.YOLOv8와 YOLOv8 실행에 필요한 라이브러리 설치
    !pip install ultralytics

    import ultralytics
    ultralytics.checks()

> 2.Downloading the Dataset
    !pip install roboflow

    from roboflow import Roboflow
    rf = Roboflow(api_key="iS9n0*********")
    project = rf.workspace("ai-e1r2s").project("driver-motion")
    dataset = project.version(1).download("yolov8")

> 3.Load a Pre-trained model
    from ultralytics import YOLO

    YOLO 모델 초기화
    model = YOLO('yolov8n.pt')
    model = YOLO('yolov8s.pt')
    model = YOLO('yolov8m.pt')
    model = YOLO('yolov8l.pt')
    model = YOLO('yolov8x.pt')

> 3-1 MS COCO Dataset에 정의되어 있는 클래스 개수와 종류 확인
    print(type(model.names), len(model.names))

    print(model.names)

> 4.Training
  1) data.yaml 파일 수정
      
      # 불러온 파일 중 yaml 파일을 확인
      !cat /content/Driver-motion-1/data.yaml

      # PyYAML 설치
      !pip install PyYAML

      # 필요한 module import
      import yaml

      data = {'train' : '/content/Driver-motion-1/train/images',
        'val' : '/content/Driver-motion-1/valid',
        'test' : '/content/Driver-motion-1/test/images',
        'names' : ['calling', 'smoking', 'texting'],
        'nc' : 3 }

      with open('/content/Driver-motion-1/data.yaml','w') as f:
        yaml.dump(data,f)

      with open('/content/Driver-motion-1/data.yaml', 'r') as f:
        Driver_yaml = yaml.safe_load(f)
        display(Driver_yaml)

  2) 불러온 pretraind-model인 yolo 모델을 기반으로, 학습 진행

      # yolov8m.pt 모델을 기반으로, 우리가 만든 data.yaml으로 학습 진행
model.train(data = '/content/Driver-motion-1/data.yaml',
            epochs=100,
            patience=25,
            batch=32,
            imgsz=500,
            project='/content/drive/MyDrive/result',
            name='sample',
            exist_ok=True)      

      # 학습된 결과물 출력
print(type(model.names),len(model.names))
print(model.names)      

      # 밑 코드는 model이 항상 yolov8n.pt로 되어있음
!yolo task=detect mode=train model=yolov8m.pt data='/content/bebeface-6/data.yaml' epochs=1 patience=12 batch=32 imgsz=640 project='/content/drive/MyDrive/KOSA3/project/result' name=sample exist_ok=True

# 참고 (Validate)
> mAP(Mean Average Precision) : 각 클래스에 대한 Average Precision을 평균내어 얻은 값으로, 모든 클래스에 대한 성능을 종합적으로 나타냄
- 높은 mAP는 모델이 각 클래스에 대해 높은 정밀도와 재현율을 갖고 있다는 것


> Box(Precision, Recall) : 각 클래스에 대한 박스 수준의 정밀도와 재현율
- 클래스 당 높은 박스 수준의 정밀도와 재현율은 모델이 해당 클래스를 얼마나 잘 감지하는지


> mAP50, mAP50-95 : IoU (Intersection over Union) 임계값을 다르게 한 경우의 mAP 값
- 높은 mAP50와 mAP50-95는 모델이 다양한 IoU 임계값에서 좋은 성능을 보이는 것을 나타냅니다.


> Precision, Recall : 모델의 정밀도와 재현율
- 높은 정밀도는 모델이 예측한 결과가 실제로 정답인 비율이 높다는 것
- 높은 재현율은 모든 실제 정답 중에서 모델이 얼마나 많은 것을 찾아냈는지를 나타냅니다.

    import locale
locale.getpreferredencoding = lambda: "UTF-8"

    # training 과정에서 평가 지표를 보고 싶으면, 스크롤을 올려서 표를 찾으면 되고,
# 따로 바로 보고싶으면 이 코드를 실행시키면 된다
    !yolo task=detect mode=val model=/content/drive/MyDrive/KOSA3/project2/result/sample/weights/best.pt data=/content/Driver-motion-1/data.yaml

> 5.Prediction

  1) 방법1 CLI
      
      !yolo task=detect mode=predict model=/content/drive/MyDrive/KOSA3/project2/result/sample/weights/best.pt conf=0.25 source=/content/drive/MyDrive/KOSA3/project/data/prediction save=True

  2) 방법2    
      model = YOLO("/content/drive/MyDrive/KOSA3/project2/result/sample/weights/best.pt")

      # source = '내가 테스트할 사진이나 영상 미리 넣어놓기'
        results = model.predict(source = "/content/drive/MyDrive/KOSA3/project/data/custom_test_img", save=True)

  3) 저장
    import shutil

# 복사할 폴더의 경로
    source_folder = "/content/runs/detect/predict3"

# Google Drive에 저장할 경로
    destination_folder = "/content/drive/MyDrive/KOSA3/project/prediction/predict1" # 이미존재하는 폴더일 때 여기 뒤 숫자 바꾸기

# shutil을 사용하여 폴더 복사
    shutil.copytree(source_folder, destination_folder)

    print(f"{source_folder} 폴더를 {destination_folder}로 복사 완료")

> 6.Export
    
    model = YOLO("/content/drive/MyDrive/KOSA3/project2/result/sample/weights/best.pt")

    model.export(format="onnx", imgsz=[480,640])


이러한 과정을 통해 모델의 결과를 얻을 수 있다. 
<img width="600" alt="KakaoTalk_20240104_153645134" src="https://github.com/sangbeom1/sangbeom1.github.io/assets/145951445/963ab220-adfa-4633-8b38-b6cab7380401"><br>