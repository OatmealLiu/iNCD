# Class-incremental Novel Class Discovery (ECCV2022)
> Class-incremental Novel Class Discovery (ECCV2022)
>
> Subhankar Roy†, Mingxuan Liu†, Zhun Zhong, Nicu Sebe, and Elisa Ricci
>
> † equal contribution

This Github repository presents the PyTorch implementation for the paper **Class-incremental Novel Class Discovery** \[[arXiv](https://arxiv.org/abs/2207.08605)\], accepted with a poster presentation at European Conference on Computer Vision (ECCV) held at Tel Aviv International Convention Center on October 23-27, 2022.

![](figures/framework.png)


## Preparation
### Environment
```shell
Python >= 3.8.8
PyTorch >= 1.10.0 
```

`environment.yaml` includes all the dependencies for conda installation. To install (Please pre-install [Anaconda](https://www.anaconda.com/)):
```shell
conda env create -f environment.yaml
```
To activate the installed environment:
```shell
conda activate iNCD
```

### Dataset
Option 1
- Download our prepared datasets (CIFAR-10, CIFAR-100 and TinyImagenet) from drive [datasets](https://drive.google.com/file/d/1O0PPC_Bk16XLLe-UFCSw1UdbRSc-DyOI/view?usp=drive_link)
- Move the downloaded `datasets.zip` file to `./data/` folder.
```shell
# cd to the repository root
cd data
unzip datasets.zip
```

Option 2
- For CIFAR-10 and CIFAR-100 simply download the datasets and put into `./data/datasets/`.
- For TinyImagenet, to download and generate image folders to `./data/datasets/`. Please follow https://github.com/tjmoon0104/pytorch-tiny-imagenet

## Training and Testing
![](figures/setting.png)

### Step 1: Supervised learning with labelled data
```shell
# Pre-train on CIFAR-10 (# of base classes: 5)
CUDA_VISIBLE_DEVICES=0 sh step1_scripts/pretrain_cifar10.sh

# Pre-train on CIFAR-100 (# of base classes: 80)
CUDA_VISIBLE_DEVICES=0 sh step1_scripts/pretrain_cifar100.sh

# Pre-train on TinyImagenet (# of base classes: 180)
CUDA_VISIBLE_DEVICES=0 sh step1_scripts/pretrain_tinyimagenet.sh
```

### Step 2: Class-incremental Novel Class Discovery (class-iNCD) with unlabeled data
```shell
# class-iNCD on CIFAR-10 (# of novel classes: 5)
CUDA_VISIBLE_DEVICES=0 sh step2_scripts_cifar10/incd_OG_FRoST.sh

# class-iNCD on CIFAR-100 (# of novel classes: 20)
CUDA_VISIBLE_DEVICES=0 sh step2_scripts_cifar100/incd_OG_FRoST.sh

# class-iNCD on TinyImagenet (# of novel classes: 20)
CUDA_VISIBLE_DEVICES=0 sh step2_scripts_tinyimagenet/incd_OG_FRoST.sh
```

### Two-steps class-iNCD
```shell
# Two-step class-iNCD on CIFAR-100 (80-10-10)
CUDA_VISIBLE_DEVICES=0 sh two-steps_scripts/auto_2step_incd_OG_FRoST_cifar100.sh

# Two-step class-iNCD on TinyImagenet (180-10-10)
CUDA_VISIBLE_DEVICES=0 sh two-steps_scripts/auto_2step_incd_OG_FRoST_tinyimagenet.sh
```

## Evaluation Protocol
![](figures/evalutation.png)

## Testing the Trained Model
You can use the following scripts to test the trained models under class-iNCD and two-step class-iNCD settings.

We also provide our trained models which you can use to reproduce the experimental results in our paper:
- Download our trained model weights from drive [trained models](https://drive.google.com/file/d/1R6EB2biQj5iBPYZwC7dAzoy-qMJ-Naz6/view?usp=sharing)
- Move the downloaded `experiments.zip` file to `./data/` folder. Then:
```shell
# cd to the repository root
cd data
unzip experiments.zip # Note: this will replace your saved model weights in your `./data/experiments/` folder
```

### Test class-iNCD setting
```shell
# CIFAR-10
sh test_cifar10/test_FRoST_incd.sh

# CIFAR-100
sh test_cifar100/test_FRoST_incd.sh

# TinyImagenet
sh test_tinyimagenet/test_FRoST_incd.sh
```

### Test two-step class-iNCD setting
```shell
# Two-step class-iNCD on CIFAR-100 (80-10-10)
sh test_cifar100/test_FRoST_2step_incd.sh

# Two-step class-iNCD on TinyImagenet (180-10-10)
sh test_tinyimagenet/test_FRoST_2step_incd.sh
```

## Evaluation Results
Table 1: Comparison with state-of-the-art methods in class-iNCD

![](figures/results_SOTA-HM.png)

Table 2: Comparison with the state-of-the-art methods in the two-step class-iNCD setting where new classes arrive in two episodes, instead of one. New-1-J: new classes performance from joint head at first step, New-1-N: new classes performance from novel head at first step, etc

![](figures/results_2step-iNCD.png)

Table 3: Ablation study on the proposed feature distillation (FD), feature replay (FR) and self-training (ST) that form our FRoST

![](figures/results_ablation.png)

Table 4: Ablation study comparing FRoST with LwF (logits-KD)

![](figures/results_LwF.png)

Table 5: Ablation study on having a single and separated heads for old and new classes. Joint: class-agnostic head; Novel: new classes classifier head

![](figures/results_heads.png)


## Citation
```
@inproceedings{roy2022class,
  title={Class-incremental Novel Class Discovery},
  author={Roy, Subhankar and Liu, Mingxuan and Zhong, Zhun and Sebe, Nicu and Ricci, Elisa},
  booktitle={Proceedings of the European Conference on Computer Vision (ECCV)},
  year={2022}}
```


