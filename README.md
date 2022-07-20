# Cross-Domain Few-Shot Semantic Segmentation (CD-FSS)

## Introduction

The Cross-Domain Few-Shot Semantic Segmentation includes data from the CropDiseases [1], EuroSAT [2], ISIC2018 [3-4], and ChestX [5] datasets, which covers plant disease images, satellite images, dermoscopic images of skin lesions, and X-ray images, respectively. The selected datasets reflect real-world use cases for few-shot learning since collecting enough examples from above domains is often difficult, expensive, or in some cases not possible. In addition, they demonstrate the following spectrum of readily quantifiable domain shifts from ImageNet: 1) CropDiseases images are most similar as they include perspective color images of natural elements, but are more specialized than anything available in ImageNet, 2) EuroSAT images are less similar as they have lost perspective distortion, but are still color images of natural scenes, 3) ISIC2018 images are even less similar as they have lost perspective distortion and no longer represent natural scenes, and 4) ChestX images are the most dissimilar as they have lost perspective distortion, all color, and do not represent natural scenes.

## Datasets
The following datasets are used for evaluation in CD-FSS:

### Source domain: 

* **PASCAL VOC2012**:

    Download PASCAL VOC2012 devkit (train/val data):
    ```bash
    wget http://host.robots.ox.ac.uk/pascal/VOC/voc2012/VOCtrainval_11-May-2012.tar
    ```
    Download PASCAL VOC2012 SDS extended mask annotations from [[Google Drive](https://drive.google.com/file/d/10zxG2VExoEZUeyQl_uXga2OWHjGeZaf2/view?usp=sharing)].

### Target domains: 

* **Deepglobe**:

    Home: http://deepglobe.org/

    Direct: https://www.kaggle.com/datasets/balraj98/deepglobe-land-cover-classification-dataset

* **ISIC2018**:

    Home: http://challenge2018.isic-archive.com

    Direct (must login): https://challenge.isic-archive.com/data#2018

* **Chest X-ray**:

    Home: https://www.ncbi.nlm.nih.gov/pmc/articles/PMC4256233/

    Direct: https://www.kaggle.com/datasets/nikhilpandey360/chest-xray-masks-and-labels

* **FSS-1000**:

    Home: https://github.com/HKUSTCV/FSS-1000

    Direct: https://drive.google.com/file/d/16TgqOeI_0P41Eh3jWQlxlRXG9KIqtMgI/view

## Requirements

- Python 3.7
- PyTorch 1.5.1
- cuda 10.1
- tensorboard 1.14

Conda environment settings:
```bash
conda create -n patnet python=3.7
conda activate patnet

conda install pytorch=1.5.1 torchvision cudatoolkit=10.1 -c pytorch
conda install -c conda-forge tensorflow
pip install tensorboardX
```

## Training
> ### PASCAL VOC
> ```bash
> python train.py --backbone {vgg16, resnet50} 
>                 --fold 4 
>                 --benchmark pascal
>                 --lr 1e-3
>                 --bsz 20
>                 --logpath "your_experiment_name"
> ```

## Testing

> ### 1. Deepglobe
> ```bash
> python test.py --backbone {vgg16, resnet50} 
>                --fold {0, 1, 2, 3} 
>                --benchmark Deepglobe
>                --nshot {1, 5} 
>                --load "path_to_trained_model/best_model.pt"
> ```


> ### 2. ISIC
> ```bash
> python test.py --backbone {vgg16, resnet50} 
>                --fold {0, 1, 2, 3} 
>                --benchmark ISIC 
>                --nshot {1, 5} 
>                --load "path_to_trained_model/best_model.pt"
> ```

> ### 3. Chest X-ray
> ```bash
> python test.py --backbone {vgg16, resnet50} 
>                --benchmark Lung 
>                --nshot {1, 5} 
>                --load "path_to_trained_model/best_model.pt"
> ```

> ### 4. FSS-1000
> ```bash
> python test.py --backbone {vgg16, resnet50} 
>                --benchmark fss 
>                --nshot {1, 5} 
>                --load "path_to_trained_model/best_model.pt"
> ```

## Acknowledgement
The implementation is based on [HSNet](https://github.com/juhongm999/hsnet). <br>


[//]: # (### BibTeX)

[//]: # (Please cite the following paper in use of this evaluation framework:)

[//]: # (```
[//]: # (@inproceedings{guo2020broader,
[//]: # (  title={A broader study of cross-domain few-shot learning},
[//]: # (  author={Guo, Yunhui and Codella, Noel C and Karlinsky, Leonid and Codella, James V and Smith, John R and Saenko, Kate and Rosing, Tajana and Feris, Rogerio},
[//]: # (  year={2020},
[//]: # (  organization={ECCV}
[//]: # (})
[//]: # (```)

## References

[1] Demir, I., Koperski, K., Lindenbaum, D., Pang, G., Huang, J., Basu, S., Hughes,
F., Tuia, D., Raskar, R.: Deepglobe 2018: A challenge to parse the earth through
satellite images. In: The IEEE Conference on Computer Vision and Pattern Recog-
nition (CVPR) Workshops (June 2018)Li, X., Wei, T., Chen, Y.P., Tai, Y.W., Tang, C.K.: Fss-1000: A 1000-class dataset
for few-shot segmentation. In: Proceedings of the IEEE/CVF Conference on Com-
puter Vision and Pattern Recognition. pp. 2869–2878 (2020)

[2] Codella, N., Rotemberg, V., Tschandl, P., Celebi, M.E., Dusza, S., Gutman, D.,
Helba, B., Kalloo, A., Liopyris, K., Marchetti, M., et al.: Skin lesion analysis toward
melanoma detection 2018: A challenge hosted by the international skin imaging
collaboration (isic). arXiv preprint arXiv:1902.03368 (2019)

[3] Tschandl, P., Rosendahl, C., Kittler, H.: The ham10000 dataset, a large collection
of multi-source dermatoscopic images of common pigmented skin lesions. Scientific
data 5, 180161 (2018)

[4] Candemir, S., Jaeger, S., Palaniappan, K., Musco, J.P., Singh, R.K., Xue, Z.,
Karargyris, A., Antani, S., Thoma, G., McDonald, C.J.: Lung segmentation in
chest radiographs using anatomical atlases with nonrigid registration. IEEE trans-
actions on medical imaging 33(2), 577–590 (2013)

[5] Jaeger, S., Karargyris, A., Candemir, S., Folio, L., Siegelman, J., Callaghan, F.,
Xue, Z., Palaniappan, K., Singh, R.K., Antani, S., et al.: Automatic tuberculosis
screening using chest radiographs. IEEE transactions on medical imaging 33(2),
233–245 (2013)

[6] Li, X., Wei, T., Chen, Y.P., Tai, Y.W., Tang, C.K.: Fss-1000: A 1000-class dataset
for few-shot segmentation. In: Proceedings of the IEEE/CVF Conference on Com-
puter Vision and Pattern Recognition. pp. 2869–2878 (2020)
