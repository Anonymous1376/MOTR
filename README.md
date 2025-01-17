# MOTR: End-to-End Multiple-Object Tracking with TRansformer

This repository is an official implementation.

## Introduction

**TL; DR.** MOTR is a fully end-to-end multiple-object tracking framework based on Transformer. It directly outputs the tracks within the video sequences without any association procedures.

<div style="align: center">
<img src=./figs/motr.png/>
</div>

**Abstract.** The key challenge in multiple-object tracking task is temporal modeling of the object under track. Existing tracking-by-detection methods adopt simple heuristics, such as spatial or appearance similarity. Such methods, in spite of their commonality, are overly simple and lack the ability to learn temporal variations from data in an end-to-end manner. In this paper, we present MOTR, a fully end-to-end multiple-object tracking framework. It learns to model the long-range temporal variation of the objects. It performs temporal association implicitly and avoids previous explicit heuristics. Built upon DETR, MOTR introduces the concept of "track query". Each track query models the entire track of an object. It is transferred and updated frame-by-frame to perform iterative predictions in a seamless manner. Tracklet-aware label assignment is proposed for one-to-one assignment between track queries and object tracks. Temporal aggregation network together with collective average loss is further proposed to enhance the long-range temporal relation. Experimental results show that MOTR achieves competitive performance and can serve as a strong Transformer-based baseline for future research.


## Main Results

|  **Method**  |  **Dataset**  |  **Train Data**  |  **MOTA**  |  **IDF1**  |  **IDS**  |
|:------:|:------:|:------:|:------:|:------:|:------:|
| MOTR | MOT16 | MOT17+CrowdHuman Val | 66.8 | 67.0 | 586 |
| MOTR | MOT17 | MOT17+CrowdHuman Val | 67.4 | 67.0 | 1992 |

*Note:*

1. All models of MOTR are trained on 8 NVIDIA Tesla V100 GPUs.
2. The training time is about 2.5 days for 200 epochs;
3. The inference speed is about 7.5 FPS for resolution 1536x800;
4. All models of MOTR are trained with ResNet50 with pre-trained weights on COCO dataset.


## Installation

The codebase is built on top of [Deformable DETR](https://github.com/fundamentalvision/Deformable-DETR).

### Requirements

* Linux, CUDA>=9.2, GCC>=5.4
  
* Python>=3.7

    We recommend you to use Anaconda to create a conda environment:
    ```bash
    conda create -n deformable_detr python=3.7 pip
    ```
    Then, activate the environment:
    ```bash
    conda activate deformable_detr
    ```
  
* PyTorch>=1.5.1, torchvision>=0.6.1 (following instructions [here](https://pytorch.org/))

    For example, if your CUDA version is 9.2, you could install pytorch and torchvision as following:
    ```bash
    conda install pytorch=1.5.1 torchvision=0.6.1 cudatoolkit=9.2 -c pytorch
    ```
  
* Other requirements
    ```bash
    pip install -r requirements.txt
    ```

* Build MultiScaleDeformableAttention
    ```bash
    cd ./models/ops
    sh ./make.sh
    ```

## Usage

### Dataset preparation

Please download [MOT17 dataset](https://motchallenge.net/) and [CrowdHuman dataset](https://www.crowdhuman.org/) and organize them like [FairMOT](https://github.com/ifzhang/FairMOT) as following:

```
.
├── crowdhuman
│   ├── images
│   └── labels_with_ids
├── MOT15
│   ├── images
│   ├── labels_with_ids
│   ├── test
│   └── train
├── MOT17
│   ├── images
│   ├── labels_with_ids

```

### Training and Evaluation

#### Training on single node

You can download COCO pretrained weights from [Deformable DETR](https://github.com/fundamentalvision/Deformable-DETR). Then training MOTR on 8 GPUs as following:

```bash 
sh configs/r50_motr_train.sh

```

#### Evaluation on MOT15

You can download the pretrained model of MOTR (the link is in "Main Results" session), then run following command to evaluate it on MOT15 train dataset:

```bash 
sh configs/r50_motr_eval.sh

```

For visual in demo video, you can enable 'vis=True' in eval.py like:
```bash 
det.detect(vis=True)

```

#### Evaluation on MOT17

You can download the pretrained model of MOTR (the link is in "Main Results" session), then run following command to evaluate it on MOT17 test dataset (submit to server):

```bash
sh configs/r50_motr_submit.sh

```
