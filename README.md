
# 基于BERT的NLU联合模型

## 概述

本项目实现了一个基于BERT架构的单模型，用于处理对话系统中的NLU（自然语言理解）任务。该模型旨在替代RASA NLU，提供更加精准和高效的意图识别和实体识别。此外，模型还能应用于关系抽取和事件抽取任务，如事件类型、角色和论元的识别。

## 模型原理

### 意图识别

对于意图识别，模型采用了`bert + poolout + softmax`的架构。其中：

- **BERT**：BERT作为基础模型，用于提取句子的深层次语义特征。
- **PoolOut**：在BERT的输出上应用池化操作，以获得固定长度的特征向量。
- **Softmax**：最后通过Softmax层进行分类，得出不同意图的概率分布。

### 实体识别

实体识别部分采用了`bert + CRF (条件随机场)`的架构。具体步骤如下：

- **BERT**：同样使用BERT提取特征。
- **CRF**：在BERT输出的基础上，应用CRF层进行序列标注。模型首先识别出BIO（Begin, Inside, Outside）标记，然后基于这些标记识别出对应的实体类别。
### 联合损失函数

Loss_total = Loss_i +Loss_e

### 对抗训练

为了提高模型的鲁棒性和泛化能力，特别加入了对抗训练。对抗训练通过引入扰动，使模型学习到更加泛化的特征表示。

## 数据格式

本模型采用类似RASA的数据格式，分别针对意图和实体识别构建数据集。以下是数据示例：

```yaml
nlu:
  - intent: auto/openapp
    examples: |
      - [打开](open)[百度地图](app)
      - [使用](open)[百度地图](app)
  - intent: auto/navigation
    examples: |
      - [导航去](navigate)[新浪总部](destination)
      - [开车去](navigate)[工体](destination)
```

## 环境依赖

- bert4torch==0.4.0
- torch==2.1.0

## 训练模型

运行以下命令进行训练：

```bash
python nlu_main.py train
```

## 推理

运行以下命令进行推理：

```bash
python nlu_main.py
```

## 参考资料

- [BERT4Torch实现序列标注和CRF](https://github.com/Tongjilibo/bert4torch/blob/master/examples/sequence_labeling/task_sequence_labeling_ner_cascade_crf.py)
- [A survey of joint intent detection and slot-filling models in natural language understanding
](https://arxiv.org/abs/2101.08091)

---

