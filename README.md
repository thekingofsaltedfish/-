```python
# 查看当前挂载的数据集目录, 该目录下的变更重启环境后会自动还原
# View dataset directory. 
# This directory will be recovered automatically after resetting environment. 
!ls /home/aistudio/data
```


```python
# 查看工作区文件, 该目录下的变更将会持久保存. 请及时清理不必要的文件, 避免加载过慢.
# View personal work directory. 
# All changes under this directory will be kept even after reset. 
# Please clean unnecessary files in time to speed up environment loading. 
!ls /home/aistudio/work
```


```python
# 如果需要进行持久化安装, 需要使用持久化路径, 如下方代码示例:
# If a persistence installation is required, 
# you need to use the persistence path as the following: 
!mkdir /home/aistudio/external-libraries
!pip install beautifulsoup4 -t /home/aistudio/external-libraries
```


```python
# 同时添加如下代码, 这样每次环境(kernel)启动的时候只要运行下方代码即可: 
# Also add the following code, 
# so that every time the environment (kernel) starts, 
# just run the following code: 
import sys 
sys.path.append('/home/aistudio/external-libraries')
```

请点击[此处](https://ai.baidu.com/docs#/AIStudio_Project_Notebook/a38e5576)查看本环境基本用法.  <br>
Please click [here ](https://ai.baidu.com/docs#/AIStudio_Project_Notebook/a38e5576) for more detailed instructions. 

# 一、项目背景
	为了快速入门飞桨和医疗图像而进行的项目，使用的数据集是患者肺部CT图像，用于诊断新冠肺炎。

##   1.安装paddleX


```python
!pip install paddlex -i https://mirror.baidu.com/pypi/simple
```

    Looking in indexes: https://mirror.baidu.com/pypi/simple
    Collecting paddlex
    [?25l  Downloading https://mirror.baidu.com/pypi/packages/d6/a2/07435f4aa1e51fe22bdf06c95d03bf1b78b7bc6625adbb51e35dc0804cc7/paddlex-1.3.11-py3-none-any.whl (516kB)
    [K     |████████████████████████████████| 522kB 15.0MB/s eta 0:00:01
    [?25hCollecting xlwt (from paddlex)
    [?25l  Downloading https://mirror.baidu.com/pypi/packages/44/48/def306413b25c3d01753603b1a222a011b8621aed27cd7f89cbc27e6b0f4/xlwt-1.3.0-py2.py3-none-any.whl (99kB)
    [K     |████████████████████████████████| 102kB 8.6MB/s eta 0:00:01
    [?25hCollecting pycocotools; platform_system != "Windows" (from paddlex)
      Downloading https://mirror.baidu.com/pypi/packages/de/df/056875d697c45182ed6d2ae21f62015896fdb841906fe48e7268e791c467/pycocotools-2.0.2.tar.gz
    Requirement already satisfied: colorama in /opt/conda/envs/python35-paddle120-env/lib/python3.7/site-packages (from paddlex) (0.4.4)
    Requirement already satisfied: visualdl>=2.0.0 in /opt/conda/envs/python35-paddle120-env/lib/python3.7/site-packages (from paddlex) (2.2.0)
    Collecting shapely>=1.7.0 (from paddlex)
    [?25l  Downloading https://mirror.baidu.com/pypi/packages/98/f8/db4d3426a1aba9d5dfcc83ed5a3e2935d2b1deb73d350642931791a61c37/Shapely-1.7.1-cp37-cp37m-manylinux1_x86_64.whl (1.0MB)
    [K     |████████████████████████████████| 1.0MB 16.5MB/s eta 0:00:01
    [?25hRequirement already satisfied: flask-cors in /opt/conda/envs/python35-paddle120-env/lib/python3.7/site-packages (from paddlex) (3.0.8)
    Requirement already satisfied: opencv-python in /opt/conda/envs/python35-paddle120-env/lib/python3.7/site-packages (from paddlex) (4.1.1.26)
    Requirement already satisfied: tqdm in /opt/conda/envs/python35-paddle120-env/lib/python3.7/site-packages (from paddlex) (4.36.1)
    Collecting paddleslim==1.1.1 (from paddlex)
    [?25l  Downloading https://mirror.baidu.com/pypi/packages/d1/77/e257227bed9a70ff0d35a4a3c4e70ac2d2362c803834c4c52018f7c4b762/paddleslim-1.1.1-py2.py3-none-any.whl (145kB)
    [K     |████████████████████████████████| 153kB 66.6MB/s eta 0:00:01
    [?25hRequirement already satisfied: pyyaml in /opt/conda/envs/python35-paddle120-env/lib/python3.7/site-packages (from paddlex) (5.1.2)
    Requirement already satisfied: psutil in /opt/conda/envs/python35-paddle120-env/lib/python3.7/site-packages (from paddlex) (5.7.2)
    Requirement already satisfied: sklearn in /opt/conda/envs/python35-paddle120-env/lib/python3.7/site-packages (from paddlex) (0.0)
    Collecting paddlehub==2.1.0 (from paddlex)
    [?25l  Downloading https://mirror.baidu.com/pypi/packages/7a/29/3bd0ca43c787181e9c22fe44b944b64d7fcb14ce66d3bf4602d9ad2ac76c/paddlehub-2.1.0-py3-none-any.whl (211kB)
    [K     |████████████████████████████████| 215kB 68.8MB/s eta 0:00:01
    [?25hRequirement already satisfied: setuptools>=18.0 in /opt/conda/envs/python35-paddle120-env/lib/python3.7/site-packages (from pycocotools; platform_system != "Windows"->paddlex) (41.4.0)
    Requirement already satisfied: cython>=0.27.3 in /opt/conda/envs/python35-paddle120-env/lib/python3.7/site-packages (from pycocotools; platform_system != "Windows"->paddlex) (0.29)
    Requirement already satisfied: matplotlib>=2.1.0 in /opt/conda/envs/python35-paddle120-env/lib/python3.7/site-packages (from pycocotools; platform_system != "Windows"->paddlex) (2.2.3)
    Requirement already satisfied: flask>=1.1.1 in /opt/conda/envs/python35-paddle120-env/lib/python3.7/site-packages (from visualdl>=2.0.0->paddlex) (1.1.1)
    Requirement already satisfied: bce-python-sdk in /opt/conda/envs/python35-paddle120-env/lib/python3.7/site-packages (from visualdl>=2.0.0->paddlex) (0.8.53)
    Requirement already satisfied: numpy in /opt/conda/envs/python35-paddle120-env/lib/python3.7/site-packages (from visualdl>=2.0.0->paddlex) (1.16.4)
    Requirement already satisfied: pre-commit in /opt/conda/envs/python35-paddle120-env/lib/python3.7/site-packages (from visualdl>=2.0.0->paddlex) (1.21.0)
    Requirement already satisfied: requests in /opt/conda/envs/python35-paddle120-env/lib/python3.7/site-packages (from visualdl>=2.0.0->paddlex) (2.22.0)
    Requirement already satisfied: Pillow>=7.0.0 in /opt/conda/envs/python35-paddle120-env/lib/python3.7/site-packages (from visualdl>=2.0.0->paddlex) (7.1.2)
    Requirement already satisfied: shellcheck-py in /opt/conda/envs/python35-paddle120-env/lib/python3.7/site-packages (from visualdl>=2.0.0->paddlex) (0.7.1.1)
    Requirement already satisfied: six>=1.14.0 in /opt/conda/envs/python35-paddle120-env/lib/python3.7/site-packages (from visualdl>=2.0.0->paddlex) (1.15.0)
    Requirement already satisfied: pandas in /opt/conda/envs/python35-paddle120-env/lib/python3.7/site-packages (from visualdl>=2.0.0->paddlex) (1.1.5)
    Requirement already satisfied: Flask-Babel>=1.0.0 in /opt/conda/envs/python35-paddle120-env/lib/python3.7/site-packages (from visualdl>=2.0.0->paddlex) (1.0.0)
    Requirement already satisfied: flake8>=3.7.9 in /opt/conda/envs/python35-paddle120-env/lib/python3.7/site-packages (from visualdl>=2.0.0->paddlex) (3.8.2)
    Requirement already satisfied: protobuf>=3.11.0 in /opt/conda/envs/python35-paddle120-env/lib/python3.7/site-packages (from visualdl>=2.0.0->paddlex) (3.14.0)
    Requirement already satisfied: pyzmq in /opt/conda/envs/python35-paddle120-env/lib/python3.7/site-packages (from paddleslim==1.1.1->paddlex) (18.1.1)
    Requirement already satisfied: scikit-learn in /opt/conda/envs/python35-paddle120-env/lib/python3.7/site-packages (from sklearn->paddlex) (0.22.1)
    Requirement already satisfied: filelock in /opt/conda/envs/python35-paddle120-env/lib/python3.7/site-packages (from paddlehub==2.1.0->paddlex) (3.0.12)
    Requirement already satisfied: gitpython in /opt/conda/envs/python35-paddle120-env/lib/python3.7/site-packages (from paddlehub==2.1.0->paddlex) (3.1.14)
    Collecting paddle2onnx>=0.5.1 (from paddlehub==2.1.0->paddlex)
    [?25l  Downloading https://mirror.baidu.com/pypi/packages/37/80/aa6134b5f36aea45dc1b363e7af941dccabe4d7e167ac391ff046f34baf1/paddle2onnx-0.7-py3-none-any.whl (94kB)
    [K     |████████████████████████████████| 102kB 25.9MB/s ta 0:00:01
    [?25hRequirement already satisfied: rarfile in /opt/conda/envs/python35-paddle120-env/lib/python3.7/site-packages (from paddlehub==2.1.0->paddlex) (3.1)
    Requirement already satisfied: packaging in /opt/conda/envs/python35-paddle120-env/lib/python3.7/site-packages (from paddlehub==2.1.0->paddlex) (20.9)
    Requirement already satisfied: gunicorn>=19.10.0; sys_platform != "win32" in /opt/conda/envs/python35-paddle120-env/lib/python3.7/site-packages (from paddlehub==2.1.0->paddlex) (20.0.4)
    Requirement already satisfied: colorlog in /opt/conda/envs/python35-paddle120-env/lib/python3.7/site-packages (from paddlehub==2.1.0->paddlex) (4.1.0)
    Requirement already satisfied: paddlenlp>=2.0.0rc5 in /opt/conda/envs/python35-paddle120-env/lib/python3.7/site-packages (from paddlehub==2.1.0->paddlex) (2.0.7)
    Requirement already satisfied: easydict in /opt/conda/envs/python35-paddle120-env/lib/python3.7/site-packages (from paddlehub==2.1.0->paddlex) (1.9)
    Requirement already satisfied: cycler>=0.10 in /opt/conda/envs/python35-paddle120-env/lib/python3.7/site-packages (from matplotlib>=2.1.0->pycocotools; platform_system != "Windows"->paddlex) (0.10.0)
    Requirement already satisfied: pytz in /opt/conda/envs/python35-paddle120-env/lib/python3.7/site-packages (from matplotlib>=2.1.0->pycocotools; platform_system != "Windows"->paddlex) (2019.3)
    Requirement already satisfied: python-dateutil>=2.1 in /opt/conda/envs/python35-paddle120-env/lib/python3.7/site-packages (from matplotlib>=2.1.0->pycocotools; platform_system != "Windows"->paddlex) (2.8.0)
    Requirement already satisfied: pyparsing!=2.0.4,!=2.1.2,!=2.1.6,>=2.0.1 in /opt/conda/envs/python35-paddle120-env/lib/python3.7/site-packages (from matplotlib>=2.1.0->pycocotools; platform_system != "Windows"->paddlex) (2.4.2)
    Requirement already satisfied: kiwisolver>=1.0.1 in /opt/conda/envs/python35-paddle120-env/lib/python3.7/site-packages (from matplotlib>=2.1.0->pycocotools; platform_system != "Windows"->paddlex) (1.1.0)
    Requirement already satisfied: Werkzeug>=0.15 in /opt/conda/envs/python35-paddle120-env/lib/python3.7/site-packages (from flask>=1.1.1->visualdl>=2.0.0->paddlex) (0.16.0)
    Requirement already satisfied: itsdangerous>=0.24 in /opt/conda/envs/python35-paddle120-env/lib/python3.7/site-packages (from flask>=1.1.1->visualdl>=2.0.0->paddlex) (1.1.0)
    Requirement already satisfied: click>=5.1 in /opt/conda/envs/python35-paddle120-env/lib/python3.7/site-packages (from flask>=1.1.1->visualdl>=2.0.0->paddlex) (7.0)
    Requirement already satisfied: Jinja2>=2.10.1 in /opt/conda/envs/python35-paddle120-env/lib/python3.7/site-packages (from flask>=1.1.1->visualdl>=2.0.0->paddlex) (2.10.3)
    Requirement already satisfied: future>=0.6.0 in /opt/conda/envs/python35-paddle120-env/lib/python3.7/site-packages (from bce-python-sdk->visualdl>=2.0.0->paddlex) (0.18.0)
    Requirement already satisfied: pycryptodome>=3.8.0 in /opt/conda/envs/python35-paddle120-env/lib/python3.7/site-packages (from bce-python-sdk->visualdl>=2.0.0->paddlex) (3.9.9)
    Requirement already satisfied: aspy.yaml in /opt/conda/envs/python35-paddle120-env/lib/python3.7/site-packages (from pre-commit->visualdl>=2.0.0->paddlex) (1.3.0)
    Requirement already satisfied: nodeenv>=0.11.1 in /opt/conda/envs/python35-paddle120-env/lib/python3.7/site-packages (from pre-commit->visualdl>=2.0.0->paddlex) (1.3.4)
    Requirement already satisfied: virtualenv>=15.2 in /opt/conda/envs/python35-paddle120-env/lib/python3.7/site-packages (from pre-commit->visualdl>=2.0.0->paddlex) (16.7.9)
    Requirement already satisfied: identify>=1.0.0 in /opt/conda/envs/python35-paddle120-env/lib/python3.7/site-packages (from pre-commit->visualdl>=2.0.0->paddlex) (1.4.10)
    Requirement already satisfied: cfgv>=2.0.0 in /opt/conda/envs/python35-paddle120-env/lib/python3.7/site-packages (from pre-commit->visualdl>=2.0.0->paddlex) (2.0.1)
    Requirement already satisfied: importlib-metadata; python_version < "3.8" in /opt/conda/envs/python35-paddle120-env/lib/python3.7/site-packages (from pre-commit->visualdl>=2.0.0->paddlex) (0.23)
    Requirement already satisfied: toml in /opt/conda/envs/python35-paddle120-env/lib/python3.7/site-packages (from pre-commit->visualdl>=2.0.0->paddlex) (0.10.0)
    Requirement already satisfied: idna<2.9,>=2.5 in /opt/conda/envs/python35-paddle120-env/lib/python3.7/site-packages (from requests->visualdl>=2.0.0->paddlex) (2.8)
    Requirement already satisfied: urllib3!=1.25.0,!=1.25.1,<1.26,>=1.21.1 in /opt/conda/envs/python35-paddle120-env/lib/python3.7/site-packages (from requests->visualdl>=2.0.0->paddlex) (1.25.6)
    Requirement already satisfied: chardet<3.1.0,>=3.0.2 in /opt/conda/envs/python35-paddle120-env/lib/python3.7/site-packages (from requests->visualdl>=2.0.0->paddlex) (3.0.4)
    Requirement already satisfied: certifi>=2017.4.17 in /opt/conda/envs/python35-paddle120-env/lib/python3.7/site-packages (from requests->visualdl>=2.0.0->paddlex) (2019.9.11)
    Requirement already satisfied: Babel>=2.3 in /opt/conda/envs/python35-paddle120-env/lib/python3.7/site-packages (from Flask-Babel>=1.0.0->visualdl>=2.0.0->paddlex) (2.8.0)
    Requirement already satisfied: pycodestyle<2.7.0,>=2.6.0a1 in /opt/conda/envs/python35-paddle120-env/lib/python3.7/site-packages (from flake8>=3.7.9->visualdl>=2.0.0->paddlex) (2.6.0)
    Requirement already satisfied: mccabe<0.7.0,>=0.6.0 in /opt/conda/envs/python35-paddle120-env/lib/python3.7/site-packages (from flake8>=3.7.9->visualdl>=2.0.0->paddlex) (0.6.1)
    Requirement already satisfied: pyflakes<2.3.0,>=2.2.0 in /opt/conda/envs/python35-paddle120-env/lib/python3.7/site-packages (from flake8>=3.7.9->visualdl>=2.0.0->paddlex) (2.2.0)
    Requirement already satisfied: scipy>=0.17.0 in /opt/conda/envs/python35-paddle120-env/lib/python3.7/site-packages (from scikit-learn->sklearn->paddlex) (1.3.0)
    Requirement already satisfied: joblib>=0.11 in /opt/conda/envs/python35-paddle120-env/lib/python3.7/site-packages (from scikit-learn->sklearn->paddlex) (0.14.1)
    Requirement already satisfied: gitdb<5,>=4.0.1 in /opt/conda/envs/python35-paddle120-env/lib/python3.7/site-packages (from gitpython->paddlehub==2.1.0->paddlex) (4.0.5)
    Requirement already satisfied: h5py in /opt/conda/envs/python35-paddle120-env/lib/python3.7/site-packages (from paddlenlp>=2.0.0rc5->paddlehub==2.1.0->paddlex) (2.9.0)
    Requirement already satisfied: jieba in /opt/conda/envs/python35-paddle120-env/lib/python3.7/site-packages (from paddlenlp>=2.0.0rc5->paddlehub==2.1.0->paddlex) (0.42.1)
    Requirement already satisfied: seqeval in /opt/conda/envs/python35-paddle120-env/lib/python3.7/site-packages (from paddlenlp>=2.0.0rc5->paddlehub==2.1.0->paddlex) (1.2.2)
    Requirement already satisfied: multiprocess in /opt/conda/envs/python35-paddle120-env/lib/python3.7/site-packages (from paddlenlp>=2.0.0rc5->paddlehub==2.1.0->paddlex) (0.70.11.1)
    Requirement already satisfied: MarkupSafe>=0.23 in /opt/conda/envs/python35-paddle120-env/lib/python3.7/site-packages (from Jinja2>=2.10.1->flask>=1.1.1->visualdl>=2.0.0->paddlex) (1.1.1)
    Requirement already satisfied: zipp>=0.5 in /opt/conda/envs/python35-paddle120-env/lib/python3.7/site-packages (from importlib-metadata; python_version < "3.8"->pre-commit->visualdl>=2.0.0->paddlex) (0.6.0)
    Requirement already satisfied: smmap<4,>=3.0.1 in /opt/conda/envs/python35-paddle120-env/lib/python3.7/site-packages (from gitdb<5,>=4.0.1->gitpython->paddlehub==2.1.0->paddlex) (3.0.5)
    Requirement already satisfied: dill>=0.3.3 in /opt/conda/envs/python35-paddle120-env/lib/python3.7/site-packages (from multiprocess->paddlenlp>=2.0.0rc5->paddlehub==2.1.0->paddlex) (0.3.3)
    Requirement already satisfied: more-itertools in /opt/conda/envs/python35-paddle120-env/lib/python3.7/site-packages (from zipp>=0.5->importlib-metadata; python_version < "3.8"->pre-commit->visualdl>=2.0.0->paddlex) (7.2.0)
    Building wheels for collected packages: pycocotools
      Building wheel for pycocotools (setup.py) ... [?25ldone
    [?25h  Created wheel for pycocotools: filename=pycocotools-2.0.2-cp37-cp37m-linux_x86_64.whl size=278362 sha256=55fea551e9d2f9b5eae48716848e4b992e21f5355e923ac1486712b3720860ce
      Stored in directory: /home/aistudio/.cache/pip/wheels/fb/44/67/8baa69040569b1edbd7776ec6f82c387663e724908aaa60963
    Successfully built pycocotools
    Installing collected packages: xlwt, pycocotools, shapely, paddleslim, paddle2onnx, paddlehub, paddlex
      Found existing installation: paddlehub 2.0.4
        Uninstalling paddlehub-2.0.4:
          Successfully uninstalled paddlehub-2.0.4
    Successfully installed paddle2onnx-0.7 paddlehub-2.1.0 paddleslim-1.1.1 paddlex-1.3.11 pycocotools-2.0.2 shapely-1.7.1 xlwt-1.3.0


# 二、数据集简介  

使用的数据集是患者肺部CT图像，用于诊断新冠肺炎。一共包含800张肺部CT图片（格式包含JPG和PNG两种）,按比例6：2：2划分为训练集、验证集和测试集。  

数据集解压到data目录下的images文件夹下，数据集包含两个文件夹。  
两个文件夹（CT_COVID和CT_NonCOVID）分别包含确诊新冠肺炎的患者和正常人的肺部CT图像。训练所需要的标注信息后续通过代码生成。

 ##   1.解压文件


```python
!mkdir /home/aistudio/data/images #生成存放数据集的路径
!unzip -q /home/aistudio/data/data102630/data.zip -d /home/aistudio/data/images #解压文件
```

##   2. 生成标注
除了训练图片，PaddleClas还需要我们提供一个数据列表文件，里面每条数据按照 “文件路径 类别” 的格式标记，以供后续训练。使用下面的代码生成标注。数据和标注的格式均按照ImageNet格式。查询PaddleX文档即可（新手可以通过文档中的“10分钟快速上手使用部分”快速学习）：[PaddleX文档链接](https://paddlex.readthedocs.io/zh_CN/release-1.3/index.html)


```python
import os
base_dir = "/home/aistudio/data/images/" # CT图片所在路径
img_dirs = ["CT_NonCOVID",  "CT_COVID"] # 两类CT图片文件夹名

file_names = ["train_list.txt", "val_list.txt", "test_list.txt"]
splits = [0, 0.6, 0.8, 1] # 按照 6 2 2 的比例对数据进行分组

for split_ind, file_name in enumerate(file_names):
    with open(os.path.join("./data", file_name), "w") as f:
        for type_ind, img_dir in enumerate(img_dirs):
            imgs = os.listdir(os.path.join(base_dir, img_dir) )
            for ind in range( int(splits[split_ind]* len(imgs)), int(splits[split_ind + 1] * len(imgs)) ):
                print("{} {}".format(img_dir + "/" + imgs[ind], type_ind), file = f)
```

生成label.txt文件，有可以自行手动建立


```python
category=["CT_COVID","CT_NonCOVID"] #数据集包含的类别
with open(os.path.join("./data","labels.txt"),"w") as f:  #生成存放labels.txt的路径
    for cat in enumerate(category):
        print("{} ".format(cat[1]), file = f)

```

## 3. 数据加载和预处理 
在PaddleClas中，数据只要与ImageNet数据集的格式一致，便可以直接调用pdx.datasets.ImageNet函数加载。


```python
#定义数据增强
import paddlex as pdx
from paddlex.cls import transforms
train_transforms = transforms.Compose([
    transforms.RandomCrop(crop_size=224), 
    transforms.RandomHorizontalFlip(),
    transforms.Normalize()
])
eval_transforms = transforms.Compose([
    transforms.ResizeByShort(short_size=256),
    transforms.CenterCrop(crop_size=224), 
    transforms.Normalize()
])
#加载数据
train_dataset = pdx.datasets.ImageNet(
                    data_dir='./data/images',
                    file_list='./data/train_list.txt',
                    label_list='./data/labels.txt',
                    transforms=train_transforms,
                    shuffle=True)
eval_dataset = pdx.datasets.ImageNet(
                    data_dir='./data/images',
                    file_list='./data/val_list.txt',
                    label_list='./data/labels.txt',
                    transforms=eval_transforms)
test_dataset = pdx.datasets.ImageNet(
                    data_dir='./data/images',
                    file_list='./data/test_list.txt',
                    label_list='./data/labels.txt',
                    transforms=eval_transforms)
```

    2021-08-11 21:43:34 [INFO]	Starting to read file list from dataset...
    2021-08-11 21:43:34 [INFO]	449 samples in file ./data/train_list.txt
    2021-08-11 21:43:34 [INFO]	Starting to read file list from dataset...
    2021-08-11 21:43:34 [INFO]	150 samples in file ./data/val_list.txt
    2021-08-11 21:43:34 [INFO]	Starting to read file list from dataset...
    2021-08-11 21:43:34 [INFO]	151 samples in file ./data/test_list.txt


# 三、模型选择和开发
使用ResNet101_vd_ssld模型开始训练，选用该模型是在切换不同模型及参数情况下，该模型训练结果最好。

## 1. 模型训练


```python
num_classes = len(train_dataset.labels)
model = pdx.cls.ResNet101_vd_ssld(num_classes=2)

model.train(num_epochs=50,
            train_dataset=train_dataset,
            train_batch_size=32,
            eval_dataset=eval_dataset,
            learning_rate=0.01,
            lr_decay_epochs=[10, 20, 25],
            save_dir='output/resnet101_vd_ssld',
            use_vdl=True)
```

    /opt/conda/envs/python35-paddle120-env/lib/python3.7/site-packages/paddle/fluid/framework.py:689: DeprecationWarning: `np.bool` is a deprecated alias for the builtin `bool`. To silence this warning, use `bool` by itself. Doing this will not modify any behavior and is safe. If you specifically wanted the numpy scalar type, use `np.bool_` here.
    Deprecated in NumPy 1.20; for more details and guidance: https://numpy.org/devdocs/release/1.20.0-notes.html#deprecations
      elif dtype == np.bool:


    2021-08-01 16:51:50 [INFO]	Decompressing output/resnet101_vd_ssld/pretrain/ResNet101_vd_ssld_pretrained.tar...
    2021-08-01 16:51:54 [INFO]	Load pretrain weights from output/resnet101_vd_ssld/pretrain/ResNet101_vd_ssld_pretrained.
    2021-08-01 16:51:54 [WARNING]	[SKIP] Shape of pretrained weight output/resnet101_vd_ssld/pretrain/ResNet101_vd_ssld_pretrained/fc_0.w_0 doesn't match.(Pretrained: (2048, 1000), Actual: (2048, 2))
    2021-08-01 16:51:54 [WARNING]	[SKIP] Shape of pretrained weight output/resnet101_vd_ssld/pretrain/ResNet101_vd_ssld_pretrained/fc_0.b_0 doesn't match.(Pretrained: (1000,), Actual: (2,))
    2021-08-01 16:51:55 [INFO]	There are 530 varaibles in output/resnet101_vd_ssld/pretrain/ResNet101_vd_ssld_pretrained are loaded.
    2021-08-01 16:52:01 [INFO]	[TRAIN] Epoch=1/50, Step=2/14, loss=0.700481, acc1=0.4375, acc2=1.0, lr=0.01, time_each_step=2.9s, eta=0:45:53
    2021-08-01 16:52:01 [INFO]	[TRAIN] Epoch=1/50, Step=4/14, loss=0.650503, acc1=0.59375, acc2=1.0, lr=0.01, time_each_step=1.57s, eta=0:24:48
    2021-08-01 16:52:02 [INFO]	[TRAIN] Epoch=1/50, Step=6/14, loss=0.596561, acc1=0.6875, acc2=1.0, lr=0.01, time_each_step=1.11s, eta=0:17:28
    2021-08-01 16:52:02 [INFO]	[TRAIN] Epoch=1/50, Step=8/14, loss=0.673924, acc1=0.53125, acc2=1.0, lr=0.01, time_each_step=0.88s, eta=0:13:46
    2021-08-01 16:52:02 [INFO]	[TRAIN] Epoch=1/50, Step=10/14, loss=0.552203, acc1=0.75, acc2=1.0, lr=0.01, time_each_step=0.74s, eta=0:11:32
    2021-08-01 16:52:03 [INFO]	[TRAIN] Epoch=1/50, Step=12/14, loss=0.617196, acc1=0.65625, acc2=1.0, lr=0.01, time_each_step=0.64s, eta=0:10:3
    2021-08-01 16:52:03 [INFO]	[TRAIN] Epoch=1/50, Step=14/14, loss=0.518498, acc1=0.75, acc2=1.0, lr=0.01, time_each_step=0.58s, eta=0:9:0
    2021-08-01 16:52:03 [INFO]	[TRAIN] Epoch 1 finished, loss=0.63404, acc1=0.631696, acc2=1.0, lr=0.01 .
    2021-08-01 16:52:03 [INFO]	Start to evaluating(total_samples=150, total_steps=5)...


      0%|          | 0/5 [00:00<?, ?it/s]share_vars_from is set, scope is ignored.
    100%|██████████| 5/5 [00:06<00:00,  1.34s/it]


    2021-08-01 16:52:10 [INFO]	[EVAL] Finished, Epoch=1, acc1=0.633333, acc2=1.0 .


## 2. 模型评估测试


```python
# 模型评估，根据prepare接口配置的loss和metric进行返回
import paddlex as pdx
model = pdx.load_model('output/resnet101_vd_ssld/best_model') #加载训练好的模型
result = model.evaluate(test_dataset,batch_size=32)

print(result)
```

    2021-08-11 21:49:47 [INFO]	Model[ResNet101_vd_ssld] loaded.
    2021-08-11 21:49:47 [INFO]	Start to evaluating(total_samples=151, total_steps=5)...


    100%|██████████| 5/5 [00:47<00:00,  9.54s/it]


    OrderedDict([('acc1', 0.9536423841059603), ('acc2', 1.0)])


## 5.模型预测
此处仅举例使用数据集中的数据


```python
import paddlex as pdx
import numpy as np
model = pdx.load_model('output/resnet101_vd_ssld/best_model')
result = model.predict('data/images/CT_NonCOVID/91%0.jpg')
print("Predict Result: ",np.argmax(result))
```

    2021-08-11 21:59:03 [INFO]	Model[ResNet101_vd_ssld] loaded.
    Predict Result:  0


# 四、效果展示
该项目只需要按步骤即可执行，代码比较简单适合新手学习Paddle。在训练过程中，手动调整参数，根据训练结果选出最好的模型。  
调整的参数可以自行手动记录，如图所示。
![](https://ai-studio-static-online.cdn.bcebos.com/281cd75a282e465dbe3833250551ef0dbf058685087b46609e0147b735176766)


# 五、总结与升华
通过该项目，快速学习了Paddle的使用，上传数据集时要注意压缩格式和解压语句是否对应，训练过程中的参数不需要全部了解，需要用到的部分再去查文档，这样可以更快的完成一个项目。
**补充知识：**
医疗图像的格式一般为特殊格式（如CT图像格式为dicom），在使用中可以通过simpleITK包进行加载，或者通过专用软件（如MicroDicom）将图像转换为常用的图片格式。
在检测和分割问题中，医疗格式的数据可以通过ITK-SNAP等专业软件标注，或者将数据转换为图片格式后使用Labelimg进行标注。

# 个人简介
一个在医疗图像上努力的小菜狗，感兴趣的同学可以关注我。  

[AI Studio个人链接](https://aistudio.baidu.com/aistudio/personalcenter/thirdview/453024)
