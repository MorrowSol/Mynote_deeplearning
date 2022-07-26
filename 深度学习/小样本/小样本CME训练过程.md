# CME训练过程

### 配置文件

- datacfg  存放数据配置 cfg/fewshot/metayolo_split1.data 

  ```
  metayolo=1
  metain_type=2
  data=voc
  neg = 1
  rand = 0
  novel = data/voc_novels_split1.txt
  novelid = 0
  # scale = 0
  meta = data/voc_traindict_full.txt
  train = /home/ubuntu/Dataset/VOC_Fewshot_Detection/voc_train.txt
  valid = /home/ubuntu/Dataset/VOC_Fewshot_Detection/2007_test.txt
  backup = backup/metayolo
  gpus=0,1
  ```

- darknetcfg  主干网络配置 cfg/darknet_dynamic.cfg 

- learnetcfg   副网络配置 cfg/reweighting_net_decoupling.cfg

### 训练集

- train_loader  输出 (img1, label)

  - trainlist 根据datacfg文件
    - Base training dataset：dataopt['train']
    
      每张图片有几个标签，类别不属于基类的不要，太小的目标不要，太多标签不要
    
    - Meta tuning dataset：dataopt['meta']
    
      

- metaloader   输出 (img2, mask, clsid)

  - metadict data_options['meta']文件

    data/voc_traindict_full.txt

    ```
    aeroplane /home/ubuntu/Dataset/VOC_Fewshot_Detection/voclist/aeroplane_train.txt
    bicycle /home/ubuntu/Dataset/VOC_Fewshot_Detection/voclist/bicycle_train.txt
    bird /home/ubuntu/Dataset/VOC_Fewshot_Detection/voclist/bird_train.txt
    boat /home/ubuntu/Dataset/VOC_Fewshot_Detection/voclist/boat_train.txt
    ```

    

网络输入: img1, img2, mask

网络输出: output, dynamic_weights

### 训练过程(Base training)

根据划分数据集得到base_cls和novel_cls 对basecls进行预训练

#### 训练

每次取一批train_loader  和 metaloader  放入网络

**train_loader**：大数据集img voc 07+12中基类每张图片和标签  14554张

图片: (8, 3, 416, 416) label: (8, 15, 250)

**metaloader**：在**基类**每类取1shot 共15张  每类4000张的数据集（含重复）

metaimg:(15, 3, 416, 416)  metamask:(15, 1, 416, 416) clsid (15) 

#### 进网络后输出

output: (120, 30, 13, 13)  (batch* cls, anchor *(box+cls) , w, h)

dynamic_weights: (15, 512, 1, 1)   (15类, 512维, 1, 1)

target: (8, 15, 250)   (batch, 15类, 每类最多50张*box)

#### 测试

test_loader: 大数据集voc 07test中基类每张图片和标签 共4952

test_metaloader: 在**基类**每类取1shot 共15张  每类4000张的数据集（含重复）

### 训练过程(Meta tuning) 以2shot为例 

#### 训练

**train_loader**：选取了6000张作为训练集  15类每类2shot张 乘以重复次数200 共6000

图片: (8, 3, 416, 416) label: (8, 20, 250)

**metaloader**：在**基类和新类**每类取2shot 共40张 每类取4000张 

metaimg:(20, 3, 416, 416)  metamask:(20, 1, 416, 416) clsid (20) 

#### 测试

test_loader: 大数据集voc 07test中基类每张图片和标签 共4952

test_metaloader: 在**基类和新类**每类取2shot 共40张 每类取4000张 

# DeFRCN-训练过程
