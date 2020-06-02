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
 ![isfashion](https://user-images.githubusercontent.com/58968614/83541928-3dfc0e00-a535-11ea-9979-6ece8c3cc22a.png)


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

  ### step 0. Data Preparation
 1. 개별 이미지의 파일 이름과 레이블 정보가 담긴 csv 파일
 
     |id|gender|masterCategory|subCategory|articleType|baseColour|season|year|usage|productDisplayName
     |------|---|---|---|---|---|---|---|---|---|
     |15970|Men|Apparel|Topwear|Shirts|Navy Blue|Fall|2011|Casual|Turtle Check Men Navy Blue Shirt
     |39386|Men|Apparel|Bottomwear|Jeans|Blue|Summer|2012	|Casual|Peter England Men Party Blue Jeans
     |59263|Women|Accessories|Watches|Watches|Silver|Winter|2016|Casual|Titan|WomenSilver Watch
     |...|...|...|...|...|...|...|...|...|...|
 
 2. 속성의 이름으로 된 폴더마다 하위 폴더 train, validation, test로 이미지가 구성되어 있음
       ```
      ├── season (folder)
      │      
      │  ├──── train (folder)
      │       │ imagefile01.jpg
      │       │ imagefile02.jpg
      │       │ ...
      │ 
      │  ├──── validation (folder)
      │       │ imagefile101.jpg
      │       │ imagefile102.jpg
      │       │ ...
      │
      └──├───── test (folder)
               │ imagefile1001.jpg
               │ imagefile1002.jpg
               │ ...
      
      
      ```
  
  ### step 1. 실행 환경 세팅
  - resource/config/test.yaml 파일에서 파라미터 변경
  - local 환경에 맞게 dataset_path 변경
  - 'category' 변수에 attribute 이름 지정(e.g. 'gender')
  - 'nb_class' 변수에 위에서 지정한 attribute의 개수 입력(e.g. **'gender'일 경우 5**)
  - 'pretraining_model', 'activation_function' 등 모델 관련 파라미터는 각주에 있는 항목으로 변경 가능
  
   ![image](https://user-images.githubusercontent.com/58968614/83546433-9f26e000-a53b-11ea-808d-89d8d43d0c60.png)

 
  ### step 2. Data preprocessing
  데이터 전처리 및 모델 업로드 등을 진행합니다. 
  ```bash
  python preprocessing.py
  ```

  ### step 3. 실행
  - 모델을 직접 학습할 경우: 아래 training_model.py을 실행하여 모델을 학습시키고, 결과를 확인
  ```bash
  python training_model.py
  Ready to config
  Found 2439 images belonging to 4 classes.
  Found 631 images belonging to 4 classes.
  Found 631 images belonging to 4 classes.

  Model: "model_1"
  __________________________________________________________________________________________________
  Layer (type)                    Output Shape         Param #     Connected to                     
  ==================================================================================================
  input_1 (InputLayer)            (None, None, None, 3 0                                            
  __________________________________________________________________________________________________
  conv1_pad (ZeroPadding2D)       (None, None, None, 3 0           input_1[0][0]                    
  __________________________________________________________________________________________________
  conv1 (Conv2D)                  (None, None, None, 6 9472        conv1_pad[0][0]                  
  __________________________________________________________________________________________________
  bn_conv1 (BatchNormalization)   (None, None, None, 6 256         conv1[0][0]                      
  __________________________________________________________________________________________________
  activation_1 (Activation)       (None, None, None, 6 0           bn_conv1[0][0]                   
  __________________________________________________________________________________________________
  pool1_pad (ZeroPadding2D)       (None, None, None, 6 0           activation_1[0][0]               
  __________________________________________________________________________________________________
  max_pooling2d_1 (MaxPooling2D)  (None, None, None, 6 0           pool1_pad[0][0]                  
  __________________________________________________________________________________________________
  ...
  __________________________________________________________________________________________________
  dense_2 (Dense)                 (None, 4)            260         dense_1[0][0]                    
  ==================================================================================================
  Total params: 23,719,108
  Trainable params: 8,012,612
  Non-trainable params: 15,706,496
  __________________________________________________________________________________________________

  Epoch 1/10

   1/38 [..............................] - ETA: 31:57 - loss: 1.5204 - top1_acc: 0.2812 - top2_acc: 0.6094 - top3_acc: 0.7812
   2/38 [>.............................] - ETA: 27:13 - loss: 1.5268 - top1_acc: 0.2891 - top2_acc: 0.6094 - top3_acc: 0.8516
   3/38 [=>............................] - ETA: 25:16 - loss: 1.4849 - top1_acc: 0.3229 - top2_acc: 0.6458 - top3_acc: 0.8854
   4/38 [==>...........................] - ETA: 23:48 - loss: 1.4654 - top1_acc: 0.3359 - top2_acc: 0.6680 - top3_acc: 0.9023

   ...


   ```
  
 
  - 학습 후 저장된 모델을 불러와서 테스트할 경우: 아래 pretrain_model_run.py을 실행하여 결과를 확인
   ```bash
  python pretrain_model_run.py
  
   - 로그 예시
  
     Loss: 1.14746811132240788
     Top1 acc: 0.7718446601941747
     Top2 acc: 0.9422785194174758
     Top3 acc: 0.9839199029126213
     
                    precision    recall  f1-score   support

                 0       0.74      0.67      0.70       3453
                 1       0.90      0.79      0.84       1824
                 2       0.78      0.83      0.80       6453
                 3       0.76      0.77      0.77       2454

         micro avg       0.77      0.77      0.77       13184
         macro avg       0.80      0.76      0.78       13184
      weighted avg       0.77      0.77      0.77       13184
       samples avg       0.77      0.77      0.77       13184

      [[ 2298   20  972  163]
       [ 33   647  113  31]
       [ 653   43  5367  390]
       [ 116   12 430  1896]]
       
      Evaluation time: 0:07:54.020724

      Process finished with exit code 0
 
  ```

# Experiment result

   |attribute|Top1|Top2|Top3|
   |------|---|---|---|
   |baseColour|0.804871|0.923988|0.960477|
   |gender|0.893780|0.971078|0.993539|
   |season|0.771844|0.942278|0.983919|
   
  
