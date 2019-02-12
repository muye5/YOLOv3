# YOLOv3
YOLOv3 implemented with distributed tensorflow including train and inference module.

### Usage

###### 1. generate anchors

>put `VOC 2007/2012` or `custom files` with `VOC Annotation Format` under `data/` and run:

    ./run_prepare.sh -a

>data meta info and anchor output as below: 

    max_width = 500, max_height = 500, max_box_per_image = 39
    iteration 1: dists = 256748.369356
    iteration 2: dists = 10246.3258411
    ...
    ... 
    ('\naverage IOU for', 9, 'anchors:', '0.68')
    29,53, 61,94, 73,205, 134,119, 135,271, 218,329, 256,162, 342,247, 363,374

>copy `max_box_per_image` and `anchors` into `yolo_darknet53_voc.json` or yourself config json file. `max_width` and `max_height` are used to be width and height when converting data to tfrecord.

###### 2. convert data to tfrecord
>convert `VOC Annotation Format` image and label into tfrecord under `data/tfrecord/`

    ./run_prepare.sh -t

###### 3. train
>confirm all parameters in the config file before begining to train, refering to `yolo_darknet53_voc.json` <br>
>download the [darknet53.conv.74](https://pjreddie.com/media/files/darknet53.conv.74) or [tiny.weights](https://pjreddie.com/media/files/tiny.weights) from yolo homepage and put them under `conf/init/`. <br>
>of course you can train with random initialization, for my limited experience, it could get similar mAP if your problem is not very complex like coco. I got very similar mAP on my other tasks execpt for convergence speed.

    ./run_train.sh -l

>you must change the data path and model storage directory parameters and rewrite `distributed_train()` in `run_train.sh`, then replace `-l` with `-r`

###### 4. convert checkpoint model to pb
>convert checkpoint model to pb format for inference

    ./run_convert.sh -d

###### 5. predict
>predict the `VOC2007 Test` and output a single result file for every class.

    ./run_evaluate.sh -p

###### 6. evaluate mAP

    ./run_evaluate.sh -e

### Performance
Train Data | Test Data | Input Size | Model | Initialize | mAP | FLOPS |
-----------|-----------|------------|-------|------------|-----|-------|
VOC2007+2012 | VOC2007 Test | 416*416 | DarkNet53(YOLOv3) | darknet53.conv.74 | 0.772 | 65.86 |
VOC2007+2012 | VOC2007 Test | 416*416 | YOLOFLY | tiny.weights | 0.691 | 9.78 |


#### Acknowledgements
Thanks to [experiencor](https://github.com/experiencor) for his great job of [keras-yolo3](https://github.com/experiencor/keras-yolo3)
