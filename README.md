# 중고서적 판매를 위한 등급 분류 프로젝트
### NIPA_Google_ML_3기 2팀

## AI/Computer Vision 기반 중고서적 상태 분석 및 챗봇 서비스 
### 목표 
- 알라딘의 중고 서적 매입 시스템을 개선하여, 사용자 경험을 극대화하고, 자동화된 서적 상태 평가 시스템을 도입해 효율성을 높임

### 주요 기능
- AI 이미지 분석: Object Detection/Instance Segmentation 모델을 활용한 중고서적의 상태 자동 판정
- 알라딘 API 연동: ISBN 코드 입력을 받아 책 정도 제공
- AI 챗봇 서비스: Gemini 챗봇 API를 통한 실시간 사용자 응대

### 타겟
- 알라딘 (기업), 특히 중고서적 매입 시스템을 개선하고자 하는 알라딘의 사업 부서
---
### 프로젝트 과정 요약 및 성능 개선 노력
#### 1. Object Detection vs. Instance Segmentation 모델 비교 
- 배경: 손상 감지에서 Objectv Detection과 Instance Segmentation 모델의 성능을 비교함
- 결과: Instance Segmentation 모델이 Object Detection 보다 높은 mAP를 기록하며 더 효과적임을 확인

#### 2. 라벨링 클래스 세분화: 5개 vs. 9개 클래스 
- 배경: 원래의 5개 클래스(ripped, wet, stain, wear, folded)에서 9개의 클래스(front_wear, front_ripped, back_ripped, back_wear, side_ripped, side_wear)로 세분화
- 세분화 방식: 손상 위치에 따라 front, back, side로 구분하고 비교적 뚜렷한 특성이 존재하지 않는 손상 유형(ripped, wear)을 세분화
- 결과: 세분화된 라벨링을 통해 mAP가 향상됨.
- 결론: 손상 위치와 유형을 세부적으로 분류하는 것이 손상 감지 성능 향상에 기여함을 확인.

#### 3. Data Augmentation 적용 여부 
- 배경: 학습 데이터에 Data Augmentation을 적용한 경우와 아닌 경우를 비교
- 결과: Augmentation 적용 시 mAP가 높아져, 데이터 다양성이 성능에 긍정적 영향을 미침
- 결론: Augmentation 적용이 유리함을 확인하여 다양한 Augmentation 적용

#### 4. 모델 크기 비교: YOLOv11 nano vs. small vs. large
- 배경: 동일한 데이터셋에서 YOLOv11 모델의 nano, small, large 버전을 비교하여 각 버전의 성능을 평가
- 결과: 모델 크기에 따라 mAP가 미세하게 다르게 나타남. 그 중 small 버전이 가장 좋았음
- 결론: 성능과 리소스 효율성을 고려해 최적의 모델 크기 선택

#### 5. YOLO 모델의 한계 인식 및 Faster R-CNN과 Mssk R-CNN 적용
- 배경: YOLO 모델에서의 성능 한계를 느껴 2-stage 모델인 Faster R-CNN과 Mask R-CNN을 실험적으로 적용
- 결론: mAP가 크게 향상되었으나, 2-stage detection 방식의 특성상 처리 시간이 길어짐
- 결과: 실시간 처리가 필요한 챗봇 서비스에는 적용이 어려워, mAP와 속도 균형이 필요한 상황에서는 사용을 지양함
---
### 개선사항 및 결론
#### 1. 데이터셋 확장 및 모델 개선
- 현재 직접 촬영한 1000장의 이미지로 트레이닝해서 mAP가 높지 않음. 이후 데이터셋을 추가하여 성능 향상 목표
- 추가적인 데이터 전처리 및 증강 기법 도입으로 YOLO 모델 최적화 필요

#### 2. 프리미엄 알라딘 API 도입
- 실시간 판매 시세 정보를 위해 알라딘 프리미엄 API 도입 필요
- 실시간 매입 가능 여부 확인 및 보다 정확한 가격 추정 기능 추가

#### 3. 모델 선택과 실시간 서비스의 한계 
- 실시간 서비스 구현을 위해 YOLO 모델 사용 중이나 Faster R-CNN이 더 높은 mAP 성능을 보임
- 향후 더 나은 서버 환경에서 Faster R-CNN 도입 예정

#### 4. 데이터베이스 연동
- 현재 데이터베이스 미연동 상태로, 향후 연동 시 다양한 추가 정보 제공 가능
- 서적 상태와 가격 정보를 실시간으로 받아 신뢰성 및 편의성 향상 기대 
