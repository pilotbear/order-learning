# [ICLR 2020] Order Learning and Its Application to Age Estimation
![Lim2020Order](img/test_result.JPG)
## Paper
[**Order Learning and Its Application to Age Estimation**](https://openreview.net/pdf?id=HygsuaNFwr)

We propose order learning to determine the order graph of classes, representing ranks or priorities, and classify an object instance into one of the classes. To this end, we design a pairwise comparator to categorize the relationship between two instances into one of three cases: one instance is 'greater than,' 'similar to,' or 'smaller than' the other. Then, by comparing an input instance with reference instances and maximizing the consistency among the comparison results, the class of the input can be estimated reliably. We apply order learning to develop a facial age estimator, which provides the state-of-the-art performance. Moreover, the performance is further improved when the order graph is divided into disjoint chains using gender and ethnic group information or even in an unsupervised manner. 

The full paper can be found via the link above.

Please cite our paper if you use our code or dataset:
```
@inproceedings{
Lim2020Order,
title={Order Learning and Its Application to Age Estimation},
author={Kyungsun Lim and Nyeong-Ho Shin and Young-Yoon Lee and Chang-Su Kim},
booktitle={International Conference on Learning Representations},
year={2020},
url={https://openreview.net/forum?id=HygsuaNFwr}
}
```

## Dataset
We form the ‘balanced dataset’ from MORPH II [1], AFAD [2], and UTK [3]. Before sampling images from MORPH II, AFAD, and UTK, we rectify inconsistent labels by following the strategy in [4]. For each combination of gender in {female, male} and ethnic group in {African, Asian, European}, we sample about 6,000 images. Also, during the sampling, we attempt to make the age distribution as uniform as possible within range [15,80]. The balanced dataset is partitioned into training and test subsets with ratio 8 : 2. 

![Lim2020Order](img/balanced_dataset.JPG)

You should download MORPH II, AFAD, and UTK dataset before using the balanced dataset.

Downloads links are followed:  
MORPH II[[Link](https://ebill.uncw.edu/C20231_ustores/web/classic/product_detail.jsp?PRODUCTID=8)]  AFAD[[Link](https://afad-dataset.github.io/)] UTK[[Link](https://susanqq.github.io/UTKFace/)]

We also upload the whole MORPH II fold indices for the following 4 experimental settings.  
* Setting A: 5,492 images of Europeans are randomly selected and then divided into training and testing sets with ratio 8:2
* Setting B: About 21,000 images are randomly selected, while restricting the ratio between Africans and Europeans to 1:1 and that between females and males to 1:3. They are divided into three subsets (S1, S2, S3). The training and testing are done under two sub-settings
  * (B1) training on S1, testing on S2 + S3
  * (B2) training on S2, testing on S1 + S3
* Setting C (SE): The entire dataset is randomly split into five folds, subject to the constraint that the same persons images should belong to only one fold, and the 5-fold cross-validation is performed.
* Setting D (RS): The entire dataset is randomly split into five folds without any constraint, and the
5-fold cross-validation is performed.

## Dependencies
* Python3
* Tensorflow v1
* Pandas
* MORPH II
* AFAD
* UTK

## Preprocessing
We use SeetaFaceDetection [5][[Link](https://github.com/seetaface/SeetaFaceEngine)] for face detection and face alignment code provided from pyimagesearch [[Link](https://www.pyimagesearch.com/2017/05/22/face-alignment-with-opencv-and-python/)] for face alignment.

## Supervised training
### Usage
You can download pre-trained models here [[Link](https://drive.google.com/file/d/1kOJfZbYU-3mSEOISaDGi_JjLehPSBVF6/view?usp=sharing)]. Use the following command for training.
```
python comparator_train.py --chain The_number_of_chains --experimental_setting Experimental_setting --data_path img_dir --dataset Dataset_for_evaluation --initial_model_path pre-trained_model_dir --save_dir dir_for_saving_model --log_dir dir_for_saving_logs
```

## Unsupervised training
#### Coming Soon...

## Reference Selection
### Usage
Use the following command for reference selection.
```
python comparator_select_references.py --chain The_number_of_chains --experimental_setting Experimental_setting --data_path img_dir --dataset Dataset_for_evaluation --save_dir dir_for_saving_ref_data
```

## Test
### Usage
You can download trained models here [[Link](https://drive.google.com/open?id=1WzGjwC2YeGgnuq5ni-34byRDaXjU8b2N)]. Use the following command for evaluation.
```
python comparator_test.py --chain The_number_of_chains --experimental_setting Experimental_setting --dataset Dataset_for_evaluation
```
#### Balanced Dataset
* Dataset_for_evaluation : 'Balanced'
* The_number_of_chains : '1CH', '2CH', '3CH', '6CH'
* Experimental_setting : 'Supervised', 'Unsupervised'

#### MORPH II Dataset
* Dataset_for_evaluation : 'Morph'
* The_number_of_chains : '1CH'
* Experimental_setting : 'A', 'B', 'C', 'D'


## Results
### Balanced Dataset
Chain| 1CH | 2CH | 3CH | 6CH|
:--------|:--------:|:--------:|:--------:|:--------:|
Superivsed|4.23 / 73.2|4.18 / 73.4|4.19 / 73.4|4.18 / 73.4|
Unsupervised|-|4.16 / 74.0|4.17 / 73.9|4.16 / 74.0|


### MORPH II Dataset
Settting(1CH) | A | B | C | D|
:--------|:--------:|:--------:|:--------:|:--------:|
MAE|2.41|2.75|2.68|2.22|
CS(%)|91.7|88.2|88.8|93.3|

## Reference
[1] Karl Ricanek and Tamirat Tesafaye. MORPH: A longitudinal image database of normal adult age-progression. In FGR, pp. 341–345, 2006.  
[2] Zhenxing Niu, Mo Zhou, Le Wang, Xinbo Gao, and Gang Hua. Ordinal regression with multiple output CNN for age estimation. In CVPR, pp. 4920–4928, 2016.  
[3] Zhifei Zhang, Yang Song, and Hairong Qi. Age progression/regression by conditional adversarial autoencoder. In CVPR, 2017.  
[4] Benjamin Yip, Garrett Bingham, Katherine Kempfert, Jonathan Fabish, Troy Kling, Cuixian Chen, and Yishi Wang. Preliminary studies on a large face database. In ICBD, pp. 2572–2579, 2018.  
[5] Jie Zhang, Shiguang Shan, Meina Kan, and Xilin Chen. Coarse-to-fine auto-encoder networks (CFAN) for real-time face alignment. In ECCV, pp. 1–16, 2014.
