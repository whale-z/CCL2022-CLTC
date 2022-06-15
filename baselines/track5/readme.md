# Track5:语法纠错质量评估（Quality Estimation）基线
## 代码结构说明
以下是本项目主要代码结构及说明：
```
├── script # 存放基线脚本
├── bert_model.py # bert模型的相应定义
├── data_loader # Dataset子类，完成数据读取
├── generate_feature.py # 对修改句生成相应质量评估分数
├── models.py # 完成模型结构设计以及前向传播流程
├── results_to_file.py # 将质量评估分数调整成最终提交格式
├── train.py # 完成训练和验证并保存最佳模型
└── README.md # 文档说明
```
### script说明
#### - train.sh
```
export CUDA_VISIBLE_DEVICES="0,1,2,3"
python train.py --outdir ./save_model \
--train_path ./data_qe/train.json \
--valid_path ./data_qe/valid.fluency.json \
--model_name_or_path bert-base-chinese
```
关键参数解析：
- ```CUDA_VISIBLE_DEVICES```：使用的gpu卡号
- ```output```：训练生成的最佳模型保存路径
- ```valid_path```:验证集路径
- ```train_path```:训练集路径
- ```model_name_or_path```:预训练使用的模型


#### - generate_feature.sh
```
python generate_feature.py --test_path ./data_qe/test.phase1.fluency.json \
--out_path ./data_feature/output_phase1_fluency.json \
--model_name_or_path bert-base-chinese \
--checkpoint ./save_model/model.best.pt
```
关键参数解析：
- ```test_path```：测试集路径
- ```output_path```：质量评估分数输出路径
- ```checkpoint```:最佳模型存放路径


#### - results_to_file.sh
```
python results_to_file.py --test_path ./data_qe/test.phase1.fluency.json \
--test_score_path ./data_feature/output_phase1_fluency.json \
--output_path ./output/final_phase1_fluency.json
```
关键参数解析：
- ```test_path```：测试集路径
- ```test_score_path```：质量评估分数输出路径
- ```output_path```:最终提交数据存放路径
