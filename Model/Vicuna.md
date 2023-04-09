[original article](https://pub.towardsai.net/meet-vicuna-the-latest-metas-llama-model-that-matches-chatgpt-performance-e23b2fc67e6b)


## 배경

UC Berkeley, CMU, Stanford, UC San Diego 에서 기조 LLMㅇ 존재하는 훈련 부족와 구조상 문제를 해결하기 위해 개발되었다.


## 구조
Vicuna-13B는 ShareGPT.com의 7만건의 공유 대화 데이터를 사용해 fine-tuning을 진행했다.
> ShareGPT.com : ChatGPT 대화를 사용자들간에 공유하는 사이트

- Memory optimizations : 기존 alpaca에서 처리 가능했던 512의 문맥길이(~Token) 제한을 2048로 확장
- Multi-round conversation : 이어지는(multi-round) 대화에 답학 위해 훈련 손실값으 조정하고, 오직 챗봇의 출력값에 대해서만 fine-tuning 손실값을 계산
- Cost reduction via Spot Instance : 40배 많은 데이터셋과 4배 긴 문장의 훈련을 위한 비용이 도전 과제였으며, 이를 줄이 위해 SkyPilot으로부터 고용된 팀은 효율대비 싸게 먹히는 지점을 찾는 역할을 수행

## 평가
