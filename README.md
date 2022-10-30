# img_dehaze
使用python复现何凯明Single Image Haze Removal Using Dark Channel Prior论文中的图像去雾算法
## 前言
何恺明的暗通道先验（dark channel prior）去雾算法是CV界去雾领域很有名的算法，关于该算法的论文"Single Image Haze Removal Using Dark Channel Prior"一举获得2009年CVPR最佳论文。作者统计了大量的无雾图像，发现一条规律：每一幅图像的RGB三个颜色通道中，总有一个通道的灰度值很低，几乎趋向于0。基于这个几乎可以视作是定理的先验知识，作者提出暗通道先验的去雾算法。其暗通道的数学表达式为：
$${J^{dark}}(x) = \mathop {\min }\limits_{y \in \Omega (x)} \left[ {\mathop {\min }\limits_{y \in \Omega (x)} {J^c}(y)} \right]$$

其中：
$J^c$
图像每个通道，
$\Omega (x)$
表示以像素x为中心的窗口。

原理：先取图像中每一个像素的三通道中的灰度值的最小值，得到一幅灰度图像，再在这幅灰度图像中，以每一个像素为中心取一定大小的矩形窗口，取矩形窗口中灰度值最小值代替中心像素灰度值，从而得到输入图像的暗通道图像。

滤波窗口大小：
$WindowSize = 2 \times Radius + 1$

暗通道先验理论：
${J^{dark}} \to 0$

## 算法原理
去雾模型：
$I(x) = J(x)t(x) + a[1 - t(x)]$

其中：I(x)为原图，即待去雾图像。J(x)是要恢复的无雾图像。A是大气光成分，t(x)透光率。

对于成像模型，将其归一化，即两边同时除以每个通道的大气光值：
$$\frac{{{I^c}(x)}}{{{A^c}}} = t(x)\frac{{{J^c}(x)}}{{{A^c}}} + 1 - t(x)$$

假设大气光A为已知量，t(x) 透光率为常数，将其定义为t(x)对上式两边两次最小化运算：
$$\mathop {\min }\limits_{y \in \Omega (x)} \left[ {\mathop {\min }\limits_c \frac{{{I^c}(x)}}{{{A^c}}}} \right] = \tilde t(x)\mathop {\min }\limits_{y \in \Omega (x)} \left[ {\mathop {\min }\limits_c \frac{{{I^c}(x)}}{{{A^c}}}} \right] + 1 - \tilde t(x)$$

根据 暗通道先验理论：
${J^{dark}} \to 0$
有：
$${J^{dark}}(x) = \mathop {\min }\limits_{y \in \Omega (x)} \left[ {\mathop {\min }\limits_c {J^c}(y)} \right] = 0$$

推出：
$$\mathop {\min }\limits_{y \in \Omega (x)} \left[ {\mathop {\min }\limits_c \frac{{{I^c}(x)}}{{{A^c}}}} \right] = 0$$

代回原式，得到：
$$\tilde t(x) = 1 - \mathop {\min }\limits_{y \in \Omega (x)} \left[ {\mathop {\min }\limits_c \frac{{{I^c}(x)}}{{{A^c}}}} \right]$$

为了防止去雾太过彻底，恢复出的景物不自然，应引入参数
$\omega = 0.95$
，重新定义传输函数为：
$$\[\tilde t(x) = 1 - \mathop {\omega \min }\limits_{y \in \Omega (x)} \left[ {\mathop {\min }\limits_c \frac{{{I^c}(x)}}{{{A^c}}}} \right]\]$$

上述推论假设大气A为已知量。实际中，可借助于暗通道图从有雾图中获取该值。具体步骤大致为：从暗通道图中按照亮度大小提取最亮的额前0.1%像素。然后在原始尤物图像I(x)中寻找对应位置最高两点的值，作为A值。至此，可以进行无雾图像恢复了。
考虑到当透射图t的值很小时，会导致J的值偏大，使整张图向白场过度，故设置一个阈值
${t_0}$
，当
$t < {t_0}$
时，令
$t = {t_0}$
（示例程序
${t_0}=0.1$
）最终公式：
$$J(x) = \frac{{I(x) - A}}{{\max [t(x),{t_0}]}} + A$$

关于导向滤波的方法：对于求解得到的传输函数，应用滤波器对其进行细化，文章中介绍的方法是软抠图的方法，此方法过程复杂，速度缓慢，因此采用导向滤波对传输函数进行滤波。导向滤波的原理此处不再赘述，详情：https://blog.csdn.net/wsp_1138886114/article/details/84228939

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
