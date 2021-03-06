# Codebase for deep autonomous driving perception tasks

Segmentation models (**Deeplab, FCN, ERFNet**), Lane detection models (**ERFNet, ERFNet-SCNN and others**) based on Python 3.6 and PyTorch 1.6.0 (CUDA 10) & TorchVision 0.7.0 with mixed precision training.

## Highlights

Fast probability map to poly line conversion, modulated (borrowed) mIOU & pixel acc calculation, "poly" learning rate schedule, basic input loading, transformations and visualizations, also tests of mixed precision training and tensorboard logging.

And models from this repo is faster (also better or at least similar) than the official implementations and other implementations out there.

## Supported datasets: 

| Task | Dataset |
| :---: | :---: |
| semantic segmentation | PASCAL VOC 2012 |
| semantic segmentation | Cityscapes |
| semantic segmentation | GTAV* |
| semantic segmentation | SYNTHIA* |
| lane detection | CULane |
| lane detection | TuSimple (*In progress*) |

\* The UDA baseline setup, with Cityscapes *val* set as validation.

## Supported models:

| Task | Backbone | Model |
| :---: | :---: | :---: |
| semantic segmentation | ResNet-101 | FCN |
| semantic segmentation | ResNet-101 | DeeplabV2 |
| semantic segmentation | ResNet-101 | DeeplabV3 |
| semantic segmentation | - | ERFNet |
| lane detection | ERFNet | ERFNet |
| lane detection | ERFNet | SCNN |
| lane detection | ERFNet | SAD (*In progress*) |
| lane detection | ERFNet | PRNet (*In progress*) |

*You can of course also use other backbones (e.g. ResNet-50) in TorchVision by simply calling a different function by using the most recent TorchVision implementation*

## Semantic segmentation performance:

| model | resolution | mixed precision? | Dataset | mIoU (%) | Training time |
| :---: | :---: | :---: | :---: | :---: | :---: |
| FCN | 321 x 321 | *yes* | PASCAL VOC 2012 | 69.46 | 3.3h |
| FCN | 321 x 321 | *no* | PASCAL VOC 2012 | 69.90 | 6.3h |
| DeeplabV2 | 321 x 321 | *yes* | PASCAL VOC 2012 | 72.84 | 3.3h |
| DeeplabV3 | 321 x 321 | *yes* | PASCAL VOC 2012 | 77.05 | 6.9h |
| FCN | 257 x 513 | *yes* | Cityscapes | 65.79 | 2.3h |
| DeeplabV2 | 257 x 513 | *yes* | Cityscapes | 66.89 | 2.3h |
| DeeplabV3 | 257 x 513 | *yes* | Cityscapes | 67.87 | 4.8h |
| DeeplabV2 | 257 x 513 | *no* | Cityscapes | 67.07 | 4h |
| ERFNet| 512 x 1024 | *yes* | Cityscapes | 71.99 | 5h |
| DeeplabV2 | 512 x 1024 | *yes* | Cityscapes | 71.17 | 9h |

*\* All performance is measured with ImageNet pre-training and reported as 3 times average on validation set. Note that the best run from ERFNet on Cityscapes val is 72.47% in mIoU, slightly better than the original implementation by the authors (72.2%).*

## Lane detection performance:

| model | resolution | mixed precision? | Dataset | F measure | Training time |
| :---: | :---: | :---: | :---: | :---: | :---: |
| ERFNet | 288 x 800 | *yes* | CULane | 0.7346 | 6h |
| SCNN | 288 x 800 | *yes* | CULane |  |  |


*\* All performance is measured with ImageNet pre-training and reported as 3 times average on test set.*

## Preparations:

1. Setup a Python3 environment (CUDA 10), with PyTorch >= 1.6, TorchVision >= 0.7.0, tqdm, tensorboard, numpy.

2. Download the code:
   
```
git clone https://github.com/voldemortX/pytorch-auto-drive.git
cd code
```

3. Prepare the code:

```
chmod 777 *.sh
```

## Enable tensorboard (optional):

```
tensorboard --logdir=runs
```

## Getting started

Get started with [SEGMENTATION.md](SEGMENTATION.md) for semantic segmentation.

Get started with [LANEDETECTION.md](LANEDETECTION.md) for lane detection.

## Notes:

1. Cityscapes dataset is down-sampled by 2 when training at 257 x 513, to specify different sizes, modify them in code/data_processing.py; similar changes can be done with other experiments.

2. Training times are measured on **a single RTX 2080Ti**, including online validation time for segmentation, test time for lane detection.

3. All segmentation results reported are from single model without CRF and without multi-scale testing.

4. PR and issues are always welcomed.
