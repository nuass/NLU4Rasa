### NLU 模型

基于BERT架构的单模型，替代RASA中的NLU模型，包含意图识别、实体识别、

### 数据
采用类似RASA的格式，将意图和实体识别分开，实体识别的格式：
nlu:
  - intent: auto/openapp
    examples: |
      - [打开](open)[百度地图](app)
      - [使用](open)[百度地图](app)
  - intent: auto/navigation
    examples: |
      - [导航去](navigate)[新浪总部](destination)
      - [开车去](navigate)[工体](destination)

### 训练

```
python nlu_main.py train 
```

### 推理


```
python nlu_main.py
```