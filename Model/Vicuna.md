
# [Vicuna](https://github.com/lm-sys/FastChat)

## 배경

UC Berkeley, CMU, Stanford, UC San Diego 에서 기존 LLM에 존재하는 훈련 부족과 구조상 문제를 해결하기 위해 개발되었다.


## 구조

Vicuna-13B는 ShareGPT.com의 7만건의 공유 대화 데이터를 사용해 fine-tuning을 진행
> ShareGPT.com : ChatGPT 대화를 사용자들간에 공유하는 사이트

PyTorch FSDP와 A100 GPU 8대를 하루동안 사용해 학습

### 최적화

- Memory optimizations : 기존 alpaca에서 처리 가능했던 512의 문맥길이(~Token) 제한을 2048로 확장
- Multi-round conversation : 이어지는(multi-round) 대화에 답학 위해 훈련 손실값으 조정하고, 오직 챗봇의 출력값에 대해서만 fine-tuning 손실값을 계산
- Cost reduction via Spot Instance : 40배 많은 데이터셋과 4배 긴 문장의 훈련을 위한 비용이 도전 과제였으며, 이를 줄이 위해 SkyPilot으로부터 고용된 팀은 효율대비 싸게 먹히는 지점을 찾는 역할을 수행

## 평가

GPT-4 기반으로 챗봇 성능측정을 자동화학 위한 평가 프레임워크 제안

### 평가 절차
1. 프레임워크를 총 8개의 카테고리로 구성 (페르미 문제, 역할 시나리오, 코딩/수학 등이 포함)
2. 카테고리별 10문제씩 선별 후, 5개의 챗봇으로부터 결과를 수집 (LLaMA, Alpaca, ChatGPT, Bard, Vicuna)
3. 수집된 결과는 '유용함', '관련성', '정확도', '세부성'을 기반으로 GPT-4 에게 팻봇의 성능을 평가하도록 질의
4. 평가된 점수 및 세부 설명을 참조한 전체 평가
  > 코딩/수학계산 카테고리에 대한 평가 신뢰도는 낮음

### 결과

<img src="https://miro.medium.com/v2/resize:fit:720/format:webp/0*qTrWfPbqFB9sijxy.png" width="500">


## Reference
- [Article_01](https://pub.towardsai.net/meet-vicuna-the-latest-metas-llama-model-that-matches-chatgpt-performance-e23b2fc67e6b)

---

# Training

## [Setup](https://github.com/nomic-ai/gpt4all#setup)

### Clone the Repo

```
git clone --recurse-submodules https://github.com/nomic-ai/gpt4all.git
git submodule update --init
```

### Setup the environment

```
python -m pip install -r requirements.txt

cd transformers
pip install -e . 

cd ../peft
pip install -e .
```

### Training

```
accelerate launch --dynamo_backend=inductor --num_processes=8 --num_machines=1 --machine_rank=0 --deepspeed_multinode_launcher standard --mixed_precision=bf16  --use_deepspeed --deepspeed_config_file=configs/deepspeed/ds_config.json train.py --config configs/train/finetune.yaml
```

### Generate

```
python generate.py --config configs/generate/generate.yaml --prompt "Write a script to reverse a string in Python"
```

## 조건별 코드 수정

<details>
  <summary>Set Model & Tokenizer </summary>

  환경파일 설정
  
  ``` INI
  # configs/train/finetune.yaml

  model_name: "decapoda-research/llama-7b-hf"
  tokenizer_name: "decapoda-research/llama-7b-hf"
  ```
  
  [Model & Tokenizer 개체 생성](https://huggingface.co/docs/transformers/quicktour#use-another-model-and-tokenizer-in-the-pipeline)
  
  ``` Python
  from transformers import AutoTokenizer, AutoModelForSequenceClassification

  model = AutoModelForSequenceClassification.from_pretrained(model_name)
  tokenizer = AutoTokenizer.from_pretrained(model_name)
  ```
  
</details>

<details>
  <summary>wandb 미사용</summary>

  환경파일 설정
  
  ``` INI
  # configs/train/finetune.yaml
  
  wandb: false
  ```
  
</details>

<details>
  <summary>cpu 사용</summary>
  
  ``` Python
  # train.py

  if __name__ == "__main__":
    # ...
    if config["wandb"]:
      # ...
    else:
      accelerator = Accelerator(cpu=True)

    train(accelerator, config=config)
  ```
  
</details>

## Error Handling

<details>
  <summary>Can't load tokenizer using from_pretrained, please update its configuration: Tokenizer class LLaMATokenizer does not exist or is not currently imported.</summary>
  
  LLaMA에서 사용중이 Tokenizer를 찾지 못해 발생하는 문제로 현재 사용중인 LLaMA Tokenizer를 명시적으로 기입 [참고](https://huggingface.co/docs/transformers/main/en/model_doc/llama)
  
  ``` Python
  # transformers/src/transformers/models/auto/tokenization_auto.py
  # ~line 625
    
        # using "LlamaTokenizer" Not "LLaMATokenizer"
        config_tokenizer_class = "LlamaTokenizer"   ### FIXME: Assign tokenizer directly
  
        tokenizer_auto_map = None
        if "auto_map" in tokenizer_config:
            if isinstance(tokenizer_config["auto_map"], (tuple, list)):
                # Legacy format for dynamic tokenizers
                tokenizer_auto_map = tokenizer_config["auto_map"]
            else:
                tokenizer_auto_map = tokenizer_config["auto_map"].get("AutoTokenizer", None)
  
  ```

</details>


모델 및 토크나이저 불러오기 : https://huggingface.co/docs/transformers/quicktour#use-another-model-and-tokenizer-in-the-pipeline
llama-7b-hf 모델명 : https://huggingface.co/decapoda-research/llama-7b-hf


## Reference
- [gpt4all.git](https://github.com/nomic-ai/gpt4all)
