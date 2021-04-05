## CNN(Convolutional Neural Networks)

### 1.CNN이 무엇일까?

​	Convolution 이라는 전처리 작업이 들어가는 Neural Network 모델, 기존의DNN(Deep Neural Network) 은 1차원 형태의 데이터만 사용하기 때문에 공간,지역 정보가 손실된다. 추상화 과정없이 연산과정으로 넘어가 학습효율이 떨어진다.

​	이를 보완하기 위해 raw input을 그대로 받아서 공간,지역 정보의 feature들의 계층을 빌드업한다. CNN은 이미지 전체보다는 부분을 보고 한 픽셀과 주변 픽셀들의 연관성을 살린다.

![Image for post](https://miro.medium.com/max/1058/1*NFsuKOLUultX5U2wb_OpXQ.png)

예를 들어 우리는 어떠한 이미지가 주어졌을때 이것이 새의 이미지인지 아닌지 결정할 수 있는 모델을 만들고 싶다고 가정합시다.

모델이 전체 이미지를 보는 것 보다는 새의 부리 부분을 잘라 보는게 더 효율적이겠죠? 그것을 해주는것이 CNN입니다. CNN의 뉴런이 패턴(이 경우에서는 새의 부리)을 파악하기 위해서 전체 이미지를 모두 다 볼 필요가 없습니다.



### 2.CNN의 주요 컨셉들

#### 2.1 Convolution의 작동 원리

![Image for post](https://miro.medium.com/max/1539/1*xVKN9dBAzPn7E-DwaIjK3Q.png)

전체 이미지에 Filter을 통해 훑어주면서 우리의 입력값 이미지의 모든 영역에 같은 필터를 반복 적용해 패턴을 찾아 처리하는 것이 목적이다.

![Image for post](https://miro.medium.com/max/1436/1*Gj7y0S1VG35njLYk4Vy_0A.png)

Filter 가 이미지를 훑을 때 작동하는 연산처리

![Image for post](https://miro.medium.com/max/1748/1*s6yBccVNHVuEEO9Ur6Az9w.png)

#### 2.2 Zero Padding

Convolution 과정을 거치게 되면 5x5 크기의 이미지가 3x3으로 줄어들게된다. 이를 해결하기 위해서 Padding 을 통해 테두리를 0으로 감싸준다. 입력값의 크기 7X7로 변경되어 결과값의 크기가 5X5로 기존 이미지와 같아진다.

#### 2.3 Stride

필터를 얼만큼 움직일 것인가에 대한 값. 기본값은 1이다.

#### 2.4 The Order-3 Tensor

3차원의 경우.

### 3.CNN의 전체적인 네트워크 구조

![Image for post](https://miro.medium.com/max/960/1*usA-K08Tn5i6P7eLvV8htg.png)

#### 3.1 첫번째 Convolutional Layer

![Image for post](https://miro.medium.com/max/1685/1*E-5sL3jJMIygx0Xm9_y6Ew.png)

28x28 크기의 이미지를 여러개의 Filter을 통해 결과값을 구한다.

그림의 예시로 보면 10개의 24x24 output에 Activation Function을 통해 Convolutional Layer을 완성한다.

```
Convolutional Layer = Convolution Outputs + Activation Function
```

#### 3.2 첫번째 Pooling Layer

![Image for post](https://miro.medium.com/max/1685/1*wGrlfRciYNcqhGqBWSiErg.png)

Pooling은 이미지의 output의 갯수가 너무 많아져서 생기는 문제를 방지해줌.            각 결과값의 dimentionality를 축소해주는 목적을 가짐. correlation이 낮은 부분을 제거하여 dimension을 축소시킨다.

![Image for post](https://miro.medium.com/max/774/1*8DRW7Uw6lHfAdPdXrHiY9w.png)

각각 Max값을 갖고오고, Average 값을 가져와서 4x4 크기를 2x2로 줄여준다.



#### 3.3 두번째 Convolutional Layer

![Image for post](https://miro.medium.com/max/1740/1*3I8tqf4zMdfIpFCjTB-77g.png)

이번에는 Tensor Convolution을 적용하여 12x12x10의 텐서를 대상으로 5x5x10크기의 텐서필터 20개를 적용하여 8x8 크기의 결과값 20개를 얻을 수 있다.



#### 3.4 두번째 Pooling Layer

![Image for post](https://miro.medium.com/max/1679/1*nIwZpkWG5iT5cTJWzhnbXg.png)

3.2에서 진행한 Pooling 과정을 한번 더 진행



#### 3.5 Flatten(Vectorization)

![Image for post](https://miro.medium.com/max/1682/1*v8TDECzNYJBrsSvlSLTcNg.png)

Flatten : 4x4x20의 텐서를 일자 형태의 데이터로 펴준다.

320 - dimension을 가진 vector 형태로 바뀌게 된다.

두번째 Pooling Layer을 통해서 4x4 크기의 이미지들은 입력된 이미지에서 얻어온 특이점 데이터가 되어 1차원의 벡터 데이터로 변형시켜주어도 무관하다.



#### 3.6 Fully-Connected Layers(Dense Layers)

![Image for post](https://miro.medium.com/max/1680/1*EcOvxbhodURWEjXXepKYIw.png)

이제 마지막으로 FC Layer을 통해 Softmax activation function을 적용해주면 최종 결과물을 출력하게 된다.



### 4. 매개변수(Parameter) 와 Hyper-parameter

#### 4.1 학습 가능한 parameters

![Image for post](https://miro.medium.com/max/1899/1*WURJp4FEj2yr1eyozpnBhg.png)

- 첫 convolution과정에서 5x5크기의 필터 10개를 사용하였으므로 **250개**의 매개변수가 존재합니다.
- Pooling layer에서는 단순히 크기를 줄이는 개념이므로 매개변수가 **없습니다**.
- 두번째 convolution 과정에서 5x5x10크기의 텐서 스무개를 사용하였으므로 **5,000개**의 매개변수가 추가되었습니다
- 두개의 Fully-connected layer들에서 각각 매개변수 **32,000개, 1000개**가 추가되었습니다.

결과적으로, 생략한 intercepts 값까지 고려해보면 총 **38,250 이상**의 학습 가능한 매개변수를 갖게 됩니다.



#### 4.2 Hyper-parameters

마지막으로 CNN모델에서 튜닝 가능한 하이퍼파라미터는 어떤 것들이 있는지 간단히 살펴보겠습니다.

- Convolutional layers: 필터의 갯수, 필터의 크기, stride값, zero-padding의 유무
- Pooling layers: Pooling방식 선택(MaxPool or AvgPool), Pool의 크기, Pool stride 값(overlapping)
- Fully-connected layers: 넓이(width)
- 활성함수(Activation function): ReLU(가장 주로 사용되는 함수), SoftMax(multi class classification), Sigmoid(binary classification)
- Loss function: Cross-entropy for classification, L1 or L2 for regression
- 최적화(Optimization) 알고리즘과 이것에 대한 hyperparameter(보통 learning rate): SGD(Stochastic gradient descent), SGD with momentum, AdaGrad, RMSprop
- Random initialization: Gaussian or uniform, Scaling

출처: https://halfundecided.medium.com/%EB%94%A5%EB%9F%AC%EB%8B%9D-%EB%A8%B8%EC%8B%A0%EB%9F%AC%EB%8B%9D-cnn-convolutional-neural-networks-%EC%89%BD%EA%B2%8C-%EC%9D%B4%ED%95%B4%ED%95%98%EA%B8%B0-836869f88375



















