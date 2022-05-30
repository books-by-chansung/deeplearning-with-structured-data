# 딥러닝을 이용한 정형 데이터 분석
매닝 출판사의 원본 저장소 [**Deep Learning with Structured Data**](https://www.manning.com/books/deep-learning-with-structured-data)를 약간 개정한 파일을 제공하는 저장소입니다. 또한 한국어판 출판에 따라 번역된 노트북도 함께 제공합니다.

## 알아둘 사항
이 저장소는 [원본 저장소](https://github.com/ryanmark1867/manning)를 약간 고친것으로, 다음과 같은 개정 사항을 포함하고 있습니다. _참고로 원래 이 책의 시작은 TensorFlow 1.x 였기 때문에, 아래 개선 사항의 대부분은 이미 종이 책에 적용되어 있습니다_.
- 좀 더 합리적인 파일 이름
- 좀 더 간소화된 디렉터리 구조
- 무료 환경이 제공하는 파이썬 3.6과 3.7에서 검증된 노트북
- 하드코딩된 파라미터를 제거하기 위한 설정 파일
- 좀 더 쉽게 실습할 수 있도록 리팩터링된 코드
- 모델의 학습과 배포를 위해 TensorFlow 2.0을 사용

## 디렉터리 구조
- **data**: 전처리된 데이터셋과 중간 결과로 얻은 데이터셋을 저장한 피클 파일
- **deploy**: 8장의 학습된 모델을 Ras와 페이스북 메신저로 배포하기 위한 코드
- **deploy_web**: 8장의 학습된 모델을 Flask로 웹 배포하기 위한 코드
- **models**: 저장된 학습된 모델
- **notebooks**: **streetcar_data_preparation.ipynb** (데이터 준비용), **streetcar_model_training.ipynb** (딥러닝 모델 학습용), **streetcar_model_training_xgb.ipynb** (XGBoost 모델 학습용) 주피터 노트북과 각 노트북에서 쓰이는 설정 및 클래스 파일
- **pipelines**: **streetcar_model_training.ipynb**에 의해 생성된, 피클로 저장된 파이프라인 파일. 학습된 모델과 함께 배포에 사용되는 파일
- **sql**: 2장의 간단한 SQL 예제에서 사용된 테이블을 생성하는 SQL

## 이 저장소의 코드를 실습하는 방법
- **데이터 준비** - 딥러닝 모델을 학습시키기 위해 입력 데이터를 정리하는 단계. 이 단계의 출력은 정리된 데이터셋을 담은 피클된 데이터프레임 입니다.

  1. 입력 데이터(만약 `load_from_scratch = False` 이면 `pickled_input_dataframe`을, `load_from_scratch = True` 이면 `https://open.toronto.ca/dataset/ttc-streetcar-delay-data/` 에서 다운로드한 엑셀 파일을 **data** 디렉터리로 복사)와 출력 데이터프레임 이름을 지정하기 위해 **notebooks/streetcar_data_preparation_config.yml** 파일 내용을 수정합니다.

  2. **notebooks/streetcar_data_preparation.ipynb** 를 실행합니다.

- **딥러닝 모델 학습** - 앞 단계에서 정리된 데이터셋으로 딥러닝 모델을 학습시키는 단계. 이 단계의 출력은 학습된 모델(h5 형식의 파일)과 파이프라인에 대한 두 피클 파일 입니다.
  1. 입력 데이터프레임(pickled_dataframe)을 지정하기 위해 **notebooks/streetcar_model_training_config.yml** 파일 내용을 수정합니다. **데이터 준비** 단계의 출력으로 얻은 데이터프레임 파일 이름으로 지정되어야 합니다.
  2. 텐서플로 2.0이 설치된 환경에서 **notebooks/streetcar_model_training.ipynb** 를 실행합니다.

- **XGBoost 모델 학습** - **데이터 준비** 단계에서 정리된 데이터셋으로 XGBoost 모델을 학습시키는 단계. 이 단계의 출력은 학습된 모델(h5 형식의 파일)과 파이프라인에 대한 두 피클 파일 입니다.
  1. 입력 데이터프레임(pickled_dataframe)을 지정하기 위해 **notebooks/streetcar_model_training_config.yml** 파일 내용을 수정합니다. **데이터 준비** 단계의 출력으로 얻은 데이터프레임 파일 이름으로 지정되어야 합니다.
  2. **streetcar_model_training_xgb.ipynb** 를 실행합니다. - 학습된 XGBoost 모델은 딥러닝 모델과 함께 **models** 디렉터리에 저장됩니다.

- **모델의 웹 배포** - 학습된 모델을 간단한 웹 페이지로 상호 작용하기 위한 배포 단계. 앞 단계의 파이프라인 파일과 학습된 모델을 사용합니다.
  1. **deploy_web/deploy_web_config.yml** 파일을 수정합니다: `pipeline1_filename`, `pipeline2_filename`, `model_filename`을 앞 단계에서 얻은 파이프라인 파일과 학습된 모델의 파일 이름에 맞춰 설정합니다. 또는 설정 파일을 그대로 사용해 학습용 노트북을 실행하면, 저장소가 이미 제공하는 준비된 파이프라인과 모델 파일을 사용할 수 있습니다.
  2. **deploy_web/flask_server.py**을 실행해 Flask 서버를 구동합니다: `python flask_server.py`
  3. 브라우저로 `localhost:5000`에 접근합니다. 그리고 경전철 여정에 대한 세부 사항을 입력한 뒤 **Get prediction** 버튼을 클릭합니다.

## 배경 자료

- [**원본 데이터셋**](https://open.toronto.ca/dataset/ttc-streetcar-delay-data/)
- [**이 예제의 케라스 모델을 생성하는 데 사용된 캐글 서브미션**](https://www.kaggle.com/knowledgegrappler/a-simple-nn-solution-with-keras-0-48611-pl) 
