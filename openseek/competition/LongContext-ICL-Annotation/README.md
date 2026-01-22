# LongContext-ICL-Annotation

Large Language Models Automatic Data Annotation under Long-Context Scenarios.

## News
<!-- BEGIN NEWS -->
- **[2026-01-20] `Release`:** The competition is now officially live on **Kaggle**. See details: [FlagOS Open Computing Global Challenge](https://www.kaggle.com/competitions/flag-os-open-computing-global-challenge).
- **[2026-01-06] `Release`:** The comprehensive competition **FlagOS Open Computing Global Challenge** was officially announced, co-hosted by the **FlagOS Community**, the **Beijing Academy of Artificial Intelligence (BAAI)**, and **CCF ODTC**. See details:  
  [FlagOS开放计算全球挑战赛- AI赛事通 | 数据算法赛](https://www.competehub.dev/zh/competitions/modelscope180)
<!-- END NEWS -->

## Introduction

The LongContext-ICL-Annotation Challenge focuses on automatic data annotation under long-context settings using Large Language Models (LLMs). The competition is built upon the Qwen3-4B model and adopts the In-context Learning (ICL) paradigm to investigate scalable and high-quality automated annotation methods.

Participating teams are required to use the officially provided datasets and design effective ICL-based annotation solutions tailored for ultra-long context scenarios. All submissions will be evaluated on a unified benchmark dataset. The Organizing Committee will conduct standardized evaluations and determine the final rankings based on the official evaluation results.

## Objectives

This challenge takes Large Language Models (LLMs) as the core technical foundation and targets automated data annotation under ultra-long context constraints, aiming to explore novel paradigms that balance annotation efficiency and annotation accuracy. The competition focuses on the following key scientific and engineering challenges:

- 1. Instruction and Prompt Design:

    How can effective model instructions and prompt strategies be designed in ultra-long context scenarios to guide LLMs toward stable and high-quality data annotation?
- 2. Ultra-Long Context Construction:
    
    When the number of available annotation examples significantly exceeds the model’s context capacity, how can information-dense and structurally coherent ultra-long context inputs be constructed for target data annotation?
- 3. Multi-Turn and Continuous Annotation:

    In automated multi-round dialogue or continuous interaction settings, how can ultra-long contexts be efficiently leveraged to achieve both consistency and scalability in data annotation?

## Challenge Details

- Participating teams are expected to independently design a complete LLM-based automatic data annotation pipeline and validate their approach under a unified dataset and evaluation protocol. Evaluation scores and rankings will be published on a standardized leaderboard.

- In addition to prediction results, teams must submit a technical report and fully reproducible source code in accordance with the competition requirements. The Organizing Committee will reproduce submitted solutions and review the technical design. The final score will be calculated as a weighted combination of prediction performance and technical solution evaluation, with detailed rules specified by the competition.

- Teams are required to submit their technical reports and complete source code to the official OpenSeek GitHub repository designated by the competition.

- For additional details, please refer to [FlagOS platform](https://flagos.io/RaceDetail?id=296fmsd8&lang=en). All competition-related information is subject to the announcements published on the official platform.

## Quick Start

### 1. Environment Setup

```bash
openai
torch
flagScale
```

On NVIDIA platforms, it is recommended to create the environment using: `cd src && bash create_env_nvidia.sh`

### 2. Download Model Weights

```bash
hf download Qwen/Qwen3-4B --local-dir Qwen3-4B
# or
modelscope download --model Qwen/Qwen3-4B
```

### 3. Long-Context Configuration

In `Qwen3-4B/config.json`, replace the original configuration with the following settings:

```json
"rope_scaling": {
    "rope_type": "yarn",
    "factor": 4.0,
    "original_max_position_embeddings": 32768
}
```

### 4. Model Deployment

Configure the `llm_config.yaml` file according to your actual requirements. Then start the service with:

```bash
cd FlagScale
python run.py --config-path .. --config-name llm_config action=run
```

After the model service is launched, you can test the local API using:

```bash
python api_test.py
```

To stop the service, run:

```bash
python run.py --config-path .. --config-name llm_config action=stop
```

### 5. Run or Extend the Baseline Method

Start the baseline annotation pipeline with:

```bash
python main.py
```

To implement a new annotation method, modify the `method.py` file. Within this file, you may:

- Define new instruction or prompt templates
- Design new context example selection strategies
- Implement alternative model inference and annotation pipelines
- Add custom post-processing logic
