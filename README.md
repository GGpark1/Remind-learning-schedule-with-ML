# Open-University-Learning-Analytics

## 프로젝트 목차
- [프로젝트 배경](#프로젝트-배경)
- [문제 설정](#문제-설정)
- [분석 목표](#분석-목표)
- [가설 설정](#가설-설정)
- [주요 지표 소개](#주요-지표-소개)
- [모델링 결과](#모델링-결과)
- [결과 시각화 및 해석](#결과-시각화-및-해석)
- [결론](#결론)
- [레퍼런스](#레퍼런스)

## 프로젝트 배경

- 오픈 유니버시티는 국공립 대학으로 예산을 받기 위해서는 등록생의 수료율을 높여야 함
- 오픈 유니버시티에 다니는 학생들은 대부분은 직업이 있기 때문에 학업에 온전히 집중하기 어려운 상황임
- 교육의 수준을 낮춰서 수료율을 높이면 대학으로서의 위상에 문제가 생기며, 양질의 교육을 원하는 등록생의 니즈를 충족하기 어려워짐


## 문제 설정

- 현재 교육 수준을 유지하면서 수료율을 높이기 위한 방법을 찾아야 함
- 이 문제를 풀기 위해 대학은 수료율이 낮을 것으로 예상되는 사람들에게 학습 진도율 알림/선수과정 추천 등의 학습 도우미 서비스를 제공하려고 함
- 학습 도우미로 등록생들에게 remind하려는 주기, 기준을 설정해야함


## 분석 목표

- 학생 데이터를 기반으로 학생의 수료 여부를 예측하기
- 어떤 요인, 어느 시점에 수료율을 낮추는지 확인하기
- 학교가 개입할 수 있는 기준(시점) 만들기


## 가설 설정

- 귀무가설 : 학습 화면의 Click률, 학습 화면 평균 체류일, 과제 제출비율 **세 변수와 수료율은 관계가 없다.**
- 대립가설 : 학습 화면의 Click률, 학습 화면 평균 체류일, 과제 제출비율 **세 변수와 수료율은 관계가 있다.**


## 주요 지표 소개

- 학생 별 평균 수업 참여율 : 학생 별 수업 총 참여일 / 과목 별 수업일
- 평가물 지각제출율 : 학생 별 지각 제출 횟수 / 총 과제 수
- 수업 화면 평균 클릭 수 : 학생 별 수업 화면 총 클릭 횟수 / 총 수업 화면(site) 수

## 모델링 결과

<img width="884" alt="Screen Shot 2022-04-28 at 9 23 07 PM" src="https://user-images.githubusercontent.com/93904398/165751211-b1919fb3-fac5-4433-b968-023bf7a31708.png">

- 테스트한 모델 모두 베이스라인(0.68) 이상의 성능을 보임
- 네 개의 모델 중 테스트 데이터와 학습 데이터의 차이가 가장 적은 RandomForest(adjust) 모델을 분석에 사용함

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
- 이수에 필요한 적정한 수의 Click 수가 있다고 생각할 수 있으나, 4회 이상 클릭한 데이터가 적어서 PDP의 지표가 하락하는 것일 수 있음  -> 해석에 주의 필요

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

## 레퍼런스

- https://www.kaggle.com/datasets/anlgrbz/student-demographics-online-education-dataoulad
