Video Object Detection – MEGA.PyTorch Setup (CUDA 10.0 / PyTorch 1.2.0)

Authors: Helena Narganes Navares & Javier Collado Orellana
Course: Deep Learning for Video Signal Processing (Lab Session 2 – Video Object Detection)

This repository provides a **fully functional and tested setup** for running **MEGA.PyTorch** on a clean Conda environment with **CUDA 10.0**, **PyTorch 1.2.0**, and **no dependency on NVIDIA Apex**.  

---

## 1. Environment Setup

### Check your Conda configuration

Before installing, make sure Conda and Python point to the same environment:

```bash
which conda
which pip
which python
```

They should all refer to the same environment path.

Create a clean Conda environment:

If you want to create it in a specific path, use:

```bash
conda create --prefix your/environment/path/MEGA python=3.7 -y
source activate your/environment/path/MEGA
```

If not:
```bash
conda create --name MEGA -y python=3.7
source activate MEGA
```

Install base dependencies:

```bash
conda install ipython pip
pip install ninja yacs cython matplotlib tqdm opencv-python scipy
```

---

## 2. Install PyTorch and TorchVision

For this project, use the following verified configuration (CUDA 10.0 compatible):

```bash
conda install pytorch=1.2.0 torchvision=0.4.0 cudatoolkit=10.0 -c pytorch
```

Note: We explicitly use PyTorch 1.2.0 instead of 1.3.0 to ensure compatibility with MEGA.

---

## 3. Install External Dependencies

Set a working variable for your installation directory:
 - Choose the path where you want to download all the files; it doesn't need to be the same as the environment path. Just remember to have activated your environment. 

```bash
export INSTALL_DIR=$(pwd)
```

a. COCO API

```bash
cd $INSTALL_DIR
git clone https://github.com/cocodataset/cocoapi.git
cd cocoapi/PythonAPI
python setup.py build_ext install
```

b. Cityscapes Scripts

```bash
cd $INSTALL_DIR
git clone https://github.com/mcordts/cityscapesScripts.git
cd cityscapesScripts/
python setup.py build_ext install
```

c. MEGA.PyTorch

```bash
cd $INSTALL_DIR
git clone https://github.com/j19co/MEGA-Modified-Version
cd MEGA-Modified-Version
python setup.py build develop
```

Finally, install a compatible version of Pillow:

```bash
pip install 'pillow<7.0.0'
```

Unset the installation variable when done:

```bash
unset INSTALL_DIR
```

---

## 4. Download the images, videos and models: R_101 and MEGA_R_101 (These two have to be inside mega.pytorch)

| Model                 | Backbone    | AP50 | AP (fast) | AP (med) | AP (slow) | Link   |
|-----------------------|-------------|------|-----------|----------|-----------|--------|
| single frame baseline | ResNet-101  | 76.7 | 52.3      | 74.1     | 84.9      | [Google](https://drive.google.com/file/d/1W17f9GC60rHU47lUeOEfU--Ra-LTw3Tq/view) |
| MEGA                  | ResNet-101  | 82.9 | 62.7      | 81.6     | 89.4      | [Google](https://drive.google.com/file/d/1ZnAdFafF1vW9Lnpw-RPF1AD_csw61lBY/view) |

---

## 5. Paths and Folder Configuration

Set the following directories for image input and output visualization.

Image input path:
```
your/working/path/MEGA/image_folder
```
Output folders:

Baseline results: ```./MEGA/image_folder/visualization_baseline```

MEGA results:     ```./MEGA/image_folder/visualization_mega```

---

## 6. Running the Inference Demo

You can run either the baseline model or the MEGA model visualization using the following commands.

- Baseline Image Inference:
```
python demo/demo.py base configs/vid_R_101_C4_1x.yaml R_101.pth \
--suffix ".JPEG" \
--visualize-path your/working/path/MEGA/image_folder \
--output-folder visualization_baseline
```
- MEGA Image Inference:
```
python demo/demo.py mega configs/vid_R_101_C4_MEGA_1x.yaml MEGA_R_101.pth \
--suffix ".JPEG" \
--visualize-path your/working/path/MEGA/image_folder \
--output-folder visualization_mega
```

VIDEO NAMES: v_HorseRiding_g10_c01.avi, v_WalkingWithDog_g01_c01.avi, v_WalkingWithDog_g10_c03.avi
FOLDER NAMES: HorseRiding_g10_c01, WalkingWithDog_g01_c01, WalkingWithDog_g10_c03
(substitute "video_name" and "folder_name" coherently for each case)

- Baseline Video Inference:
```
    python demo/demo.py base configs/MEGA/vid_R_101_C4_MEGA_1x.yaml R_101.pth --video \
        --visualize-path your/working/path/MEGA/video_folder/video_name \
        --output-folder your/working/path/MEGA/video_folder/visualization_baseline/folder_name     
```           
- MEGA Video Inference:
```
    python demo/demo.py mega configs/MEGA/vid_R_101_C4_MEGA_1x.yaml MEGA_R_101.pth --video \
        --visualize-path your/working/path/MEGA/video_folder/video_name \
        --output-folder your/working/path/MEGA/video_folder/visualization_mega/folder_name
```
---

## 7. Additional Notes

Tested successfully on Ubuntu 18.04, Python 3.7, CUDA 10.0, PyTorch 1.2.0.

Apex is not required; all mixed precision (AMP) references have been safely removed.

If using a different CUDA version, check version compatibility at PyTorch.org.

Ensure your GPU drivers support CUDA 10.0 before installation.
