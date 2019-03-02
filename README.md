# Domain Adaptive Faster R-CNN

A PyTorch implementation of the CVPR 2018 paper [Domain Adaptive Faster R-CNN for Object Detection in the Wild](https://arxiv.org/pdf/1803.03243).
The original code used by the authors can be found [here](https://github.com/yuhuayc/da-faster-rcnn).

This implementation is built on [jwyang/faster-rcnn.pytorch](https://github.com/jwyang/faster-rcnn.pytorch). You may find it to be helpful for debugging.

Note that consistency regularization is broken hence disabled. 

## Usage
This model supports `pytorch 0.2.0`.

Install python dependencies with `pip install -r requirements.txt`.

Ensure all images used for training (source) and testing have annotations (in Pascal VOC format).

Modify [this line](https://github.com/divyam02/dafrcnn-pytorch/blob/a4ee095b6818a0351f3aca306262d82425c39948/new_trainval_net.py#L494) in `new_trainval_net.py` to the directory of the target dataset.

Add labels as needed in `./lib/datasets/pascal_voc.py`.

Place source data in `./data/src/` in PASCAL VOC format. Similarly for target data, under `./data/tar/`.

Delete `cache` folders in `src` and `tar` before training and testing on a new data set.

Add the data set in `new_trainval_net.py` wrt [this line](https://github.com/divyam02/dafrcnn-pytorch/blob/a4ee095b6818a0351f3aca306262d82425c39948/new_trainval_net.py#L232).

### Compile
Compile cuda dependencies with `cd lib && sh make.sh`. Incase cuda is not on `$PATH` use `make2.sh`.

### Train
Add the source and target datasets (in Pascal VOC format) in the `src/` and `tar/` folders respectively.

Modify `factory.py` as given in the dummy formatting so it is usable.

Run training as:
```
CUDA_VISIBLE_DEVICES=$GPU_ID python trainval_net_x.py \
                   --src $SOURCE_DATASET_NAME --da True 
                   --net res101 \ --bs $BATCH_SIZE --nw $WORKER_NUMBER \
                   --lr $LEARNING_RATE --lr_decay_step $DECAY_STEP \
                   -- $PATH_TO_PRETRAINED_MODEL save_dir $./data/pretrained_model/
                   --cuda
```
### Test
For testing your model, ensure the model is placed in the target dataset folder. Run:
```
python test_net_x.py --dataset $TARGET_DATASET_NAME --net res101 \
                   --checksession $SESSION --checkepoch $EPOCH --checkpoint $CHECKPOINT \
                   --cuda --load_dir $path/to/dir
```
Specify the specific model session, checkepoch and checkpoint, e.g., SESSION=1, EPOCH=5, CHECKPOINT=5931.

### Demo
Follow instructions given [here](https://github.com/jwyang/faster-rcnn.pytorch#demo)

## Benchmarks
Source dataset  | Target dataset  |
-----------------|-----------------|
Cityscapes  | Foggy Cityscapes

Model | Mean AP | Person AP | Rider AP  | Car AP  | Truck AP  | Bus AP  | Train AP  | Motorcycle AP | Bicycle AP  |
------|---------|-----------|-----------|---------|-----------|---------|-----------|---------------|-------------|
Faster R-CNN | 18.8  | 17.8  | 23.6  | 27.1  | 11.9  | 23.8  | 9.1 | 14.4  | 22.8  |
Y.Chen et al  | 27.6  | 25.0  | 31.0  | 40.5  | 22.1  | 35.3  | 20.2  | 20.0  | 27.1  |
dafrcnn-pytorch | 28.6  | 25.7  | 40.1  | 36.0  | 20.1  | 33.9  | 17.3  | 24.3  | 31.3  |

## Acknowledgements
This code is built on the python implementation of the Faster-RCNN [jwyang/faster-rcnn.pytorch](https://github.com/jwyang/faster-rcnn.pytorch)

Part of an ongoing project under Dr. Saket Anand
