
## N-gram

어떤 문장의 연속된 n개의 word에서 n번째 나올 수 있는 word를 예측하는 모델이다. 학습은 "(n-1)번째 word까지 입력이 있으면 다음 word로는 n번째 word가 출력된다"라는 구성을 하나의 데이터셋으로 보고 진행되며, 예측은 그 빈도를 측정해 확률적으로 예측한다.

예시) 아래 문장에서 **4-gram 언어 모델**을 사용해 예측하는 경우.

```
"The invariable mark of _____ is to see the miraculous in the common."

예측은 "invariable mark of _____" 부분에서 빈 칸을 채우기 위해 발생하며, 
기존에 학습된 데이터 중에서 "invariable mark of" 문장과 정확히 일치할 때 다음으로 어떤 word가 자주 출력되었는지를 기반으로 예측한다.
```

한계 : `희소 문제 (sparsity problem)` - 학습되지 않은 데이터에 대해서는 예측이 불가능하다. (앞선 예시에서 "invariable mark of"와 일치하는 문장이 학습되지 않은 경우는 빈도수가 0이기 때문에 예측이 불가하다.)


## NNLM (Neural Network Language Model)

<img src=https://wikidocs.net/images/page/45609/nnlm5_final.PNG widh=400 height=250/>

Projection Layer (투사층) : 정의된 윈도우 크기를 따라서 입력층으로부터 one-hot vector로 변환된 word를 입력받고, lookup table을 기반으로 embedding word 목록은 합쳐서(concatenate) 은닉층으로 전달한다.
> lookup table은 (투사층의 크기) * (one-hot vector의 차원) 의 크기를 가지는 가중치 행렬로, 학습 과정에서 값이 수정될 수 있다.

Hidden Layer (은닉층) : 투사층의 embedding vector를 입력받아 가중치를 곱하고 편향을 더한 값을 출력

Output Layer (출력층) : 은닉층의 출력결과를 기반으로 다음에 나올 가능성이 높은 word 활성화
> 상세설명 : y hat 출력은 one-hot vector가 나타내는 각 word가 다음에 나올 가능성을 0~1의 실수값으로 나타내며, 출력층에서는 해당 vector에 softmax 함수를 사용해 가능성이 가장 높은 word를 선별한다.

장점 : 학습과정에서 유사한 목적으로 사용되는 단어들은 유사한 임베딩 값을 가지게되므로, 훈련되지 않은 문맥이라도 다음에 나올 수 있는 단어를 선택할 수 있다. (희소문제 해결)

한계 : N-gram과 같은 윈도우닝을 기반으로 하고있어, 문맥 전체가 아닌 고정된 길이의 n개의 단어만을 참고해 예측할 수 있다.

## RNN (Recurrent Neural Network Language Model)

입력된 값을 은닉층에서 처리하고 활성화 함수로부터 출력된 결과값을 출력층 뿐만 아니라 다시 은닉층으로 전달해 다음 계산에 활용하는 방식을 사용한다.
> 단순히 출력값을 출력층으로 전달하는 방법은 Feed Forward Neural network 라고 부른다.


## LSTM (Long Short-Term Memory)

기존 RNN은 이전 정보다 다음 정보 처리를 위한 값으로 전달되는데, 처리 단계(time step)가 길어지면 앞선 정보의 양이 점점 희석되면서 중요한 정보라도 손실되는 경우가 있다. 이를 해결하기 위해 LSTM에서는 은닉층에 현재 정보와 상태값을 추가로 사용해 처리하는 방식을 사용한다.
- 입력 게이트 : 현재 정보를 기억
- 출력 게이트 : 현재 상태의 은닉 상태 결정, 현재 시점의 입력값(토큰)과 이전 시점의 은닉 상태에 대한 시그모이드 함수 값
- 삭제 게이트 : 삭제 과정을 거친 정보의 양, 현재 시점의 입력값(토큰)과 이전 시점의 은닉 상태에 대한 시그모이드 함수 값
- 셀 상태 : 이전 및 현재 정보를 얼마나 반영할지 결정


## Seq2Seq (Sequence to Sequence)

입력된 시퀀스로부터 다른 도메인의 시퀀스를 출력하는 방법을 인코더(Encoder), 디코더(Decoder) 및 RNN 을 사용해 구현한 모델

단점 : 인코딩 결과는 고정된 크기의 context vector값으로 출력되어 디코더로 전달되기 때문에 정보손실 및 병목현상이 발생할 수 있다.

## Attention

Seq2Seq 모델을 기반으로, 디코더의 입력값으로 기존 context vector값과 함께 인코더의 각 입력값(토큰) 처리 결과 전체를 전달/처리하는 방법을 사용해 정보손실을 줄인 모델

디코딩 과정에서는 인코딩 과정의 각 입력값들 중 어느 입력값에 더 집중(attention)해서 처리해야하는지 에너지 및 가중치 값을 계산해 사용한다.


## Transformer

트랜스포머는 아래 3개의 주요 요소들을 통해 RNN을 대체할 수 있었다.

1. Positional Encoding : 위치 기반으로 각 입력 embedding에 대한 다양한 활성화 사인파(sine wave)들을 추가해, NLP에서 RNNs를 사용함으로써 얻을 수 있는 sequence내 순서를 고려하는 능력을 대체
2. Self-attention : Self-attention의 핵심은 문장(context/sentence/paragraph)에서 하나의 단어(word)와 이 외 모든 단어들에 사이에 attention 매커니즘을 적용했다는 점에 있다. (vanilla attention의 경우에는 인코더와 디코더 사이의 attention에만 초점을 두고있다.)
3. Multi-head attention

[참조](https://www.pinecone.io/learn/sentence-embeddings/)


## BERT



# Reference

[wikidocs01](https://wikidocs.net/45609)
