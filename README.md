# SBAS-SLAM
The relevant dataset and source code for "SBAS: Salient bundle adjustment for visual SLAM"
# This code will be uploaded as soon as possible after the corresponding paper is accepted.
## Framework
Our proposed framework comprises two components: a geometry-based SLAM pipeline and a learning-based saliency prediction module. The saliency prediction module generates a saliency map (a 2D distribution predicting the conspicuity of specific locations and their likelihood in attracting fixations) corresponding to the input image. Then, the saliency map is used as input to help the SLAM system choose the salient feature point to improve the accuracy and robustness. Specifically, we use the saliency map as a weight map in bundle adjustment to gives the features in salient regions a larger weight, which we called salient bundle adjustment. Finally, our proposed SBA is used to estimate camera motion in tracking thread, build the local map in local mapping thread, and merge the map in Loop & Map merging thread. Besides, in our proposed framework, we adopt ORB-SLAM3 as the backbone.
![figure1](/figure/framework.png)

test video demo
https://www.bilibili.com/video/bv1DN411f7Ly
## Saliency map computation
Visual saliency or visual attention mechanism refers to mimic the human vision system to select the most salient and interesting features from natural scenes for further processing under different tasks. This is very curial for SLAM, where the system can pay more attention to areas with rich corners and lines, while ignoring the dynamic dynamics, in complex environments. There are several datasets used for saliency prediction. By analyzing the data of these datasets, we can find that these datasets have the following characteristics: 

(a) saliency is highly relevant with objects; 

(b) saliency is highly relevant with dynamic objects or the dynamic part of the object; 

(c) long-standing objects will receive continuous attention for a period of time; 

(d) newly-appearing objects are more noticeable than old ones; 

(e) When the scene is cluttered or there is no obvious dominant object in the scene, the attention will be biased to the center. 

However, this saliency does not completely describe everything the SLAM should attend to, and even some saliency will cause the failure of the SLAM system, such as the saliency associated with dynamic objects. Therefore, we should make our saliency prediction model focusing on the right stuff. 
For SLAM applications, we usually regard corners, edges and planes as image features. Therefore, we first extract the geometric information of each image such as points, lines, and planes, as shown in Fig.1(column 2). Then, we used the SDC-Net as the backbone, and trained the model on the Cityscapes dataset. Since the total of 30 classes in the Cityscapes dataset, we only choose 13 categories that are most relevant to the regions with robust, stable and salient features - traffic light, traffic sign, road, building, sidewalk, parking, rail track, fence, bridge, pole, pole-group, vegetation, terrain. For each image, SDC-Net provides an instance segmentation of every detected object, as shown in Fig.1(column 3). Finally, we use these segmentations to filter the geometric information to obtain the saliency map. Besides, since the saliency map obtained only from semantic will give equal-weighted attention to all the objects present. We give each object a unique weight. Fig.2(column 4) and Fig.2(column 5) show the comparison of our proposed saliency map with the existing gaze-only saliency map. 

The Salient KITTI dataset pipeline
![figure2](/figure/figure1.jpg)
Figure 2: the pipeline of making salient dataset, and comparison of our proposed semantic gaze with human gaze. 
1) we extract the geometric information such as feature points (such as ORB, FAST, SIFT, etc.) and lines (such as canny edges). ( see the second column)
2) we use semantic segmentation network to generate segmentation mask around the interest object, such as traffic light, traffic sign, pole, road, etc. (see the third column)
3) we use the generated segmentation masks to filter the geometric information extracted in step 1. (see the fourth column)

video demo
![BV15f4y1y7Su](/figure/figure3.png)(see the link: https://www.bilibili.com/video/BV15f4y1y7Su)


Salient KITTI Dataset：
Google drive： https://drive.google.com/file/d/1MEuUVR24BPa2rYW8o5aqyEqxw3Fng5Nf/view?usp=sharing

## Saliency Prediction
![figure3](/figure/saliency.png)
Based on the Salient-KITTI dataset, we can obtain a saliency prediction model by using DI-Net and use it to predict the initial saliency map. Besides, we also find that the relative distance between objects and ego-vehicle will impact our attention. For example, we will react inherently when vehicles or pedestrians are getting closer and closer, and will pay more attention to objects that are closer to ours, because the closer they are, the higher the possibility of collision. Therefore, we considered two methods to incorporate depth information into our saliency prediction framework. The first uses a monocular depth estimation network to the obtained depth map. The second use depth completion network to obtain depth map, if Lidar or other depth sensors available. Although the second method can obtain a more accurate depth map, we still use the first method because it does not require additional sensors. Then, we use the obtained depth map to correct the initial saliency map.

## Evaluation
Tested on KITTI dataset
![figure4](/figure/kitti1.png)
![figure5](/figure/kitti2.png)
Tested on EuRoc dataset
![figure6](/figure/euroc1.png)
![figure7](/figure/euroc2.png)
