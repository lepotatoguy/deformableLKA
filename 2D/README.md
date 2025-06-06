# 2D Deformable LKA

Instructions for the 2D version of the D-LKA net.




## Model weights
You can download the learned weights of the D-LKA Net in the following table. 

Task | Dataset |Learned weights
------------ | -------------|----
Multi organ segmentation | [Synapse](https://drive.google.com/uc?export=download&id=18I9JHH_i0uuEDg-N6d7bfMdf7Ut6bhBi) | [D-LKA Net 2D](https://drive.google.com/drive/folders/1TY7G0X32kGbgnzx_Zn5px0gW8fVF4ptI?usp=sharing)
Skin 2017 | [Skin Dataset](https://challenge.isic-archive.com/data/#2017) | D-LKA Net TODO
Skin 2018 | [Skin Dataset](https://challenge.isic-archive.com/data/#2018) | D-LKA Net TODO
PH2       | Skin Dataset | D-LKA Net TODO

## Environment Setup
0. Please ensure to install CUDA = 11.3 and GCC = 10 before getting started.
   If you want to keep in conda, 
   install 
   ```bash
    conda install cudatoolkit=11.3 -c conda-forge
    ```

1. Create a new conda environment with python version 3.8.16:
    ```bash
    conda create -n "d_lka_net_2d" python=3.8.16
    conda activate d_lka_net_2d
    ```
2. Install PyTorch and torchvision
    ```bash
    pip install torch==1.11.0+cu113 torchvision==0.12.0+cu113 torchaudio==0.11.0+cu113 --extra-index-url https://download.pytorch.org/whl/cu113
    ```
3. Install the requirements with:
    ```bash
    pip install -r requirements.txt
    ```
    While installing, mmcv-full can take time. To avoid this, you can, 
    ```bash
    pip install mmcv-full==1.7.1 -f https://download.openmmlab.com/mmcv/dist/cu113/torch1.11.0/index.html
    ```

## Synapse Dataset
### Training and Testing
1. Download the Synapse dataset from the link above.

1.1. Download the pre-trained model weight (we use "maxvit_rmlp_small_rw_224_sw-6ef0ae4f.pth" here) provided from [here](https://drive.google.com/drive/folders/1k-s75ZosvpRGZEWl9UEpc_mniK3nL2xq). Then create a folder named pretrained_pth. And then move the weight there. Otherwise, you may get into an error (i.e. [here](https://github.com/xmindflow/deformableLKA/issues/16)). [More Info](https://github.com/xmindflow/deformableLKA/issues/10)

1. Run the code below to train D-LKA Net on the Synapse dataset.
    ```bash
    python train_MaxViT_deform_LKA.py --root_path ./data/Synapse/train_npz --test_path ./data/Synapse/test_vol_h5 --batch_size 20 --eval_interval 20 # (I changed batch size to 6)
    ```
    **--root_path**     [Train data path]

    **--test_path**     [Test data path]

    **--eval_interval** [Evaluation epoch]

    Reduce the batch size if you face OOM error (such as in personal devices where you may have less ram).

2. Run the below code to test the D-LKA Net on the Synapse dataset.
    ```bash
    python test.py --volume_path ./data/Synapse/ --output_dir './model_out'
    ```
    **--volume_path**   [Root dir of the test data]
        
    **--output_dir**    [Directory of your learned weights]

## Skin Dataset
Examples are given for the Skin2017 dataset. The other datasets work exactly the same.
### Data Preparation
1. Download the dataset from the link above.
2. Prepare the data. Adjust the filespath in the preparation file accordingly.
    ```bash
    cd 2D/skin_code
    python Prepare_ISIC_2017.py
    ```

    The Data structure should be as follows:
    ```
    -ISIC2017
      --/data_train.npy
      --/data_test.npy
      --/data_val.npy
      --/mask_train.npy
      --/mask_test.npy
      --/mask_val.npy
    ```
### Training and Testing
1. Adjust the path in the **train_skin_2017.py** file for your paths.
2. Run the following line of code:
    ```bash
    python train_skin_2017.py
    ```
3. For evaluation follow the instruction in the jupyter notebook **evaluate_skin.ipynb**
