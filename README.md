### Dependencies and Installation

- Pytorch >= 1.7.1
- CUDA >= 10.1
- Other required packages in `requirements.txt`
```
# git clone this repository
git clone https://github.com/sczhou/CodeFormer
cd CodeFormer

# create new anaconda env
conda create -n codeformer python=3.8 -y
conda activate codeformer

# install python dependencies
pip3 install -r requirements.txt
python basicsr/setup.py develop
conda install -c conda-forge dlib (only for face detection or cropping with dlib)
```
<!-- conda install -c conda-forge dlib -->

### Quick Inference

#### Download Pre-trained Models:
Download the facelib and dlib pretrained models from [[Releases](https://github.com/sczhou/CodeFormer/releases/tag/v0.1.0) | [Google Drive](https://drive.google.com/drive/folders/1b_3qwrzY_kTQh0-SnBoGBgOrJ_PLZSKm?usp=sharing) | [OneDrive](https://entuedu-my.sharepoint.com/:f:/g/personal/s200094_e_ntu_edu_sg/EvDxR7FcAbZMp_MA9ouq7aQB8XTppMb3-T0uGZ_2anI2mg?e=DXsJFo)] to the `weights/facelib` folder. You can manually download the pretrained models OR download by running the following command:
```
python scripts/download_pretrained_models.py facelib
python scripts/download_pretrained_models.py dlib (only for dlib face detector)
```

Download the CodeFormer pretrained models from [[Releases](https://github.com/sczhou/CodeFormer/releases/tag/v0.1.0) | [Google Drive](https://drive.google.com/drive/folders/1CNNByjHDFt0b95q54yMVp6Ifo5iuU6QS?usp=sharing) | [OneDrive](https://entuedu-my.sharepoint.com/:f:/g/personal/s200094_e_ntu_edu_sg/EoKFj4wo8cdIn2-TY2IV6CYBhZ0pIG4kUOeHdPR_A5nlbg?e=AO8UN9)] to the `weights/CodeFormer` folder. You can manually download the pretrained models OR download by running the following command:
```
python scripts/download_pretrained_models.py CodeFormer
```

#### Prepare Testing Data:
You can put the testing images in the `inputs/TestWhole` folder. If you would like to test on cropped and aligned faces, you can put them in the `inputs/cropped_faces` folder. You can get the cropped and aligned faces by running the following command:
```
# you may need to install dlib via: conda install -c conda-forge dlib
python scripts/crop_align_face.py -i [input folder] -o [output folder]
```


#### Testing:
[Note] If you want to compare CodeFormer in your paper, please run the following command indicating `--has_aligned` (for cropped and aligned face), as the command for the whole image will involve a process of face-background fusion that may damage hair texture on the boundary, which leads to unfair comparison.

Fidelity weight *w* lays in [0, 1]. Generally, smaller *w* tends to produce a higher-quality result, while larger *w* yields a higher-fidelity result. The results will be saved in the `results` folder.


🧑🏻 Face Restoration (cropped and aligned face)
```
# For cropped and aligned faces (512x512)
python inference_codeformer.py -w 0.5 --has_aligned --input_path [image folder]|[image path]
```

:framed_picture: Whole Image Enhancement
```
# For whole image
# Add '--bg_upsampler realesrgan' to enhance the background regions with Real-ESRGAN
# Add '--face_upsample' to further upsample restorated face with Real-ESRGAN
python inference_codeformer.py -w 0.7 --input_path [image folder]|[image path]
```

:clapper: Video Enhancement
```
# For Windows/Mac users, please install ffmpeg first
conda install -c conda-forge ffmpeg
```
```
# For video clips
# Video path should end with '.mp4'|'.mov'|'.avi'
python inference_codeformer.py --bg_upsampler realesrgan --face_upsample -w 1.0 --input_path [video path]
