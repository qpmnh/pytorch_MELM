# pytorch_MLEM

This is the pytorch implementation for the paper《Min-Entropy Latent Model for Weakly Supervised Object Detection》,which is a accepted paper in [CVPR2018](http://openaccess.thecvf.com/content_cvpr_2018/papers/Wan_Min-Entropy_Latent_Model_CVPR_2018_paper.pdf) and [TPAMI](https://ieeexplore.ieee.org/document/8640243). 

This implementation is based on [Winfrand's](https://github.com/Winfrand/MELM) which is the official version based on torch7 and lua. I would like to thank ruotianluo's [pytorch-faster-rcnn](https://github.com/ruotianluo/pytorch-faster-rcnn).
# Prerequisites
* Nivdia GPU 1080Ti
* Ubuntu 16.04 LTS
* pytorch **0.4** is required and we will update a new version for pytorch **1.0** soon. 
* tensorflow **1.0**, tensorboard and [tensorboardX](https://github.com/lanpa/tensorboardX) for visualizing training and    validation curve.

# Installation
1. Clone the repository
  ```Shell
  git clone https://github.com/vasgaowei/pytorch_MELM.git
  ```
2. Compile the modules(nms, roi_pooling, roi_ring_pooling and roi_align)
  ```
  cd pytorch_MELM/lib
  bash make.sh
  ```
# Setup the data

1. Download the training, validation, test data and the VOCdevkit
  ```
  cd pytorch_MELM/
  mkdir data
  cd data/
  wget http://host.robots.ox.ac.uk/pascal/VOC/voc2007/VOCtrainval_06-Nov-2007.tar
  wget http://host.robots.ox.ac.uk/pascal/VOC/voc2007/VOCtest_06-Nov-2007.tar
  wget http://host.robots.ox.ac.uk/pascal/VOC/voc2007/VOCdevkit_08-Jun-2007.tar
  ```
2. Extract all of these tars into one directory named VOCdevkit
  ```
  tar xvf VOCtrainval_06-Nov-2007.tar
  tar xvf VOCtest_06-Nov-2007.tar
  tar xvf VOCdevkit_08-Jun-2007.tar
  ```
3. Create symlinks for PASCAL VOC dataset or just rename the VOCdevkit to VOCdevkit2007
  ```
  cd pytorch_MELM/data
  ln -s VOCdevkit VOCdevkit2007
  ```
4. It should have this basic structure
  ```
  $VOCdevkit2007/                     # development kit
  $VOCdevkit2007/VOC2007/             # VOC utility code
  $VOCdevkit2007/VOCcode/             # image sets, annodations, etc
  ```
# Download the pre-trained ImageNet models
  Downliad the pre-trained ImageNet models from https://drive.google.com/drive/folders/0B1_fAEgxdnvJSmF3YUlZcHFqWTQ
  or download from  https://drive.google.com/drive/folders/1FV6ZOHOxLMQjE4ujTNOObI7lN8USH0v_?usp=sharing and put in in the     data/imagenet_weights and rename it vgg16.pth. The folder has the following form.
  ```
  $ data/imagenet_weights/vgg16.pth
  $ data/imagenet_weights/res50.pth
  ```
# Download the Selective Search proposals for PASCAL VOC 2007
  Download it from: https://dl.dropboxusercontent.com/s/orrt7o6bp6ae0tc/selective_search_data.tgz
  and unzip it and the final folder has the following form
  ```
  $ data/selective_search_data/voc_2007_train.mat
  $ data/selective_search_data/voc_2007_test.mat
  $ data/selective_search_data/voc_2007_trainval.mat
  ```
# Train your own model
  For vgg16 backbone, we can train the model using the following commands
  ```
  ./experiments/scripts/train_faster_rcnn.sh 0 pascal_voc vgg16
  ```
  And for test, we can using the following commands
  ```
  ./experiments/scripts/test_faster_rcnn.sh 0 pascal_voc vgg16
  ```
# Visualizing some detection results
  I have pretrained MLEM_pytorch model on PASCAL VOC 2007 based vgg16 backbone and you can download it from              https://drive.google.com/drive/folders/1FV6ZOHOxLMQjE4ujTNOObI7lN8USH0v_?usp=sharing and put it in the
  folder output vgg16/voc_2007_trainval/default/vgg16_MELM.pth and run the following commands.
  ```
  cd pytorch_MELM
  python ./tools/demo.py --net vgg16 --dataset pascal_voc
  ```
  Also you can visualize training and validation curve.
  ```
  tensorboard --logdir tensorboard/vgg16/voc_2007_trainval/
  ```
  
