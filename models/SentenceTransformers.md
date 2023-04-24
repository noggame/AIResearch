
트랜스포머는 아래 3개의 주요 요소들을 통해 RNN을 대체할 수 있었다.

1. Positional Encoding : 위치 기반으로 각 입력 embedding에 대한 다양한 활성화 사인파(sine wave)들을 추가해, NLP에서 RNNs를 사용함으로써 얻을 수 있는 sequence내 순서를 고려하는 능력을 대체
2. Self-attention : Self-attention의 핵심은 문장(context/sentence/paragraph)에서 하나의 단어(word)와 이 외 모든 단어들에 사이에 attention 매커니즘을 적용했다는 점에 있다. (vanilla attention의 경우에는 인코더와 디코더 사이의 attention에만 초점을 두고있다.)
3. Multi-head attention
  






[참조](https://www.pinecone.io/learn/sentence-embeddings/)
