# 超长长上下文场景中LLM自动数据标注挑战赛

---

## 消息
<!-- BEGIN NEWS -->
- **[2026-01-20] `发布`：** 赛事信息已在 **Kaggle** 正式上线。详情见：[FlagOS Open Computing Global Challenge](https://www.kaggle.com/competitions/flag-os-open-computing-global-challenge).
- **[2026-01-06] `发布`：** 由 **众智 FlagOS 社区**、**北京智源人工智能研究院（BAAI）** 与 **CCF ODTC** 联合主办的综合性大赛 **FlagOS 开放计算全球挑战赛** 正式发布。详情见：  
  [FlagOS开放计算全球挑战赛- AI赛事通 | 数据算法赛](https://www.competehub.dev/zh/competitions/modelscope180)
<!-- END NEWS -->

---

## 简介
长上下文场景中LLM自动数据标注挑战赛基于Qwen3-4B大语言模型，采用上下文（In-context Learning, ICL）范式开展自动化数据标注任务研究。参赛团队必须使用组委会统一提供的数据集，围绕超长上下文场景设计有效的 ICL 标注方案，并在统一评测集上完成推理与评测。组委会将依据标准化评测结果，对参赛方案进行综合评估并确定最终排名。

### 赛题目标
本赛题以大语言模型（Large Language Models，LLMs）为核心驱动力，面向超长上下文条件下的自动化数据标注问题，探索兼具效率与精度的新型技术范式。赛题重点聚焦以下三个关键科学与工程问题：
1. 在超长上下文场景下，如何设计有效的模型指令与提示策略，引导 LLM 稳定、高质量地完成数据标注任务？
2. 当可用标注示例数量显著超过模型上下文容量时，如何为待标注数据构造信息密集、结构合理的超长上下文输入？
3. 在自动多轮对话或持续交互场景中，如何高效利用超长上下文，实现一致性与可扩展性兼顾的数据标注？

### 赛题详情
- 参赛团队自主设计一套完整的LLM自动数据标注方案，并在统一的数据集与评测设置下进行实验验证。比赛将以标准化榜单形式公布各参赛方案的评测分数及排名。
- 除评测结果外，参赛团队还需按照赛事要求提交技术方案文档与可复现源代码。组委会将对提交方案进行复现验证，并对技术方案本身进行评审。最终成绩将由预测结果成绩与技术方案成绩加权计算得出，具体规则如下。
- 参赛队伍需按照赛题和赛制要求，提交技术方案和完整代码至Github OpenSeek官方开源项目下。
- 更多具体细节请参考[赛事平台](https://flagos.io/RaceDetail?id=296fmsd8&lang=cn)。
- 关于赛事信息，一切以赛事平台公布信息为准。

---

## 快速开始
### 1. 环境

```bash
openai
torch
flagScale
```

推荐在NVIDIA平台使用 `cd src && bash create_env_nvidia.sh` 创建环境。

### 2. 下载模型权重
```bash
hf download Qwen/Qwen3-4B --local-dir Qwen3-4B
# or
modelscope download --model Qwen/Qwen3-4B 
```
### 3. 长文本配置
在`Qwen3-4B/config.json`将原有配置替换为：
```json
"rope_scaling": {
    "rope_type": "yarn",
    "factor": 4.0,
    "original_max_position_embeddings": 32768
}
```
### 4. 模型部署

请根据实际需求，配置 `llm_config.yaml` 文件。启动配置  

```bash
cd FlagScale
python run.py --config-path .. --config-name llm_config action=run
```

在模型服务启动后，可通过以下方式测试本地 API：

```bash
python api_test.py
```

如需停止服务，请执行：

```bash
python run.py --config-path .. --config-name llm_config action=stop
```

### 5. 运行/改进基线方法（Baseline）

启动如下命令开始模型标注
```bash
python main.py
```

实现新的标注方法，请修改`method.py`文件。你可以在该文件中：  
* 定义新的指令模板、
* 定义新的上下文示例选择策略
* 定义新的模型推理、标注方案
* 添加自定义后处理逻辑
