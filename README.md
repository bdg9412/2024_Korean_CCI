# 2024_Korean_CCI
본 리포지토리는 [2024년 인공지능(AI)말평 과제 경진대회](https://kli.korean.go.kr/benchmark/taskOrdtm/taskList.do?taskOrdtmId=145&clCd=END_TASK&subMenuId=sub01) 중 [대화 맥락 추론 과제 (나)유형]을 위한 모델 및 해당 모델의 재현을 위한 소스 코드를 포함하고 있습니다.

## Data Format
```
{
    "id": "nikluge-2024-대화 맥락 추론-train-000001",
    "input": {
        "conversation": [
            {
                "speaker": 2,
                "utterance": "진짜 신의 한수",
                "utterance_id": "MDRW2100003410.1.1"
            },
            {
                "speaker": 1,
                "utterance": "이사하자마자 비 많이 와서 베란다 물 많이 새는 거 알았잖아",
                "utterance_id": "MDRW2100003410.1.2"
            },
            {
                "speaker": 2,
                "utterance": "글치 계속 해떴으면 몰랐겠지",
                "utterance_id": "MDRW2100003410.1.3"
            },
            ...
            ...
            ...
        ],
        "reference_id": [
            "MDRW2100003410.1.11"
        ],
        "category": "원인",
        "inference_1": "화자2가 사는 곳 근처에서 베란다 보수 공사가 진행되고 있다.",
        "inference_2": "화자2가 사는 곳 근처에서 싱크홀 보수 공사가 진행되고 있다.",
        "inference_3": "화자2가 사는 곳 근처에서 싱크홀 보수 공사가 중단되었다."
    },
    "output": "inference_2" # The Correct answer is inference_2
}
```
## Key Point
- 최적 모델 선별
- Lora 적용
- 학습 파라미터 수정
- CoT(Chain-of-Thought) 적용
- 앙상블

## Methods
- Model
    - [chihoonlee10/T3Q-ko-solar-dpo-v7.0 (캐글 / T4 x2에서 학습 진행)]  
        - 적절한 맥락을 선택을 유도하는 과제임에 따라 LeaderBoard 시즌 1 모델 중 instruction-tuned된 모델이며 동시에 Average 기준으로 상위 5위 안의 모델 중 선택  
    - [rtzr/ko-gemma-2-9b-it (코랩 / A100에서 학습 진행)]  
        - LogicKor의 한국어 언어모델 다분야 사고력 벤치마크 상위권인 ko-gemma-2-9b-it 모델을 선택. 또한 it의 의미가 instruction tuning 이므로 SFT 최적화 모델임에 따라 선택  
- Data Processing
    - CoT(Chain-of-Thought)를 다양한 방법으로 적용  
- Ensemble
    - Soft Voting 기법 사용
        - Soft Voting을 적용하여 다양한 CoT를 통한 논리적인 데이터 증강이 가능하도록 구현
     
## Directory Structue
```
resource
└── dataset/대화맥락추론_데이터
    ├── 대화맥락추론_dev.json
    ├── 대화맥락추론_test.json
    └── 대화맥락추론_train.json

└── model_path
    ├── results_1
    ├── results_2
    ├── results_3
    └── results_4

└── 2024_삼성역_4번_출구에서_직진출구에서_직진.ipynb
```

## Reference
huggingface/transformers (https://github.com/huggingface/transformers)  
Bllossome (Teddysum) (https://huggingface.co/MLP-KTLim/llama-3-Korean-Bllossom-8B)  
국립국어원 인공지능 (AI)말평 (https://kli.korean.go.kr/benchmark)  
Baseline (Teddysum) (https://github.com/teddysum/Korean_CCI_2024)  
Dailin Li, Chuhan Wang, Xin Zou, Junlong Wang, Peng Chen, Jian Wang, Liang Yang, and Hongfei Lin. 2024. CoT-based Data Augmentation Strategy for Persuasion Techniques Detection. In Proceedings of the 18th International Workshop on Semantic Evaluation (SemEval-2024), pages 1315–1321, Mexico City, Mexico. Association for Computational Linguistics.(https://aclanthology.org/2024.semeval-1.190/)  

## License  
Gemma 2 License: https://ai.google.dev/gemma/terms  
Apache License 2.0  
