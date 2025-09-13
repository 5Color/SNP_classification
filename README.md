# SNP_classification

유전체 정보를 이용하여 품종을 분류하는 AI 모델을 학습하여 이미 끝난 경진대회에 제출해보았다.
다음 소개, 설명하는 코드는 main_2버전이다.

# 사용 데이터:
우선 사용된 데이터는 데이콘 사이트 https://dacon.io/competitions/official/236035/data 링크에 있다.
참고하길 바란다.

# import
[pandas, lightgbm의 LGBMClassifier, sklearn.model_selection의 train_test_split, sklearn.preprocessing의 LabelEncoder]

# 데이터 로드
pandas의 read_csv함수를 이용해train.csv와 test.csv데이터를 불러오고, 변수에 참조한다음, DataFrame으로 변환해준다.
train데이터의 행, 열의 크기는 262 rows × 21 columns이고, 
test데이터의 행, 열의 크기는 175 rows × 19 columns이다.

# 전처리
이제 학습에 이용될 x (독립변수)데이터를 만들어줘야 하는데 이때  id와 정답지:Label ..('class')컬럼을 없애줘야하기에 
train.drop으로 없애주고 train_x변수에 참조해준다.
그리고, 현재 train_x데이터는 object데이터타입과, int데이터타입이 섞여있어 LabelEncoder() 이용하여 모두 정수로 전처리 해준다.

아 여기서 굳이 없어도 되는 코드가 있는데 test는 전처리쪽에 test[col] = le.fit_transform(test[col]) 코드는 없어도 된다.
왜냐면 데이터에 숫자만 있기때문이어서다

train_y = train['class']로 y (종속변수) 데이터를 만들어준다.
class컬럼은 A, B, C와 같은 문자로 되어있기에 이것도 LabelEncoder()이용해 전처리 해준다.

전처리의 마지막으로 데이터셋을 train_test_split함수로 분리해준다 이때 중요 포인트 stratify=train_y를 설정해줘야한다
이게 뭔 뜻이냐면 만약 데이터  A:100개 ,B: 50개, C:5개로 이루어진 train_y라고 가정해보자 그러면 이걸 비율에 맞게 
test셋에 A:10, B:5, C:1로 분류해준다는것이다.

# 모델 학습 및 결과
## lightGBM 모델
#### lgbm = LGBMClassifier()
train_input, train_target을 fitting시켜주어 학습시켜주면 된다.

이제 모델학습을 했으니 예측결과값을 제출하여 점수를 환산해보자.
우선 lgbm.predict(test)를 preds변수에 참조해주고 실행
sample_submission.csv파일을 불러와 submit2에 참조,
submit2['class'] = le.inverse_transform(preds)코드를 실행하자
다음 코드 분석을 해보자면 submit2에 class컬럼을 만들고, preds정수로 된 예측값을 LabelEncoder의 객체인
le의 inverse_trainsform이라는 인코딩된걸 원래대로 A,B,C처럼 원상복구시켜주는 함수를 사용해 
대회에 제출할 수 있도록 하는것이다.

마지막으로 submit2.to_csv을 하여 저장 파일 제출
## 제출 결과
점수는 0.9719171917이네요.



