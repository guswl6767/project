# vggnet 

## 망의깊이
VGGNet이 발표되기 전에는 아래 그림과 같이, deep 이라고 하지만 8 layer 수준이었다. 하지만 공교롭게 2014년은 GoogLeNet과 VGGNet이 모두 이전 구조에 비해 훨씬 deep 해진다. 그리고 2015년에 발표된 마이크로소프트의 ResNet은 무려 152 layer로 더욱 깊어지게 된다.
![depth](https://mblogthumb-phinf.pstatic.net/20160617_286/laonple_1466131093651LcTGy_PNG/%C0%CC%B9%CC%C1%F6_4.png?type=w2)
이렇게 망이 깊어지는 이유는 이미 GoogLeNet class에서 설명을 하였듯이, 훨씬 더 복잡한 문제를 풀 수 있기 때문이다.

다음 그림은 VGGNet과 AlexNet의 구조를 비교 설명하는 것인데, 확실하게 깊이에서 차이가 남을 확인할 수 있다. 하지만 전반적으로 그 구조는 LeNet5 및 AlexNet과 크게 다르지 않음을 확인할 수 있다.
![AlexNet](https://mblogthumb-phinf.pstatic.net/20160709_13/laonple_1468026470293b4dnB_PNG/alexnet.png?type=w2)
![vggnet](https://mblogthumb-phinf.pstatic.net/20160709_69/laonple_1468026496381NVR0l_PNG/VGGNet.png?type=w2)

## vggnet 구조
  VGGNet 연구팀이 그들의 논문 “Very deep convolutional networks for large-scale image recognition”에서 밝혔듯이, 원래는 망의 깊이(depth)가 어떤 영향을 주는지 연구를 하기 위해 VGGNet을 개발한 것 같다.



오직 깊이가 어떤 영향을 주는지 밝히기 위해, receptive field의 크기는 가장 간단한 3x3으로 정하고 깊이가 어떤 영향을 주는지 6개의 구조에 대하여 실험을 한다.
3x3 kernel을 사용하면 5x5, 7x7 혹은 그 이상의 크기를 갖는 convolution을 인수분해하여 깊이는 깊어지지만 파라미터 수는 줄여서 표현할 수 있다는 것이다.


처음부터 의도를 했던 것인지 아니면, 3x3을 기본으로 하면서 망이 깊은 구조를 만들다 보니 좋은 결과가 나왔는지는 명확하지 않지만, 어쨌든 그들의 구조 실험은 대 성공을 거두고 ILSVRC에서도 좋은 결과를 얻게 된다.

VGGNet은 AlexNet이나 ZFNet 처럼, 224x224 크기의 color 이미지를 입력으로 받아들이도록 하였으며, 1개 혹은 그 이상의 convolutional layer 뒤에 max-pooling layer가 오는 단순한 구조로 되어 있다. 또한 기존 CNN 구조처럼, 맨 마지막 단에는 fully-connected layer가 온다.


VGGNet의 단점은 GoogLeNet 저자 Szegedy가 비판을 했던 것처럼, 파라미터의 개수가 너무 많다는 점이다. 아래 표를 보면 알 수 있는 것처럼, GoogLeNet의 파라미터의 개수가 5 million 수준이었던 것에 비해 VGGNet은 가장 단순한 A-구조에서도 파라미터의 개수가 133 million 으로 엄청나게 많다.


그 결정적인 이유는 VGGNet의 경우는 AlexNet과 마찬가지로 최종단에 fully-connected layer 3개가 오는데 이 부분에서만 파라미터의 개수가 약 122 million개가 온다. 참고로 GoogLeNet은 Fully-connected layer가 없다.


## vggnet 특징
* VGGNet에서 A-LRN은 Local Response Normalization이 적용된 구조인데, 예상과 달리 VGGNet 구조에서는 LRN이 별 효과가 없어 사용하지 않는다.
* 1x1이 적용되기는 하지만, 적용하는 목적이 GoogLeNet이나 NIN의 경우처럼 차원을 줄이기 위한 목적이라기 보다는 차원은 그대로 유지하면서 ReLU를 이용하여 추가적인 non-linearity를 확보하기 위함이다.
* Deep net은 학습할 때 vanishing/exploding gradient 문제로 학습이 어려워질 수 있는데, 이것을 먼저 11-layer의 비교적 간단한 구조-A를 학습시킨 후 더 깊은 나머지 구조를 학습할 때는 처음 4 layer와 마지막 fully-connected layer의 경우는 구조-A의 학습 결과로 초기값을 설정한 후 학습을 시켜 이 문제를 해결하였다. (참고로 GoogLeNet은 auxiliary classifier를 사용하여 이 문제를 해결하였다.
  
## vggnet 망의 깊이
망이 깊어지면 왜 좋은지는 이미 GoogLeNet class에서 충분히 살펴보았다. GoogLeNet은 망을 깊게 만들면서 파라미터의 수를 줄이기 위해 inception이라는 독특한 구조를 만들었고, 깊어지는 망의 학습을 돕기 위해 auxiliary classifier 개념을 고안했다.

 

VGGNet 팀은 깊은 망의 독특한 구조를 설계하는 것이 목적이 아니라, 망의 깊이가 어떤 영향을 끼치는지를 확인하기 위해 가장 단순한 구조인 3x3 convolution layer를 겹쳐서 사용하는 방법을 취했으며, 여러 개의 구조 실험을 통해, 어느 정도 이상이 되면 성능 개선의 효과가 미미해지는 지점이 있음을 확인했다. (ILSVRC-2012 데이터의 경우, 16-layer가 넘어서면 별 이득이 없는 것으로 논문을 통해서 확인이 되었다. 몇 개의 layer가 최적인지는 학습 데이터의 수나 해결하고자 하는 문제의 성격에 따라 달라질 수 있다)



VGGNet의 기본 개념은 다음 그림과 같다. CNN의 기본 구조가 convolutional layer 다음에 해상도를 줄이기 위한 pooling layer가 오는 구조인데, 여기서 노란색 부분에  receptive field의 크기가 3x3인 filter를  여러 개를 stack 하는 구조를 택했다.
![vggnet depth](https://blogfiles.pstatic.net/20160630_68/laonple_1467259171401eSHTA_PNG/%C0%CC%B9%CC%C1%F6_41.png?type=w2)
이렇게 3x3 convolutional layer를 2개 쌓으면 5x5 convolution이 되고, 3개를 쌓으면 7x7 convolution이 된다. 그리고 덤으로 얻을 수 있는 효과는 파라미터의 수가 줄어들고, 그로 인해 학습의 속도가 빨라진다는 점이다. 

또한 layer 수가 많아질수록 non-linearity가 더 증가하기 때문에 좀 더 유용한 feature를 추출하기에 좋아진다. 그래서 실제로 5x5 1개로 구현하는 것보다는 3x3 2개로 구현하는 편이 결과가 더 좋다. 

11-layer의 구조에서 중간에, 아래 그림처럼 화살표의 위치에 convolutional layer를 추가하여 13-layer 구조로 만들고, 13-layer 구조에 다시 convolutional layer를 추가하여 16-layer 구조로 발전시켰으며, 구조 발전의 방향은 아래 그림과 같다.
![layer](https://blogfiles.pstatic.net/20160630_37/laonple_1467259171767Plvqh_PNG/%C0%CC%B9%CC%C1%F6_42.png?type=w2)

망이 깊어지면 vanishing/exploding gradient의 문제로 인해 학습에 어려움이 생기는데, 이 문제를 해결하기 위해  VGGNet은 11-layer 구조의 학습 결과를 더 깊은 layer의 파라미터 초기화 시에 이용함으로써 학습의 시간을 줄이고, vanishing gradient 문제도 해결하였다.

## Vggnet에서 성능을 끌어올리기 위해 적용한 방법
VGGNet 팀은 3x3 convolution이라는 단순한 구조로부터 성능을 끌어내기 위해, training과 test에 많은 공을 들였으며, 다양한 경우에 성능이 어떻게 달라지는지 공개하여 CNN 구조 이해에 많은 기여를 하였다.

### training 방법
___
ILSVRC-2012 에는 1000개의 class가 있으며, 각각의 class에는 약 1000장 정도의 학습 이미지가 있다. Class 당 1000장의 학습 데이터라면 결코 많다고 볼 수 없으며, overfitting에 빠지지 않도록 학습 방법을 잘 설정해줘야 한다. 이 때 흔히 사용하는 방법이 지능적으로 학습 데이타의 양을 늘리기 위한 data augmentation 기법이다.



AlexNet은 모든 학습 이미지를 256x256 크기로 만든 후, 거기서 무작위로 224x224 크기의 이미지를 취하는 방식으로 학습 데이터의 크기를 2048배로 늘렸으며, RGB 컬러를 주성분 분석(PCA)를 사용하여 RGB 데이터를 조작하는 방식도 사용하였다. 하지만 AlexNet에서는 모든 이미지를 256x256 크기의 single scale 만을 사용하였다.



반면에 GoogLeNet은 영상의 가로/세로 比를 [3/4, 4/3]의 범위를 유지하면서 원영상의 8%부터 100%까지 포함할 수 있도록 하여 다양한 크기의 patch를 학습에 사용하였다. 또한 photometric distortion을 통해 학습 데이타를 늘렸다.



VGGNet에서는 training scale을 ‘S’로 표시하며, single-scale training과 multi-scaling training을 지원한다. Single scale에서는 AlexNet과 마찬가지로 S = 256으로 고정시키는 경우와 S = 256과 S = 384, 두개의 scale을 지원한다.



Multi-scale의 경우는 S를 Smin과 Smax 범위에서 무작위로 선택할 수 있게 하였으며, Smin은 256이고 Smax는 512이다. 즉, 256과 512 범위에서 무작위로 scale을 정할 수 있기 때문에 다양한 크기에 대한 대응이 가능하여 정확도가 올라간다. Multi-scale 학습은 S = 384로 미리 학습 시킨 후 S를 무작위로 선택해가며 fine tuning을 한다. S를 무작위로 바꿔 가면서 학습을 시킨다고 하여, 이것을 scale jittering이라고 하였다.



이렇게 크기 조정을 통해 얻은 학습 영상으로부터 AlexNet과 마찬가지 방식으로 무작위로 224x224 크기를 선택하였으며, RGB 컬러 성분에 대한 변경 역시 비슷한 방식으로 수행했다.



GoogLeNet과 VGGNet은 그 이름과 표현만 좀 다를 뿐이지, 결과적으로는 multi-scale을 고려하였고, RGB 컬러 성분 변경을 통해 deep network이 적은 학습 데이터로 인한 overfitting 문제에 빠지는 것을 최대한 막으려는 노력을 하였다.

### testing 방법
___
Test는 학습된 신경망에서 최적의 결과를 도출해내는 것이 목적이며, 대부분 학습 이미지로부터 여러 개의 patch 혹은 crop을 이용하여, 가능한 많은 test 영상을 만들어 낸 후 여러 영상으로부터의 결과를 투표(voting)을 통해 가장 기대되는 것을 최종 test 결과로 정한다. 


AlexNet은 Test 영상을256x256 크기로 scaling하고, 그 영상을 4 코너와 중앙에서 224x224 크기의 영상을 잘라 (CNN에서는 crop이라고 함)서 5개의 영상을 만들고, 이것을 좌우 반전시켜 총 10장의 Test 영상을 만들어 냈다. 10장의 테스트 영상을 망에 집어넣고 10개의 결과를 평균하는 방식으로 최종 출력을 정했다. Softmax의 결과가 숫자로 나오기 때문에 평균을 그것들에 대한 평균을 취해 class를 결정한다.



GoogLeNet의 경우는 테스트 영상을 256x256 1개의 scale만 사용하는 것이 아니라, 256, 288, 320, 352 로 총 4개의 scale로 만들고 각각의 scale로부터 3장의 정사각형 이미지를 선택하고, 다시 선택된 이미지로부터 4개의 코너 및 센터 2개를 취해 총 6장의 224x224 크기 영상을 취하고, 각각을 좌우반전 시키면 4x3x6x2, 총 144개의 테스트 영상을 만들어 냈다. 결과에 대해서는 AlexNet과 마찬가지로 voting을 사용한다.



VGGNet은 ‘Q’라고 부르는 test scale을 사용하며, 테스트 영상을 미리 정한 크기 Q로 크기 조절을 한다. Q는 training scale S와 같을 필요가 없으며, 당연한 이야기이겠지만, 각각의 S에 대해 여러 개의 Q를 사용하게 되면 학습의 결과는 좋아진다.






VGGNet의 경우는 GoogLeNet과 같은 multi-crop( GoogLeNet은 144장, VGGNet은 150장) 방식의 테스트 영상 augmentation 방법을 적용하기도 했지만, 연산량을 줄이기 위해 OverFeat 구조(OverFeat: Integrated Recognition, Localization and Detection using Convolutional Networks)에서 사용한 dense evaluation 개념을 적용하였다. 최종적으로는 성능을 더 끌어올리기 위해, multi-crop 방식과 dense evaluation 개념을 섞어 사용하였다.



Dense evaluation은 OverFeat 구조에서 매우 중요한 부분이고, 이해를 위해 설명이 좀 더 필요하기 때문에 다음 class에서 따로 상세하게 다룰 예정이다.

 

Dense evaluation의 장점은 crop의 경우처럼 원 학습 영상으로부터 영상을 잘라낸 후 그것들을 각각 ConvNet에 적용시키는 방식이 아니라, 큰 영상에 대해 곧바로 ConvNet을 적용하고 일정한 픽셀 간격(grid)으로 마치 sliding window를 적용하듯이 결과를 끌어낼 수 있어, 연산량 관점에서는 매우 효율적이지만, grid 크기 문제로 인해서 학습 결과가 약간 떨어질 수 있다. 그러므로 crop과 dense evaluation을 상보적으로(complementary) 섞어 사용하면 더 성능이 좋아진다.

 ## Vggnet 결과
VGGNet에 single-scale test 영상을 적용했을 때의 결과는 아래 표와 같다. 망이 깊어질수록 결과가 좋아지고, 학습에 scale jittering을 사용한 경우에 결과가 더 좋다는 것을 확인할 수 있다.

 

3x3 convolution layer 2개를 겹치면 5x5 convolution의 효과를 얻을 수 있다는 것은 이미 앞선 class에서 살펴봤다. 둘 중 어느 결과가 좋을까?



B 구조에 3x3 convolution layer를 2개 겹쳐서 사용하는 경우와 5x5 convolution 1개를 사용하는 모델을 만들어 실험을 했는데, 결과는 3x3 2개를 사용하는 것이 5x5 1개보다 top-1 error에서 7% 정도 결과가 좋았다고 한다. 3x3 convolution이 단순하게 망을 깊게 만들고, 파라미터의 크기를 줄이는 것뿐만 아니라, 뉴런에 있는 non-linearity 활성함수를 통해 feature 추출 특성이 좋아졌음을 반증한다.
![](https://blogfiles.pstatic.net/20160630_289/laonple_1467259172054sspYy_PNG/%C0%CC%B9%CC%C1%F6_43.png?type=w2)

Multi-scale test 결과는 아래 표와 같다. Multi-scale test는 S가 고정된 경우는 {S-32, S, S+32}로 Q 값을 변화 시키면서 테스트를 했다. 여기서 학습의 scale과 테스트의 scale이 많이 차이가 나는 경우는 오히려 결과가 더 좋지 못해 32만큼 차이가 나게 하여 테스트를 하였다.

 

학습에 scale jittering을 적용한 경우는 출력의 크기는 [256, 384, 512]로 테스트 영상의 크기를 정했으며, 예상처럼 scale jittering을 적용하지 않은 것보다 훨씬 결과가 좋고, single-scale 보다는 multi-scale이 결과가 좋다는 것을 확인할 수 있다.

![](https://blogfiles.pstatic.net/20160630_161/laonple_1467259323970DuKM3_PNG/%C0%CC%B9%CC%C1%F6_46.png?type=w2)

앞서 살펴본 것처럼, multi-crop과 dense evaluation은 각각 적용했을 때는 grid 크기 문제로 인해 multi-crop이 아주 약간 성능이 좋은 편이며, 상보적인 특성을 갖고 있기 때문에 같이 적용을 하게 되면 성능이 개선되는 것을 아래 표처럼 알 수가 있다.

![](https://blogfiles.pstatic.net/20160630_120/laonple_14672591723355hgpI_PNG/%C0%CC%B9%CC%C1%F6_45.png?type=w2)

이상으로 VGGNet의 특성과 성능 개선을 위해 그들이 어떤 노력을 했는지 살펴보았다. 결론적으로 VGGNet은 그 구조가 간단하여 이해나 변형이 쉬운 분명한 장점을 갖고 있기는 하지만, 파라미터의 수가 엄청나게 많기 때문에 학습 시간이 오래 걸린다는 분명한 약점을 갖고 있다.