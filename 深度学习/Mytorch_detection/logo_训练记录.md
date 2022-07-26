# logo_训练记录

## logo数据集：

train：50类 x 10张

val：50类 x 40张(约)

## 网络训练

| net             | backbone   | neck       | head                    | loss(cls+bbox) |
| --------------- | ---------- | ---------- | ----------------------- | -------------- |
| Faster-RCNN(改) | resnet50   | fpn        | RPN+align+Shared2FCHead | CE+L1          |
| YOLOX-s         | CSPDarknet | YOLOXPAFPN | YOLOXHead               |                |
|                 |            |            |                         |                |

### 主要参数

| net             | epoch | bachsize | img_size | lr   |      |
| --------------- | ----- | -------- | -------- | ---- | ---- |
| Faster-RCNN(改) | 50    | 16       | 640，640 | 0.01 |      |
| YOLOX-s         | 50    | 8        | 640，640 | 0.01 |      |
|                 |       |          |          |      |      |

### 训练结果

| net                 | eval | mAP   | mAP.5 | mAP.75 | mAP_s | mAP_m | mAP_l |
| ------------------- | ---- | ----- | ----- | ------ | ----- | ----- | ----- |
| YOLOX-s             | val  | 0.151 | 0.232 | 0.176  | 0.015 | 0.110 | 0.227 |
| YOLOX-s             | test |       |       |        |       |       |       |
| YOLOX-s+aug         | val  | 0.341 | 0.475 | 0.382  | 0.121 | 0.332 | 0.434 |
| YOLOX-s+aug         | test | 0.323 | 0.453 | 0.364  | 0.114 | 0.287 | 0.403 |
| Faster-RCNN(改)     | val  | 0.267 | 0.416 | 0.284  | 0.039 | 0.264 | 0.356 |
| Faster-RCNN(改)+aug | val  | 0.300 | 0.461 | 0.333  | 0.057 | 0.281 | 0.412 |
|                     |      |       |       |        |       |       |       |



### Faster-RCNN

#### 训练策略：

warmup：0.001，100iters

学习率策略：step 36，48轮 缩小为十分之一
