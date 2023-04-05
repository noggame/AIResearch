# AI Research

## Generative AI (생성형 AI)

- 생성형 AI에서 일반적으로 사용하는 방식 중 하나는 GAN(Generative Adversarial Networks)이다.
> GAN : 새로운 Content를 생성하는 생성 네트워크와 Content를 평가해 진위 여부를 경정하는 판별 네트워크로 구성되며, 두 네트워크는 생성된 Content의 품질 향상을 위해 경쟁한다.

- 특징 : PDF파일을 입력받고, 사용자의 질의로부터 필요한 답을 PDF 내용으로부터 찾아 알려준다.
- 기존 AI vs 생성형 AI
||기존 AI|생성형 AI|
|:---:|---|---|
|작동 방식|사전에 정해진 규칙에 따름|데이터 학습 후 생성|
|사용 용도|단순한 문제 해결에 적합|복잡한 문제 해결에 적합|
|예시|체스 게임 인공지능|이미지 생성, 음성 생성, 자연어 처리|
|출력 결과|입력한 데이터에 따라 결과 반환|새로운 데이터 생성 및 출력|
|장점|명확한 규칙으로 신뢰성 높음|다양하고 예측 불가능한 결과 가능|
|단점|데이터가 부족하거나 편향 가능성 있음|모델 설명이 어렵고 생성된 데이터의 정확성 보정 필요|

### Text

1. [LaMDA](https://blog.google/technology/ai/lamda/), [Bard](https://bard.google.com/) (Google)
2. New Bing (MicroSoft) : 질의/검색 서비스로, 전달 정보에 대한 출처를 명시하는 것이 특징이다.
3. ChatGPT (OpenAI) : 질의/검색 서비스로, 생성된 답안의 경우 거짓 정보일 수도 있다.
4. [Chat PDF](https://www.chatpdf.com/) : PDF 대상 질의/검색 서비스


### Image
1. [DALL-E](https://openai.com/product/dall-e-2) (OpenAI) : 이미지 생성 서비스
1. [StyleGAN](https://github.com/NVlabs/stylegan) (NVIDIA) - 얼굴 생성 서비스
1. Adobe : Sensei
1. MidJourney
1. Google(technical) : NeRF, MiP-NeRF


### Music

1. Amper Music
2. Aiva
3. Amadeus Code
4. Google Magenta
5. MuseNet


### Code

1. CodeWhisperer (Amazon)
2. CoPilot (GitHub, MS)
3. Codex


### ETC ??
- [StyleGAN](https://github.com/NVlabs/stylegan), [CycleGAN](https://junyanz.github.io/CycleGAN/), [WaveNet](https://www.deepmind.com/blog/wavenet-a-generative-model-for-raw-audio), Neural Style Transfer


## 참고 (Reference)
- [GAN](https://deepai.org/machine-learning-glossary-and-terms/generative-adversarial-network)
- 
