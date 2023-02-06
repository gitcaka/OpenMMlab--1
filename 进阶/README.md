# 进阶作业
### 作业内容
    🚥图像分类数据集：CIFAR-10
### 完成情况
    configs是配置文件，work是训练好的模型文件。
    精度达到94%
# 进阶作业cifar10教程
1. 下载 cifar-10-python.tar.gz 数据集  
https://opendatalab.com/CIFAR-10/download
2. 下载 resnet50 预训练模型，命令行输入
```
wget https://download.openmmlab.com/mmclassification/v0/resnet/resnet50_b16x8_cifar10_20210528-f54bfad9.pth -P checkpoints
```
3. 编写配置文件 cifar10.py  (名字自取)
```
_base_ = [
    '../_base_/models/resnet50_cifar.py', '../_base_/datasets/cifar10_bs16.py',
    '../_base_/schedules/cifar10_bs128.py', '../_base_/default_runtime.py'
]
data = dict(
    samples_per_gpu = 64,
    workers_per_gpu=2,
    )
model = dict(
    head=dict(
        topk=(1, ),
    ))
optimizer = dict(type='SGD', lr=0.01, momentum=0.9, weight_decay=4e-5)
lr_config = dict(policy='step', step=[4, 8])
runner = dict(type='EpochBasedRunner', max_epochs=10)
checkpoint_config = dict(interval=10)
evaluation = dict(
    interval=1,
    metric=['accuracy', 'precision', 'f1_score'],
    metric_options=dict(topk=(1, )))

load_from = 'checkpoints/resnet50_b16x8_cifar10_20210528-f54bfad9.pth'
```
4. 开始训练代码  
超级云：修改 run.sh 文件  
本地：直接运行
```
python tools/train.py configs/自己的目录/cifar10.py --work-dir work/CIFAR-10
```