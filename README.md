# 📱 스마트폰 센서 데이터를 활용한 인간 활동 분류(HAR) 및 머신러닝 모델 비교 분석

> [cite_start]**데이터마이닝(C) 텀프로젝트** [cite: 6, 7]
> [cite_start]* 팀원: 윤준영, 김용우, 유민혁 [cite: 7]
> * 프로젝트 수행 기간: 2026.06 [cite: 7]

---

## 1. 프로젝트 개요 (Overview)
[cite_start]현대 스마트폰에 기본 내장된 가속도계(Accelerometer)와 자이로스코프(Gyroscope) 센서 데이터를 활용하여 [cite: 10][cite_start], 사용자의 6가지 일상 행동(걷기, 계단 오르기/내리기, 앉기, 서기, 눕기)을 정밀하게 분류하는 머신러닝 파이프라인 연구입니다[cite: 22, 23, 25]. 

[cite_start]단순히 높은 정확도를 내는 것에 그치지 않고, **비지도학습(K-Means)의 구조적 한계를 지도학습(K-NN, Random Forest) 모델을 통해 계단식으로 극복해 나가는 공학적 분석 과정**을 증명하였습니다[cite: 224, 455].

## 2. 데이터셋 정보 (Dataset)
* [cite_start]**출처:** UCI Machine Learning Repository - Human Activity Recognition Using Smartphones 
* [cite_start]**규모:** 전체 10,299개 샘플, 561개 독립 변수(Feature), 1개 종속 변수(Target) 
* [cite_start]**특징:** `StandardScaler`를 통해 센서 간 물리적 단위를 표준화하고, 클래스 균일 분포를 EDA로 확인하였습니다[cite: 59, 68].

## 3. 핵심 분석 및 모델링 흐름 (Key Pipeline)

### 3.1. PCA를 통한 차원 축소 및 인사이트 도출
* [cite_start]561차원의 고차원 변수가 가진 다중공선성과 '차원의 저주'를 해결하기 위해 PCA를 적용하여 2차원으로 투영하였습니다[cite: 71, 73, 74].
* [cite_start]**인사이트:** 정답 라벨이 없음에도 신호의 변동성에 따라 **동적 활동(좌측)과 정적 활동(우측)이 완벽히 격리되는 기하학적 패턴**을 확인했습니다[cite: 107, 108]. [cite_start]반면 '앉기'와 '서기'는 공간 상에서 격렬하게 중첩되어 있어 단일 거리 기반 알고리즘의 취약점이 될 것임을 선제적으로 파악했습니다[cite: 110, 111].

### 3.2. K-Means 군집화와 비지도학습의 한계 ($K=2$ vs $K=6$)
* [cite_start]Elbow Method를 가동하여 최적의 오차 감소 효율 지점인 $K=2$를 도출하고 군집화를 수행했습니다[cite: 127, 151, 153].
* [cite_start]**한계점 도출:** 실제 활동 수인 $K=6$으로 확장했을 때, 공간 상에 겹쳐 있는 '앉기'와 '서 있기' 데이터를 분리하지 못하고 무작위로 파편화되는 비지도학습의 구조적 한계를 오차 교차표(Cross Tabulation)로 증명했습니다[cite: 218, 221, 222].

### 3.3. 지도학습 도입 및 하이퍼파라미터 최적화 (K-NN vs Random Forest)
* [cite_start]**K-NN (Accuracy: 96%):** 이웃 수 $K=7$로 최적화 튜닝하여 6가지 세부 행동 분류를 정밀화했으나, 결정 경계면에서의 다수결 오류로 인해 앉기/서기 간 오분류 총 100건이 잔존했습니다[cite: 256, 270, 335, 336, 444].
* [cite_start]**Random Forest (Accuracy: 98%):** 트리 개수(`n_estimators=100`) 튜닝을 통해 변수별 중요도를 기반으로 세밀한 분기를 수행하여, K-NN의 취약점이었던 **앉기/서기 오차율을 50건으로 대폭 완화(50% 감소)시키며 최고 성능을 기록**했습니다[cite: 390, 397, 445].

## 4. 최종 결과 요약 (Result)

| 알고리즘 | 개선사항 | 한계점 |
| :---: | :--- | :--- |
| **K-Means** | [cite_start]동적/정적 데이터 및 눕기 라벨의 정확한 분리 [cite: 456] | [cite_start]앉기/서기 구분 불가능, 걷기 계열 구분 모호 [cite: 456] |
| **K-NN** | [cite_start]6가지 행동 라벨 96% 정확도 분류 [cite: 456] | [cite_start]앉기/서기 라벨 간 오분류 오차 총 100건 발생 [cite: 456] |
| **Random Forest** | [cite_start]**6가지 행동 라벨 98% 정확도 분류 (최우수)** [cite: 456, 459][cite_start], 앉기/서기 오차 대폭 하강 (50건) [cite: 445, 456] | [cite_start]물리적 사각지대로 인한 미세 오차 잔존 [cite: 456, 477] |

## 5. 팀원별 역할 분담 (Role Distribution)
* [cite_start]**윤준영 :** 프로젝트 개요 및 서론 기술, 데이터 수집·정제, StandardScaler 표준화 및 PCA 차원 축소 파이프라인 구축, K-Means 군집화 모델링 및 Elbow Method 분석[cite: 503].
* [cite_start]**김용우 :** 타겟 레이블 분포 탐색(EDA), K-NN 분류 모델링 및 하이퍼파라미터($k$) 최적화 튜닝 민감도 분석[cite: 503].
* [cite_start]**유민혁 :** Random Forest 앙상블 모델링 및 트리 개수 최적화, Confusion Matrix 기반 오분류 오차 심층 해석, 핵심 인사이트 및 한계점 도출[cite: 503].
