
## N-gram 언어모델

어떤 문장의 연속된 n개의 word에서 n번째 나올 수 있는 word를 예측하는 모델이다. 학습은 "(n-1)번째 word까지 입력이 있으면 다음 word로는 n번째 word가 출력된다"라는 구성을 하나의 데이터셋으로 보고 진행되며, 예측은 그 빈도를 측정해 확률적으로 예측한다.

예시) 아래 문장에서 **4-gram 언어 모델**을 사용해 예측하는 경우.

```
"The invariable mark of _____ is to see the miraculous in the common."

예측은 "invariable mark of _____" 부분에서 빈 칸을 채우기 위해 발생하며, 
기존에 학습된 데이터 중에서 "invariable mark of" 문장과 정확히 일치할 때 다음으로 어떤 word가 자주 출력되었는지를 기반으로 예측한다.
```

한계 : `희소 문제 (sparsity problem)` - 학습되지 않은 데이터에 대해서는 예측이 불가능하다. (앞선 예시에서 "invariable mark of"와 일치하는 문장이 학습되지 않은 경우는 빈도수가 0이기 때문에 예측이 불가하다.)


## Neural Network Language Model (NNLM)

<img url=https://wikidocs.net/images/page/45609/nnlm3_renew.PNG widh=300 height=200/>
