


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

## Reference

- [gpt4all.git](https://github.com/nomic-ai/gpt4all)
