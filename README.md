# Remind-learning-schedule-with-ML

## 프로젝트 목차
- [프로젝트 배경](#프로젝트-배경)
- [문제 설정](#문제-설정)
- [분석 목표](#분석-목표)
- [주요 지표 소개](#주요-지표-소개)
- [가설 설정](#가설-설정)
- [모델링 결과](#모델링-결과)
- [결과 시각화 및 해석](#결과-시각화-및-해석)
- [결론](#결론)
- [제안](#제안)
- [한계 지점](#한계-지점)
- [데이터 출처](#데이터-출처)

## 프로젝트 배경

- 오픈 유니버시티는 국공립 대학으로 예산을 받기 위해서는 학생의 수료율을 높여야 함
- 오픈 유니버시티에 다니는 학생들은 대부분은 직업이 있기 때문에 학업에 온전히 집중하기 어려운 상황임
- 수료율이 낮을 것으로 예상되는 사람들에게 학습 리마인드 알람 서비스를 제공하여 학습을 독려하고자 함


## 문제 설정

- 리마인드 알람을 보낼 주기와 기준을 설정해야함


## 분석 목표

- 학생 데이터를 기반으로 학생의 수료 여부를 예측하기
- 수료를 예측하는데 중요한 특성을 순열 중요도를 통해 도출하기
- 특성과 수료율의 관계를 PDP를 통해 시각화하여 수료율이 낮아지는(혹은 높아지는) 시점 찾아내기


## 주요 지표 소개

- 학생 별 평균 수업 참여율 : 학생 별 수업 총 참여일 / 과목 별 수업일
- 평가물 지각제출율 : 학생 별 지각 제출 횟수 / 총 과제 수
- 수업 화면 평균 클릭 수 : 학생 별 수업 화면 총 클릭 횟수 / 총 수업 화면(site) 수
- 수료 여부 : 타겟값


## 가설 설정

- 귀무가설 : 학습 화면의 Click률, 학습 화면 평균 체류일, 과제 제출비율 **세 변수와 수료 여부는 관계가 없다.**
- 대립가설 : 학습 화면의 Click률, 학습 화면 평균 체류일, 과제 제출비율 **세 변수와 수료 여부는 관계가 있다.**


## 모델링 결과

<img width="884" alt="Screen Shot 2022-04-28 at 9 23 07 PM" src="https://user-images.githubusercontent.com/93904398/165751211-b1919fb3-fac5-4433-b968-023bf7a31708.png">

- 테스트한 모델 모두 베이스라인(0.68) 이상의 성능을 보임
- 네 개의 모델 중 일반화 성능이 뛰어나다고 알려진 RandomForest(adjust) 모델을 분석에 사용함

## 결과 시각화 및 해석

### 1. 순열 중요도

<img width="555" alt="Screen Shot 2022-04-28 at 9 26 05 PM" src="https://user-images.githubusercontent.com/93904398/165751566-4b7c76d8-da78-40d4-b088-b09803d147ab.png">

- 타겟을 예측하는데 1) 학생 별 평균 수업 참여율 2) 평가물 지각제출율 3) 수업 화면 평균 클릭 수의 중요도가 높게 나타남

### 2. 학생 별 평균 수업 참여율

<img width="789" alt="Screen Shot 2022-04-28 at 9 29 42 PM" src="https://user-images.githubusercontent.com/93904398/165752152-8c9b67c7-fda0-4197-a849-681374c1f813.png">

- 평균 수업 참여율 0.1부터 수료율이 상승하고 0.4 부근에서 정점을 이룸
- 수업을 듣기만 해도 수료 확률이 높아짐을 알 수 있음

### 3. 수업 화면 평균 클릭수

<img width="797" alt="Screen Shot 2022-04-28 at 9 31 24 PM" src="https://user-images.githubusercontent.com/93904398/165752458-d09b61a9-d580-4971-bc23-b9c77e75c415.png">

- 평균 클릭 수 3회까지 수료율 상승
- 4회 이상부터 수료율 하락
- 이수에 필요한 적정한 수의 Click 수가 있다고 생각할 수 있으나, 4회 이상 클릭한 데이터가 적어서 PDP의 지표가 하락하는 것일 수 있음

### 4. 평가물 지각제출 비율

<img width="785" alt="Screen Shot 2022-04-28 at 9 39 05 PM" src="https://user-images.githubusercontent.com/93904398/165753760-2275e5be-fe52-496d-bca9-2c013cc5343f.png">

- 지각제출 비율이 높아질 수록 수료확률이 낮아짐


## 결론

- 평균 수업 참여율이 0.4 이하일 때 수료율이 낮아짐
- 평균 클릭 수가 1~4회를 벗어날 때 수료율이 낮아짐
- 평가물 제출 기한이 지났을 때 수료율이 낮아짐


## 제안

- 평균 수업 참요율이 0.4 이하로 내려갈 때 푸쉬알람을 보낼 것을 제안함
- 평균 클릭수가 1~4회를 벗어나는 것은 화면에서 적정한 답을 찾지 못하는 것으로 해석할 수 있음. 해당 학생에게 선수 과목을 추천할 것을 제안함
- 평가물 제출 기한에 임박했을 때 제출일을 상기시키는 푸쉬알람을 보낼 것을 제안함

## 한계 지점

- 해당 분석을 통해 리마인드 알람 발송 기준은 알 수 있었으나, 발송 주기는 알 수 없었음
- 기준 이하로 떨어질 때마다 리마인드 알람을 보내게 되면 학생들이 피로감을 느낄 가능성이 높으므로 적절한 주기를 찾아내야 함
- 향후 AB 테스트를 통해서 적절한 주기를 찾을 것을 제안함

## 데이터 출처

- https://www.kaggle.com/datasets/anlgrbz/student-demographics-online-education-dataoulad
