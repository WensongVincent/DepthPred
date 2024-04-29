# ZoeDAM

Depth Anything models focused on robust *relative* depth estimation. To achieve *metric* depth estimation, we follow ZoeDepth to fine-tune from Depth Anything pre-trained encoder with metric depth information from NYUv2.


| Method | $\delta_1 \uparrow$ | $\delta_2 \uparrow$ | $\delta_3 \uparrow$ | AbsRel $\downarrow$ | RMSE $\downarrow$ | log10 $\downarrow$ |
|:-:|:-:|:-:|:-:|:-:|:-:|:-:|
| ZoeDepth | 0.955 | 0.995 | 0.999 | 0.075 | 0.270 | 0.032 |
| ZoeDAM-L | **0.984** | **0.998** | **1.000** | **0.056** | **0.206** | **0.024** |
| ZoeDAM-B (Ours) | 0.976 | 0.997 | **1.000** | 0.061 | 0.254 | 0.029 |


## Installation

```bash
conda env create -n zoedam python=3.9.7
conda activate depth_anything_metric
pip install -r requirements.txt
```

Please follow [ZoeDepth](https://github.com/isl-org/ZoeDepth) to prepare the training and test datasets.

## Training

Please first download our Depth Anything pre-trained model [here](https://huggingface.co/spaces/LiheYoung/Depth-Anything/blob/main/checkpoints/depth_anything_vitl14.pth), and put it under the ``checkpoints`` directory.

```bash
python train_mono.py -m zoedepth -d nyu --pretrained_resource=""
```

This will automatically use our Depth Anything pre-trained ViT-L encoder.

## Evaluation

Make sure you have downloaded our pre-trained metric-depth models [here](https://drive.google.com/drive/u/0/folders/1Zy1AVGhMWRFUrDaaegoy6A_EgBxuAJOp) (for evaluation) and pre-trained relative-depth model [here](https://huggingface.co/spaces/LiheYoung/Depth-Anything/blob/main/checkpoints/depth_anything_vitl14.pth) (for initializing the encoder) and put them under the ``checkpoints`` directory.

Indoor:
```bash
python evaluate.py -m zoedepth --pretrained_resource="local::./checkpoints/depth_anything_metric_depth_indoor.pt" -d <nyu | sunrgbd | ibims | hypersim_test>
```
