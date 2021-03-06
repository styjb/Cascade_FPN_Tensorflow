# Cascade R-CNN: Delving into High Quality Object Detection 

## Abstract     
This repo is based on [FPN](https://github.com/DetectionTeamUCAS/FPN_Tensorflow), and completed by [YangXue](https://github.com/yangxue0827).     

## Train on COCO train2017 and test on COCO val2017 (coco minival).   
|Model|Backbone|Train Schedule|GPU|Image/GPU|FP16|Box AP(Mask AP)|test stage|
|-----|--------|--------------|---|---------|----|---------------|---|
|Faster (paper)|R50v1-FPN|1X|8X TITAN XP|1|no|38.3|3|
|Faster (ours)|R50v1-FPN|1X|8X 2080 Ti|1|no|38.2|3|
|Faster (Face++)|R50v1-FPN|1X|8X 2080 Ti|2|no|39.1|3|

![2](comparison.png)

## My Development Environment
1、python3.5 (anaconda recommend)             
2、cuda9.0 **(If you want to use cuda8, please set CUDA9 = False in the cfgs.py file.)**                    
3、[opencv(cv2)](https://pypi.org/project/opencv-python/)    
4、[tfplot](https://github.com/wookayin/tensorflow-plot)             
5、tensorflow == 1.12                   

## Download Model
### Pretrain weights
1、Please download [resnet50_v1](http://download.tensorflow.org/models/resnet_v1_50_2016_08_28.tar.gz), [resnet101_v1](http://download.tensorflow.org/models/resnet_v1_101_2016_08_28.tar.gz) pre-trained models on Imagenet, put it to data/pretrained_weights.       
2、Or you can choose to use a better backbone, refer to [gluon2TF](https://github.com/yangJirui/gluon2TF). [Pretrain Model Link](https://pan.baidu.com/s/1HF3G5XSxXm7W4pk10RuOlw), password: q4jg.

### Trained weights
**Select a configuration file in the folder ($PATH_ROOT/libs/configs/) and copy its contents into cfgs.py, then download the corresponding [weights](https://github.com/DetectionTeamUCAS/Models/tree/master/Cascade_FPN_Tensorflow).**      

## Compile
```  
cd $PATH_ROOT/libs/box_utils/cython_utils
python setup.py build_ext --inplace
```

## Train

1、If you want to train your own data, please note:  
```     
(1) Modify parameters (such as CLASS_NUM, DATASET_NAME, VERSION, etc.) in $PATH_ROOT/libs/configs/cfgs.py
(2) Add category information in $PATH_ROOT/libs/label_name_dict/lable_dict.py     
(3) Add data_name to $PATH_ROOT/data/io/read_tfrecord_multi_gpu.py 
```     

2、make tfrecord
```  
cd $PATH_ROOT/data/io/  
python convert_data_to_tfrecord_coco.py --VOC_dir='/PATH/TO/JSON/FILE/' 
                                        --save_name='train' 
                                        --dataset='coco'
```     

3、multi-gpu train
```  
cd $PATH_ROOT/tools
python multi_gpu_train.py
```

## Eval
```  
cd $PATH_ROOT/tools
python eval_coco.py --eval_data='/PATH/TO/IMAGES/'  
                    --eval_gt='/PATH/TO/TEST/ANNOTATION/'
                    --GPU='0'
```

## Tensorboard
```  
cd $PATH_ROOT/output/summary
tensorboard --logdir=.
``` 
![3](images.png)

![4](scalars.png)

## Reference
1、https://github.com/endernewton/tf-faster-rcnn    
2、https://github.com/zengarden/light_head_rcnn     
3、https://github.com/tensorflow/models/tree/master/research/object_detection        
4、https://github.com/CharlesShang/FastMaskRCNN       
5、https://github.com/matterport/Mask_RCNN      
6、https://github.com/msracver/Deformable-ConvNets         
7、https://github.com/tensorpack/tensorpack       
