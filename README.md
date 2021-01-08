# SBA-SLAM
The relevant dataset and source code for "SBAS: Salient bundle adjustment for visual SLAM"
# This code will be uploaded as soon as possible after the corresponding paper is accepted.

# The Salient KITTI dataset pipeline
![figure1](/figure/figure1.jpg)
Figure 1: the pipeline of making salient dataset, and comparison of our proposed semantic gaze with human gaze. 
1) we extract the geometric information such as feature points (such as ORB, FAST, SIFT, etc.) and lines (such as canny edges). ( see the second column)
2) we use semantic segmentation network to generate segmentation mask around the interest object, such as traffic light, traffic sign, pole, road, etc. (see the third column)
3) we use the generated segmentation masks to filter the geometric information extracted in step 1. (see the fourth column)


![BV15f4y1y7Su](/figure/figure3.png)(https://www.bilibili.com/video/BV15f4y1y7Su)


Salient KITTI Dataset：

Google drive： https://drive.google.com/file/d/1MEuUVR24BPa2rYW8o5aqyEqxw3Fng5Nf/view?usp=sharing
