# img_dehaze
## 前言
何恺明的暗通道先验（dark channel prior）去雾算法是CV界去雾领域很有名的算法，关于该算法的论文"Single Image Haze Removal Using Dark Channel Prior"一举获得2009年CVPR最佳论文。作者统计了大量的无雾图像，发现一条规律：每一幅图像的RGB三个颜色通道中，总有一个通道的灰度值很低，几乎趋向于0。基于这个几乎可以视作是定理的先验知识，作者提出暗通道先验的去雾算法。
其暗通道的数学表达式为：
![image1](https://github.com/Condor9/img_defogging/tree/main/img/img1.png)

## 算法原理
![image2](https://github.com/Condor9/img_defogging/tree/main/img/img2.png)

## 参考
- 论文来源
  - http://www.jiansun.org/papers/Dehaze_CVPR2009.pdf
