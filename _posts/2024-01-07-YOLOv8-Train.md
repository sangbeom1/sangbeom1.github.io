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
데이터를 가공했다면 전이학습이 필요하다. 좋은 모델일수록 좋은 학습이 필요한데 오늘은 이런 좋은 모들을 얻기위한 학습에 대해서 알아보도록 하자.<br>


# YOLOv8와 YOLOv8 실행에 필요한 라이브러리 설치
!pip install ultralytics


import ultralytics
ultralytics.checks()
