# RNN과 LSTM을 이해해보자

Recurrent Neural Networks(RNN) 과 RNN의 종류 중 하나인                                         Long Short-Term Memory models(LSTM)에 대해 알아보자.

## RNN

순차적으로 등장하는 데이터 처리에 적합한 모델.

RNN이 기존의 뉴럴 네트워크와 다른 점은 ‘기억’(다른 말로 hidden state)을 갖고 있다는 점입니다. 네트워크의 기억은 지금까지의 입력 데이터를 요약한 정보라고 볼 수 있습니다.

시퀀스의 길이에 관계없이 인풋과 아웃풋을 받아들일 수 있는 구조라 유연하게 구조를 만들 수 있다.

![RNN 다이어그램](https://files.slack.com/files-pri/T25783BPY-F6YUKQKCP/rnn-diagram.png?pub_secret=9e9b7d3f1e)

위 다이어그램에서 빨간색 사각형은 `입력`, 노란색 사각형은 `기억`, 파란색 사각형은 `출력`을 나타냅니다. 첫번째 입력이 들어오면 첫번째 기억이 만들어집니다. 두번째 입력이 들어오면 기존의 기억과 새로운 입력을 참고하여 새 기억을 만듭니다. 입력의 길이만큼 이 과정을 얼마든지 반복할 수 있습니다. 각각의 기억은 그때까지의 입력을 요약해서 갖고 있는 정보입니다. RNN은 이 요약된 정보를 바탕으로 출력을 만들어 냅니다.



![img](http://i.imgur.com/Q8zv6TQ.png)

1. 고정크기 입력 & 시퀀스 출력. 

   예) 이미지를 입력해서 이미지에 대한 설명을 문장으로 출력하는 이미지 캡션 생성

2. 시퀀스 입력 & 고정크기 출력. 

   예) 문장을 입력해서 긍부정 정도를 출력하는 감성 분석기

3. 시퀀스 입력 & 시퀀스 출력.

   예) 영어를 한국으로 번역하는 자동 번역기

### RNN의 한계

#### 장기 의존성(Long-Term Dependency) 문제점

가끔, 우리는 최근 데이터를 가지고 현재의 문제를 해결해야하는 상황에 직면하게 됩니다. 예를 들어서, 이전 단어 선택을 활용하여 다음에 입력될 단어를 예측하는 언어 모델을 생각해봅시다. 만약에 우리가 "the clouds are in the sky,"라는 문장에서 "the clouds are in the"라는 입력값을 받고 마지막 단어를 예측해야 한다면, 우리는 더 이상 문맥이 필요하지 않습니다. 명확하게 다음에 입력될 단어는 "sky"가 될 확률이 높습니다. 이러한 경우에 제공된 데이터와 배워야 할 정보의 입력 위치 차이(Gap)가 크지 않다면, RNN은 과거의 데이터를 기반으로 학습을 할 수 있게 됩니다.

![img](https://t1.daumcdn.net/thumb/R1280x0/?fname=http://t1.daumcdn.net/brunch/service/user/IgT/image/4mQRpbvw6CfP7BPmEDwaGouueEs.png)

 하지만, 우리는 더 많은 문맥이 필요할 때가 있습니다. 예를 들어 "I grew up in France... I speak fluent French.(나는 프랑스에서 자라났어... 나는 프랑스어를 유창하게 해)"라는 문장에서 마지막 단어 French(프랑스어)를 예측하는 문제를 생각해보겠습니다. 최근 정보를 기반으로 예측 모델은 다음 단어가 아마도 언어의 한 종류라고 예측될 것입니다. 그렇다면, 이 예측 모델은 "I grew up in France(나는 프랑스에서 자라났다)"에서 프랑스라는 문맥이 필요하게 됩니다. 실제로 "I grew up in France(프랑스에서 자라났다)"는 표현과 "I speak fluent (나는 언어를 유창하게 한다)"라는 표현의 위치가 멀어지는 문제는 아주 빈번하게 발생합니다.

이론상으로, RNN은 장기 의존성 문제를 해결할 능력이 있습니다. 사람은 이러한 장기 의존성 문제를 풀 수 있도록 신중하게 파라미터를 선택하면 가능은 합니다만, 슬프게도 실제적으로 RNN은 이러한 문맥을 배울 수 없다는 것이 밝혀졌습니다. 

## LSTM(Long Short Term Memory networks)

 LSTM은 장기 의존성 문제를 해결하기 위해 명시적으로 디자인되었습니다. 오랜 기간동안 정보를 기억하는 일은 LSTM에 있어 특별한 작업없이도 기본적으로 취하게 되는 기본 특성입니다. 장기 의존성 문제를 풀려고 특별히 열심히 노력할 필요가 없습니다.

![img](https://t1.daumcdn.net/thumb/R1280x0/?fname=http://t1.daumcdn.net/brunch/service/user/IgT/image/I0UJ8f2U5ePsX3LU-kJS--yIarU.png)



![img](https://t1.daumcdn.net/thumb/R1280x0/?fname=http://t1.daumcdn.net/brunch/service/user/IgT/image/g-NiohVI_BwATq4Uvt7gBPv4IWs.png)

 위 다이어그램에서 각 라인은 온전한 vector를 포함합니다. 각 출력값은 다른 노드의 입력값이 됩니다. 분홍색 원은 점단위의 연산을 표현합니다(예를 들어, 벡터 더하기 연산이 있습니다.) 그리고 노란색 박스는 뉴럴 네트워크의 단위입니다. 하나로 합쳐지는 4번째 기호는 집중을 의미합니다. 반면에 하나의 화살표가 두개로 나눠지는 5번째 기호는 결과값이 서로 다른 두 노드에 복사되는 것을 의미합니다.





출처:

https://brunch.co.kr/@chris-song/9

https://ratsgo.github.io/natural%20language%20processing/2017/03/09/rnnlstm/



- 데이터 배열을 6단위로 변경
- RNN 모델 적용시켜서 RESNET 과 같이 사용해보기