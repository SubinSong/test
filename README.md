# Requirements
- numpy == 1.18.1
- pandas == 1.0.1
- Keras == 2.3.1
- tensorflow == 1.15.0

# Attribute classifier
패션 이미지 데이터셋에 존재하는 각각의 속성 레이블에 대한 분류기임
각 레이블에 따른 분류기는 아래와 같음

- isfashion: **패션이미지인지 아닌 지**에 대하여 학습하고 분류함(이진 분류)
  - 패션이미지가 아닌 경우는 아래와 같이 여러가지 경우가 존재함
  - 패션스타일이 보이는 사진이기는 하나 외부 조형물이나 이미지 위 디자인으로 오염된 경우
  - 풍경, 동물 사진과 같이 패션이미지가 아닌 경우(e.g. 아래 이미지에서 강아지 사진의 경우)
  - **패션아이템**만 존재하여 스타일을 파악할 수 없는 경우(e.g. 아래 이미지에서 가방과 같은 경우)
 
- baseColour: **46개의 색상**에 대하여 학습하고 분류함
  - Black
  - Khaki
  - Olive
  - ... 

- gender: **5개의 성별**에 대하여 학습하고 분류함
  - Women
  - Men
  - Boys
  - Girls
  - Unisex
  
- season: **4개의 계절감**에 대하여 학습하고 분류함
  - Spring
  - Summer
  - Fall
  - Winter

- shape-related: **68개의 형태**에 대하여 학습하고 분류함
  - square
  - a-line
  - midi
  - ...


# Framework

  ### step 1. Data Preparation
 1. 개별 이미지의 파일 이름과 레이블 정보가 담긴 csv 파일
 2. 속성의 이름으로 된 폴더마다 하위 폴더로 train, validation, test로 이미지가 구성되어 있음
 
  ### step 2. Data preprocessing
  데이터 전처리 및 모델 업로드 등을 진행합니다. 
  ```bash
  python preprocessing.py
  ```

  ### step 3. training model & evaluation
 학습데이터로 모델을 학습하고 평가합니다. 
 ```bash
 python training_model.py
  ```
  
  ### step 4. testing 
  테스트데이터로 모델을 평가합니다. 
  ```bash
  python testing.py
  ```
  
# 실행법
### step 1. resource/config/test.yaml 파일에서 파라미터 변경
 - local 환경에 맞게 dataset_path 변경
 - 'category' 변수에 attribute 이름 지정(e.g. 'gender')
 - 'nb_class' 변수에 위에서 지정한 attribute의 개수 입력(e.g. **'gender'일 경우 5**)
 - 'pretraining_model', 'activation_function' 등 모델 관련 파라미터는 각주에 있는 항목으로 변경 가능

### step 2. 모델 학습 실행
 - run/training_model.py 파일 실행
 
### step 3. 학습된 모델로 테스트 진행
 - pretrain_model_run.py 파일 실행
 
