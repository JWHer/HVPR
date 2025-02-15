# "WORKING" PyTorch implementation of "HVPR: Hybrid Voxel-Point Representation for Single-stage 3D Object Detection"

<p align="center"><img src="./HVPR_files/overview.png" alt="no_image" width="98%" height="60%" /></p>

## Installation

### Docker
Recommend to create isolated environment
```bash
nvidia-docker run -it --shm-size 16g --name HVPR --gpus all -v /mnt/hdd1/:/data djiajun1206/pcdet:python3.8_pytorch1.10 /bin/bash
```

### Git Clone
```bash
# git clone https://github.com/cvlab-yonsei/HVPR.git
git clone https://github.com/JWHer/HVPR.git
```

### Install dependencies
I wrote an exact version of I had run.  
It supposed to use container `djiajun1206/pcdet:python3.8_pytorch1.10`  
(If you get right, It will also works with `python3.8` and `pytorch1.10`)  

```bash
pip install -r requirment.txt
```


### SpConv
```bash
git clone https://github.com/traveller59/spconv.git --recursive
git checkout v1.2.1
git submodule update

python setup.py bdist_wheel
cd ./dist && pip install xxx.whl
```

### OpenPCDet
I'd use master branch. commit hash `255db8f02a8bd07211d2c91f54602d63c4c93356`

```bash
git clone https://github.com/open-mmlab/OpenPCDet.git
```

In this step, It quite difficult to follow now...
1. Copy all `OpenPCDet` files on original folder `HVPR/pcdet`.
You must preserve original repo files.
2. create `__init__.py` for missing folders for original `HVPR/pcdet` folders.
3. run `python setup.py develop`

### Dataset Prepare
```bash
python -m pcdet.datasets.kitti.kitti_dataset create_kitti_infos tools/cfgs/dataset_configs/kitti_dataset.yaml

kitti
|-- ImageSets
|-- gt_database
|-- kitti_dbinfos_train.pkl
|-- kitti_infos_test.pkl
|-- kitti_infos_train.pkl
|-- kitti_infos_trainval.pkl
|-- kitti_infos_val.pkl
|-- testing -> /data/kitti/testing/
`-- training -> /data/kitti/training/
```

nuscenes lidar only setting
```bash
python -m pcdet.datasets.nuscenes.nuscenes_dataset --func create_nuscenes_infos --cfg_file tools/cfgs/dataset_configs/nuscenes_dataset.yaml --version v1.0-trainval

nuscenes/
`-- v1.0-trainval
    |-- gt_database_10sweeps_withvelo
    |-- maps -> /data/nuscenes/maps
    |-- nuscenes_dbinfos_10sweeps_withvelo.pkl
    |-- nuscenes_infos_10sweeps_train.pkl
    |-- nuscenes_infos_10sweeps_val.pkl
    |-- samples -> /data/nuscenes/samples
    |-- sweeps -> /data/nuscenes/sweeps
    `-- v1.0-trainval -> /data/nuscenes/v1.0-trainval
```

### All Done!
Good Job! Now you can run your HVPR codes.
(It provides some `.vscode/launch.json` files for you)

---
original doc

This is the implementation of the paper "HVPR: Hybrid Voxel-Point Representation for Single-stage 3D Object Detection (CVPR 2021)".

Our code is mainly based on [`OpenPCDet`](https://github.com/open-mmlab/OpenPCDet). We also plan to release the code based on [`PointPillars`](https://github.com/nutonomy/second.pytorch). 
For more information, checkout the project site [[website](https://cvlab.yonsei.ac.kr/projects/HVPR/)] and the paper [[PDF](https://openaccess.thecvf.com/content/CVPR2021/papers/Noh_HVPR_Hybrid_Voxel-Point_Representation_for_Single-Stage_3D_Object_Detection_CVPR_2021_paper.pdf)].

## Dependencies
* Python >= 3.6
* PyTorch >= 1.4.0

## Update
* 20/06/21: First update

## Installation
* Clone this repo, and follow the steps below (or you can follow the installation steps in [`OpenPCDet`](https://github.com/open-mmlab/OpenPCDet)).
1. Clone this repository:
   ```bash
   git clone https://github.com/cvlab-yonsei/HVPR.git
   ```
2. Install the dependent libraries:
   ```bash
   pip install -r requirements.txt
   ```
3. Install the `SparseConv` library from
[`spconv`](https://github.com/traveller59/spconv).

4. Install `pcdet` library:
   ```bash
   python setup.py develop
   ```
## Datasets
* KITTI 3D Object Detection 

1. Please download the official [KITTI 3D object detection](http://www.cvlibs.net/datasets/kitti/eval_object.php?obj_benchmark=3d) dataset and organize the downloaded files as follows (the road planes could be downloaded from [[road plane]](https://drive.google.com/file/d/1d5mq0RXRnvHPVeKx6Q612z0YRO1t2wAp/view?usp=sharing), which are optional for data augmentation in the training):
     ```bash
     HVPR
     ├── data
     │   ├── kitti
     │   │   │── ImageSets
     │   │   │── training
     │   │   │   ├──calib & velodyne & label_2 & image_2 & (optional: planes)
     │   │   │── testing
     │   │   │   ├──calib & velodyne & image_2
     ├── pcdet
     ├── tools
     ```
2. Generate the data infos by running the following command:
    ```bash
    python -m pcdet.datasets.kitti.kitti_dataset create_kitti_infos tools/cfgs/dataset_configs/kitti_dataset.yaml
    ```
## Training
* The config files is in tools/cfgs/kitti_models, and you can easily train your own model like:
  ```bash
  cd tools
  sh scripts/train_hvpr.sh 
  ```
* You can freely define parameters with your own settings like:
  ```bash
  cd tools
  sh scripts train_hvpr.sh --gpus 1 --result_path 'your_dataset_directory' --exp_dir 'your_log_directory'
  ```

## Evaluation
* Test your own model:
  ```bash
  cd tools
  sh scripts/eval_hvpr.sh
  ```


## Pre-trained model
* Download our pre-trained model.
<br>[[KITTI 3D Car]()]

## Bibtex
```
@article{noh2021hvpr,
  title={HVPR: Hybrid Voxel-Point Representation for Single-stage 3D Object Detection},
  author={Noh, Jongyoun and Lee, Sanghoon and Ham, Bumsub},
  journal={arXiv preprint arXiv:2104.00902},
  year={2021}
}
```
## References
Our work is mainly built on [`OpenPCDet`](https://github.com/open-mmlab/OpenPCDet) codebase. Portions of our code are also borrowed from [`spconv`](https://github.com/traveller59/spconv), [`MemAE`](https://github.com/donggong1/memae-anomaly-detection), and [`CBAM`](https://github.com/Jongchan/attention-module). Thanks to the authors!
