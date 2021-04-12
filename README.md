FastDepth
============================

This repo is re-implentation of FastDepth project at MIT, we present up to date code, with extra trained models based on different backbones and different loss functions. we provide pretrained models. and insturctions how to train and evalute models.

we also provide demo for depth estimation, and code for semantic segmentation that is available in FastSeg directory.



## Contents
0. [Requirements](#requirements)
0. [Trained Models](#trained-models)
0. [Training](#training)
0. [Evaluation](#evaluation)
0. [Demo](#demo)
0. [Results](#results)
0. [Reference](#reference)

## Requirements
## Setup

 - Clone repository:
    ```bash
    git clone https://github.com/Hagaik92/FastDepth.git
   
    cd /path/to/this/repository
    ```
- Install [PyTorch](https://pytorch.org/)
- Install the [HDF5](https://en.wikipedia.org/wiki/Hierarchical_Data_Format) format libraries. Files in our pre-processed datasets are in HDF5 format.
- Install Tensorboard [Tensorboard](https://pypi.org/project/tensorboard)
  ```bash
  sudo apt-get update
  sudo apt-get install -y libhdf5-serial-dev hdf5-tools
  pip3 install h5py matplotlib imageio scikit-image opencv-python
  ```
- Download the preprocessed [NYU Depth V2](http://cs.nyu.edu/~silberman/datasets/nyu_depth_v2.html) dataset in HDF5 format. The NYU dataset requires 32G of storage space.
	```bash
	cd FastDepth
	python3 prepare_dataset.py ../FastDepth
	```
downloading and unziping takes a while, please be patient.
## Trained Models ##
  The following trained models can be found at 'Weights' directory.
  - MobileNet-NNConv5(depthwise) with additive skip connections. Loss function: L1
  - MobileNetV2-NNConv5(depthwise) with additive skip connections. Loss function: L1
  - MobileNet-NNConv5(depthwise) with additive skip connections. Loss function: RMSLE+ Gradient+ Normal - this one turns out to be heavy, thus, download is available here:[link](https://drive.google.com/file/d/1HXClRN7_nQvfTQahpOIUPGc-RZhRluhX/view?usp=sharing)
  - MobileNetV2-NNConv5(depthwise) with additive skip connections. Loss function: RMSLE+ Gradient+ Normal
  - MobileNet-NNConv5(depthwise) with additive skip connections. Loss function: L1+ Gradient+ Normal
  - MobileNetV2-NNConv5(depthwise), with additive skip connections. Loss function: L1+ Gradient+ Normal

## Training ##
to train a new model. run the following command:
```bash
python3 main.py -mode train -backbone [encoder_type] --criterion [criterion] --gpu True
```
change encoder_type to one of the following: [mobilenet, mobilenetv2]

change the criterion you would like to use for training, option are: [l1, l1gn, rmslegn]

all checkpoints saved to the path - FastDepth/Weights/[backbone]/[criterion]/

please make sure that the directory exists before running train. for example, if you would like to use mobilenet as a backbone and l1 as a criterion, make sure that the directory 'FastDepth/Weights/mobilenet/l1 ' - exists.

if you would like to resume your training, run the following code:
```bash
python3 main.py -mode train -backbone [encoder_type] --criterion [criterion] --gpu True --resume [path_to_checkpoint]
```
### Pretrained MobileNet ###

-The model file for the pretrained MobileNet used in our model definition can be downloaded from [http://datasets.lids.mit.edu/fastdepth/imagenet/](http://datasets.lids.mit.edu/fastdepth/imagenet/).
when downloading it, make sure to put the tar file in 'Weights' directory
-The model file for pretrained MobileNetV2 is available in 'Weights' directory.

## Evaluation ##

to evalaute an existing model, weights should be inside the 'Weights' directory. use the following command to run evaluation.

```bash
python3 main.py -mode eval -backbone [mobilenet or mobilenetv2] --criterion [criterion] --pretrained [model_weights_filename] --gpu True
```
change criterion to one of the following options: [l1, l1gn, rmslegn]

The evaluation code will report model accuracy in terms of the delta1 metric as well as RMSE metric.

## Demo
we provide a a colab demonstartion to visual our results for depth estimation.
run FastDepthDemo.ipynb on your favorite framework, and follow the insturctions, make sure you are using GPU.

## Results

All results avaiable here: [link to pdf](https://drive.google.com/file/d/1Ws-wl00jpNyFQzDz-7gIQf0yQPuOQLur/view?usp=sharing)

## Referece

	@inproceedings{icra_2019_fastdepth,
		author      = {{Wofk, Diana and Ma, Fangchang and Yang, Tien-Ju and Karaman, Sertac and Sze, Vivienne}},
		title       = {{FastDepth: Fast Monocular Depth Estimation on Embedded Systems}},
		booktitle   = {{IEEE International Conference on Robotics and Automation (ICRA)}},
		year        = {{2019}}
	}
