# 언어모델

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


# Reference

[wikidocs01](https://wikidocs.net/45609)
