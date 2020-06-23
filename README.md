# RandPerson
Surpassing Real-World Source Training Data: Random 3D Characters for Generalizable Person Re-Identification

![fig1](https://github.com/VideoObjectSearch/RandPerson/blob/master/img/unity.png)  

## Table of Contents

- [Dataset Description](#link-of-the-dataset)
- [Characters and Scenes](#characters-and-scenes)
- [Experimental Results](#experiments-results)
- [Citation](#citation)

## Dataset Description

The RandPerson dataset is generated by MakeHuman and Unity. For researching viewpoint, 8000 batch generation identities and 11 unity scenes are used in this version (as shown in the following figure), including eight outdoor and three indoorscenes. Due to the large amount of data, only a subset of the experiment is provided, currently.

Experiments subset images can be downloaded from the following links:<br>
* Baidu Yun Drive: 
	* images [link](https://pan.baidu.com/s/1peTSlhze9BzDQGbcakkz2w) (code:ueeg)<br>
* Google Drive: 
	* subset images [link](https://drive.google.com/file/d/12u1xdVo6-Q-i_knsbrBrRkClFkq10oNH/view?usp=sharing)<br>

Directories & Files of images
```shell
randperson_subset
├── 000000_c00_s00_f000264.jpg
├── 000000_c00_s00_f001032.jpg
├── 000000_c01_s00_f001632.jpg
```

Name of the image

Taking "000000_c00_s00_f000264.jpg" as an example: 
*  000000 is the id of the person
*  c00   is the id of the camera
*  s00   is the id of the scene
*  f000264   is the number of frames

## Characters and Scenes
MakeHuman (http://www.makehumancommunity.org/) is a free and open-source software used to create 3D character models. In MakeHuman, new 3D characters are created through simple drag and drop operations in the working panel, so as to combine various components together and adjust their attributes, such as skeleton, body parameters, clothes, etc. Fig. 2 shows an example of how to create a 3D character in MakeHuman. After UV mapping, the textured 3D components can be applied to the standard 3D human model to form a new individualized 3D character.
![fig2](https://github.com/VideoObjectSearch/RandPerson/blob/master/img/clothes.png)  

Since each clothing model comes with a UV texture map defining the clothing texture, altering this can significantly change the appearance of the clothing. Therefore, we propose to generate a large number of clothing models by altering the UV texture maps of available clothing models. We generate new UV texture maps in two ways. The first is to search and download images from the Internet,
and use them directly as new UV texture maps. Accordingly, 5,772 images are downloaded, including landscapes, animals, and texture patterns. However, most of these images are complicated, and the color distribution cannot be controlled. Therefore, alternatively, we design a method to generate random UV texture maps. Firstly, as shown in Fig. 3, a color palette is generated, which contains representative colors sampled from the RGB space. This is done using a regular grid in the 256 × 256 × 256 RGB space, with a step of 10, resulting in 25 × 25 × 25 = 15, 625 colors, then 625 representative colors are sampled from them as the generated color palette. Besides, 16 additional texture patterns are generated, which include different stripes and spots. By sampling a background color from the color palette, and then drawing a texture pattern on the background image, a new UV texture map is generated, which can make clothes look plaid or striped. Combining different colors and texture patterns results in 625×16 = 10, 000 UV texture maps.
![fig3](https://github.com/VideoObjectSearch/RandPerson/blob/master/img/color.png)  

Now that we have a great number of UV texture maps, a new clothing model can be created by choosing an existing model, and replacing its UV texture map with one randomly sample from the pool of our downloaded or generated UV texture maps. Examples of generated clothing can be seen in Fig. 4, which shows how different UV maps can be used to texturize the same clothing model, and how the generated clothes and rendered images look differently. By utilizing these large numbers of UV texture maps, we obtain a series of different outfits, which can be further used to create different 3D characters.

![fig4](https://github.com/VideoObjectSearch/RandPerson/blob/master/img/makehuman.png)  

Finally, we created 8,000 different 3D characters, including 114 characters with the original clothing models, 2,886 with UV texture maps from web images, and 5,000 with random UV maps. The character models created are saved as standard MakeHuman .mhm files, and can then be exported to several other file types to be used in animation software. In this work, we use the .fbx file type for the exported 3D character model.

Unity3D (https://unity.com/) is a cross-platform game engine that can be used to create various 3D games. We obtain a set of customized environments, comprising streets, forest paths, highways and laboratories, among others, with bright light, dark light, blue light, etc. Fig. 5 shows some example scenarios used in this work.
![fig5](https://github.com/VideoObjectSearch/RandPerson/blob/master/img/scene.png)  
## Experimental Results

To validate the effectiveness of the RandPerson dataset, we apply a basic deep learning model for cross-dataset person re-identification. All experiments are implemented in PyTorch, using an adapted version of the Open-Source Person Re-Identification Library (open-reid). The backbone network is ResNet-50. The widely used softmax cross-entropy is adopted as the loss function. Person images are resized to 256*128, then a random horizontal flipping is used for data augmentation. The batch size of training samples is 32. Stochastic Gradient Descent (SGD) is applied for optimization, with momentum 0.9, and weight decay 5*10^{-4}. The learning rate is set to 0.001 for the backbone network, and 0.01 for newly added layers. When the training data involves real-world images, these rates are decayed by 0.1 after 40 epochs, and the training stops after 60 epochs. Otherwise, these rates are decayed by 0.1 after 10 epochs, and the training stops after 20 epochs.
First, we perform training on each individual dataset. The cross-dataset evaluation results with CUHK03-NP as the target dataset are listed in Table 2. Interestingly, the proposedRandPersondatasetoutperformsallthereal-world datasets, with improvements of 1.8% - 6.8% in Rank-1, and 0.2% - 4.9% in mAP. To the best of our knowledge, this is the ﬁrst time a synthetic dataset has outperformed real-world datasets in person re-identiﬁcation. Compared toexistingsyntheticdatasets,RandPersonalsooutperforms these by an impressively large margin, with 6.0% - 13.0% improvements in terms of Rank-1, and 4.6% - 10.4% improvements in mAP. 

![fig6](https://github.com/VideoObjectSearch/RandPerson/blob/master/img/table2.png)  

Table 3 shows results with Market-1501 as the target dataset. In this popular dataset with a larger scale, RandPerson again outperforms all real-world datasets, with the Rank-1 improved by 2.8%-19.7%, and mAP by 3.0%13.2%. NotethatDukeMTMC-reIDandMSMT17areboth large-scalereal-worlddatasets. Althoughthereisalargedomain gap between RandPerson and real-world images, our dataset’s enhanced superior performance indicates that we have successfully enhanced the diversity in identities and scenes. Besides, compared to synthetic datasets, the advantage of RandPerson is even more obvious, with improvements of 8.8% - 48.3% in Rank-1, and 5.7% - 24.8% in mAP. RandPerson outperforms SyRI by a particularly large margin (23.8% in Rank-1 and 15.3% in mAP), indicating that our dataset makes more efﬁcient usage of scenes, while the number of identities is also important.


![fig7](https://github.com/VideoObjectSearch/RandPerson/blob/master/img/table3.png)  

The cross-dataset evaluation results with DukeMTMCreID as the target dataset are listed in Table 4. As can be observed, compared to synthetic datasets, again, RandPerson improves the performance by a large extent, with a 11.3% - 42.7% increase in Rank-1, and 7.9% - 25.0% increase in mAP. However, as for the real-world datasets, though RandPerson performs much better than CUHK03NP and Market-1501, it is inferior to MSMT17. One possible reason is that the domain gap between RandPerson and DukeMTMC-reID is much larger than that betweenMSMT17andDukeMTMC-reID,andrememberthat MSMT17 is the largest and most diverse real-world person re-identiﬁcation dataset. 

![fig8](https://github.com/VideoObjectSearch/RandPerson/blob/master/img/table4.png)  

Lastly, from the results shown in Table 5, it can be observed that, when tested on MSMT17, RandPerson outperforms all synthetic and real-world datasets, though it is slightly inferior to DukeMTMC-reID. This conﬁrms the smaller domain gap between MSMT17 and DukeMTMCreID. Nevertheless, all results are quite low, indicating that MSMT17 is more diverse and challenging.

![fig9](https://github.com/VideoObjectSearch/RandPerson/blob/master/img/table5.png)  

Though the proposed synthetic dataset RandPerson outperforms all existing real-world and synthetic datasets in most cases for cross-dataset person re-identiﬁcation, it is still not as good as DukeMTMC-reID or MSMT17 when one of them is used for training and the other one for evaluation. This is due to the intrinsic domain gap between synthetic and real-world datasets. One way to reduce the domaingapistofusesyntheticandreal-worlddatasets. Therefore,weperformanotherroundofexperimentsafterdataset fusion. 

The results for dataset fusion are also reported in Tables 2-5. We make three observations. Firstly, the models trained on real-world datasets are all improved by additionally including RandPerson when training for cross-dataset evaluation. Inthiscase,asreportedinTables2-5,theRank1 is improved by 1.8%-24.4%, and the mAP is improved by 1.6%-18.9%. This indicates that RandPerson, though a synthetic dataset, is complementary to real-world datasets for person re-identiﬁcation training. 

Secondly, even in the same domain, it can be observed that additionally including RandPerson for training considerably improves the performance compared to only using the training data of the target dataset. For example, from the results shown in the ﬁrst two rows of Table 2, it can be observed that, compared to training only on CUHK03, the Rank-1 score is signiﬁcantly improved by 26.2% and mAP by 24.0% when additionally including RandPerson. 

Lastly, compared to only using RandPerson for training in cross-dataset evaluation, data fusion also improves the performance by a lot. It can be inferred from the tables that the improvements for Rank-1 range from 0.0%12.2% (except RandPerson + CUHK03 for MSMT17), and 0.2%-12.2% for mAP. This conﬁrms that when real-world datasets are combined with synthetic datasets for training, thedomaingapbetweenthemcanbeconsiderablyreduced.

## Citation
//BibTex of arXiv paper

