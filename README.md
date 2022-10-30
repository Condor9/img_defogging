# img_dehaze
使用python复现何凯明Single Image Haze Removal Using Dark Channel Prior论文中的图像去雾算法
## 前言
何恺明的暗通道先验（dark channel prior）去雾算法是CV界去雾领域很有名的算法，关于该算法的论文"Single Image Haze Removal Using Dark Channel Prior"一举获得2009年CVPR最佳论文。作者统计了大量的无雾图像，发现一条规律：每一幅图像的RGB三个颜色通道中，总有一个通道的灰度值很低，几乎趋向于0。基于这个几乎可以视作是定理的先验知识，作者提出暗通道先验的去雾算法。其暗通道的数学表达式为：
$$\[{J^{dark}}(x) = \mathop {\min }\limits_{y \in \Omega (x)} \left[ {\mathop {\min }\limits_{y \in \Omega (x)} {J^c}(y)} \right]\]$$





## 算法原理
![image2](https://github.com/Condor9/img_defogging/tree/main/img/img2.png)

## 参考
- 论文来源
  - http://www.jiansun.org/papers/Dehaze_CVPR2009.pdf

- 算法内容来源
  - https://zhuanlan.zhihu.com/p/457371436
  
# img_transforamtion
使用python实现对图像的各种变换

# img_wiener_filter
使用python实现对图像的维纳滤波

# img_DPCM
使用python实现在不同量化因子（2bit、4bit、8bit）下对图像进行DPCM编码与解码
