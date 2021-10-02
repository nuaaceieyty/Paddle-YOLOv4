# Paddle-YOLOv4

PaddlePaddle_v2.1 复现YOLOv4

## 代码结构

1. 本项目基于PaddleDetection开源项目，实现了YOLOv4的复现任务。
2. 其中将YOLOv2模型分为了backbone部分、neck部分、head部分。backbone部分为cspdarknet，代码在ppdet/modeling/backbones/CSPDarkNet.py中；neck部分代码在ppdet/modeling/necks/yolov4_neck.py中；head部分代码在ppdet/modeling/heads中。
3. YOLOv4训练配置文件在configs/yolov4中。

## 开始训练

1. cd进本项目目录。
2. pip install -r requirements.txt
3. 在顶层目录下创建output文件夹，并在此处存放主干网络cspdarknet的预训练权重（我已经将官方提供的转为了pdparams格式），地址为：https://aistudio.baidu.com/aistudio/datasetdetail/103994 。
4. 本项目使用四卡Tesla V100-32G即可训练，注意：coco数据集应该提前下好，并且解压到顶层目录下（数据集地址为：https://aistudio.baidu.com/aistudio/datasetdetail/7122 ）。如果出现数据集地址问题，请在configs/datasets/coco_detection.yml文件中将相应地址改为绝对路径。
5. python -m paddle.distributed.launch --gpus 0,1,2,3 tools/train.py -c configs/yolov4/yolov4_coco.yml --eval
6. 至此训练开始。

## 评估

1. 如果您不想自己训练，可以在顶层文件下创建output/yolov2_voc文件夹，并下载我训练好的权重数据到此文件夹中（地址为：https://aistudio.baidu.com/aistudio/datasetdetail/107066 ），然后运行第2步中命令。
2. python tools/eval.py -c configs/yolov4/yolov4_coco_test.yml （使用上述权重即可在顶层目录下得到结果bbox.json，将此结果提交至测评服务器，可得在测试集上结果为41.2%）
从测评服务器件上下载的结果见：[提交结果](./stdout.txt)

(附注：训练日志也在网址：https://aistudio.baidu.com/aistudio/datasetdetail/107066，此为AI Studio脚本任务生成的日志)
(由于AI Studio脚本任务的时长限制，三天后任务自动终止并得到日志trainer-0.log.txt，我只好接着断点再进行训练并得到日志trainer-1.log.txt)
