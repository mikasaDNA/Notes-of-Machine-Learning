# Spatial Transformer
## Spatial Transformer Layer
[![peXUQdf.png](https://s41.ax1x.com/2026/05/13/peXUQdf.png)](https://imgchr.com/i/peXUQdf)
CNN不能很好的处理scaling和rotation

所以需要在CNN之前在嵌入一个spatial transformer layer来对image进行transform
STYE也可以transform feature map

---
## How
[![peXUHld.png](https://s41.ax1x.com/2026/05/13/peXUHld.png)](https://imgchr.com/i/peXUHld)
(图中表示将图片进行平移)

通过设计不同的$w^l_{nm}$(线性变换)，来实现对图片的各种旋转缩放：
[![peXaQXR.md.png](https://s41.ax1x.com/2026/05/13/peXaQXR.md.png)](https://imgchr.com/i/peXaQXR)
[![peXaMc9.md.png](https://s41.ax1x.com/2026/05/13/peXaMc9.md.png)](https://imgchr.com/i/peXaMc9)

**仿射变换**：
[![peXarHP.png](https://s41.ax1x.com/2026/05/13/peXarHP.png)](https://imgchr.com/i/peXarHP)
这个变换只由6个参数表示，所以NN结果就是这六个数
**对l层输出feature map中的每个像素$(x,y)$，其值需要从l-1层二点输入feature map中采样(如双线性插值)得到，所以必须通过变换参数将输出坐标$(x,y)$映射到输入坐标$(x',y')$,即回推输入位置**
说人话就是回推能保证l层每个像素点都能找到自己在l-1层的位置
将layer l通过transformation变成layer l-1

[![peXaxD1.png](https://s41.ax1x.com/2026/05/13/peXaxD1.png)](https://imgchr.com/i/peXaxD1)
变换后的坐标为整数时，能够很好的找到输入像素
但变换后的坐标往往不为整数(如上图)，需要考虑使用双线性插值来估算非整数坐标处的像素值
>为什么不用四舍五入(最近邻插值)：
当参数发生微小变换时，变换后的坐标值也发生微小变换，但是四舍五入之后变换消失了，所以取微分的结果几乎永远是0，无法使用GD

---
### Interpolation(插值)
[![peXdZDI.png](https://s41.ax1x.com/2026/05/13/peXdZDI.png)](https://imgchr.com/i/peXdZDI)
使用双线性插值
找到与求出的坐标值相邻的四个整数坐标，基于距离计算权重，加权平均得到采样值
>这样，在参数发生微小变化时，该变化会影响到最终的采样值，就可以使用GD了

[![peji8US.png](https://s41.ax1x.com/2026/05/14/peji8US.png)](https://imgchr.com/i/peji8US)
ST可以加在任何一个convolution后

[![pejiG4g.md.png](https://s41.ax1x.com/2026/05/14/pejiG4g.md.png)](https://imgchr.com/i/pejiG4g)
鸟类辨识示例：使用两个ST，重点关注鸟类的翅膀和眼睛