# AI Research

## Generative AI (생성형 AI)

- 생성형 AI에서는 GAN이나 Transformer 기반 모델이 주로 사용된다.
> GAN (Generative Adversarial Networks) : 새로운 Content를 생성하는 생성 네트워크와 Content를 평가해 진위 여부를 경정하는 판별 네트워크로 구성되며, 두 네트워크는 생성된 Content의 품질 향상을 위해 경쟁한다.

> Transformer : 문장의 단어오 같은 연속된 데이터간으 관계를 추적해 그 의미를 파악하고 학습한다. (GPT3, LaMDA)

- 기존 AI vs 생성형 AI

||기존 AI|생성형 AI|
|:---:|---|---|
|작동 방식|사전에 정해진 규칙에 따름|데이터 학습 후 생성|
|사용 용도|단순한 문제 해결에 적합|복잡한 문제 해결에 적합|
|예시|체스 게임 인공지능|이미지 생성, 음성 생성, 자연어 처리|
|출력 결과|입력한 데이터에 따라 결과 반환|새로운 데이터 생성 및 출력|
|장점|명확한 규칙으로 신뢰성 높음|다양하고 예측 불가능한 결과 가능|
|단점|데이터가 부족하거나 편향 가능성 있음|모델 설명이 어렵고 생성된 데이터의 정확성 보정 필요|


## Model

1. GPT-3 : OpenAI 팀에서만든 딥러닝 모델중 하나로, 생성형으로 미리 학습된(pre-trained) Transformer 모델에 대한 표준이다.
1. LaMDA (Language Model for Dialogue Applications) : Google Transformer에서 만든 대화형 뉴럴 언어 모델이다.
1. [Alpaca](https://github.com/alpacahq)
1. [GPT4ALL](https://github.com/nomic-ai/gpt4all?ref=producthunt) ([강화학습 방법](./models/GPT4ALL.md))
1. [Vicuna 정리](./models/Vicuna.md)
1. [LlaMa2](https://ai.meta.com/llama/)
1. [Mistral](https://mistral.ai/news/announcing-mistral-7b/)

## 적용 예

### Text

1. [ChatGPT](https://openai.com/blog/chatgpt) (OpenAI) : 질의/검색 서비스로, 생성된 답안의 경우 거짓 정보일 수도 있다.
1. [New Bing](https://www.bing.com/new) (MicroSoft) : 질의/검색 서비스로, 전달 정보에 대한 출처를 명시하는 것이 특징이다.
1. [LaMDA](https://blog.google/technology/ai/lamda/), [Bard](https://bard.google.com/) (Google)
1. [Chat PDF](https://www.chatpdf.com/) : PDF 대상 질의/검색
1. [Generative AI App Builder](https://cloud.google.com/blog/products/ai-machine-learning/create-generative-apps-in-minutes-with-gen-app-builder?hl=en) (Google) : 웹URL 또는 PDF 파일을 입력받아 학습 후, 해당 모델을 서비스할 수 있도록 제공 ([관련영상](https://www.youtube.com/watch?v=0vM5UWC5crs))


### Image
1. [DALL-E](https://openai.com/product/dall-e-2) (OpenAI) : 이미지 생성
1. [MidJourney](https://www.midjourney.com/home/?callbackUrl=%2Fapp%2F) : Text to Image 생성
1. [StyleGAN](https://github.com/NVlabs/stylegan) (NVIDIA) - 얼굴 생성
1. [Sensei](https://www.adobe.com/sensei.html) (Adobe) : 이미지 및 영상처리 지원 서비스 ([관련영상](https://www.youtube.com/watch?v=YwhrzlXB1hs))
1. Google(technical) : NeRF, MiP-NeRF


### Music

1. [Amper Music](https://www.audoir.com/ampermusic) : 음악 생성 및 지원
1. [Aiva](https://www.aiva.ai/) : 감성적인 음악/효과음 생성
1. [Amadeus Code](https://amadeuscode.com/app/en#page-block-esxgnavp2gh) : 멜로디 및 작곡
1. [Google Magenta](https://magenta.tensorflow.org/) : 음악 생성 및 지원
1. [MuseNet](https://openai.com/research/musenet) (OpenAI) : 음악을 이해한다기보다는 화성, 리듬, 스타일 등을 고려해 다음 나올 음(~Token)을 예상하는 방식으로 음악 생성 지원


### Code

1. [CoPilot](https://github.com/features/copilot) (GitHub) : OpenAI Codex를 사용해 코드를 제안
1. [CodeWhisperer](https://aws.amazon.com/ko/codewhisperer/) (Amazon) : 주석을 기반으로 코드 제안
1. [Replit](https://replit.com/) (with Google) : 코드생성 및 Debug 등 개발 관련으로 다양한 정보를 제공하는 AI IDE



### ETC ??

- [StyleGAN](https://github.com/NVlabs/stylegan), [CycleGAN](https://junyanz.github.io/CycleGAN/), [WaveNet](https://www.deepmind.com/blog/wavenet-a-generative-model-for-raw-audio), Neural Style Transfer

## 참고 (Reference)

- [GAN](https://deepai.org/machine-learning-glossary-and-terms/generative-adversarial-network)
- [GenerativeAI-Article-01](https://www.altexsoft.com/blog/generative-ai/)
