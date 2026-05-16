# Optimization(优化)
## gradient descent(SGD)(梯度下降)
**Question**:类神经网络训练不起来
gradient接近0的点称为critical point，可能有以下两种情况导致失败：
1. local minima(局部最小值)
2. saddle point(鞍点)

saddle point不是我们想要的点，需要想办法逃离

---
### **区分办法1**：泰勒展开近似，利用海森矩阵
找到$L(\theta)$around$\theta=\theta'$can be approximated below
$$
L(\theta)\approx L(\theta')+(\theta-\theta')^Tg+\frac{1}{2}(\theta-\theta')^TH(\theta-\theta')
$$
**Giadient** g is a vector:
$$
g=\nabla L(\theta')\\
g_i=\frac{\partial L(\theta')}{\partial \theta_i}
$$
**Hassian** H is a martix:
$$
H_{ij}=\frac{\partial^2L(\theta')}{\partial \theta_i \partial \theta_j}
$$
$L(\theta)\approx L(\theta')+(\theta-\theta')^Tg+\frac{1}{2}(\theta-\theta')^TH(\theta-\theta')$
$\therefore$critical point:第二项=0，通过第三项来区分是local min,local max还是saddle point

---
$L(\theta)\approx L(\theta')+\frac{1}{2}(\theta-\theta')^TH(\theta-\theta')$
将$(\theta-\theta')^TH(\theta-\theta')$简写作$v^THv$

- For all $v$：$v^THv>0\Rightarrow L(\theta)>L(\theta')\Rightarrow$ local minima
- For all $v$：$v^THv<0\Rightarrow L(\theta)<L(\theta')\Rightarrow$ local maxima
- sometimes $v^THv>0$,sometimes $v^THv<0\Rightarrow$saddle point

即$H$正定时为local minima，$H$负定时为local maxima，$H$不定时为saddle point

在saddle point处，根据需求选eigenvalue(特征值)找出对应的eigenvector(特征向量)进行优化：
1. 特征值$>0$：向着该方向可找最大值
2. 特征值$<0$：向着该方向可找最小值

**统计表明，鞍点数量远远多于局部最小值**

---
### **区分方法2**：Batch(BGD批量梯度下降)
$\theta^*=\arg\min\limits_{\theta}L$
[![peHN5k9.png](https://s41.ax1x.com/2026/05/05/peHN5k9.png)](https://imgchr.com/i/peHN5k9)
在每一个epoch开始前进行一次shuffle(数据打乱)，使每次的batch都分得不一样
[![peHNof1.png](https://s41.ax1x.com/2026/05/05/peHNof1.png)](https://imgchr.com/i/peHNof1)
当不使用batch(左边)时，需把所有数据都看完才会update一次：蓄力久但powerful
当使用batch(右边)时，看完一个batch就uodate一次：蓄力短但noisy
[![peHNztA.png](https://s41.ax1x.com/2026/05/05/peHNztA.png)](https://imgchr.com/i/peHNztA)
由于GPU的存在，可进行平行运算，比较大的batch更有效率
[![peHUP6f.png](https://s41.ax1x.com/2026/05/05/peHUP6f.png)](https://imgchr.com/i/peHUP6f)
然而事实证明，当batch较大时，效果会变差（此处不是overfitting）
当batch较小时，由于noisy存在，可能使update跨过在某一个batch上$g=0$的点
进一步的研究发现即使在train时小的和大的效果差不多，但是大的batch在testig时效果不如小的batch(overfitting)
[![peHUrHe.png](https://s41.ax1x.com/2026/05/05/peHUrHe.png)](https://imgchr.com/i/peHUrHe)
由于training data使随机取样，所以当有微小偏差的时候，在峡谷中的minima Loss更大，而在盆地中的minima Loss较小
小的batch倾向于走进盆地minima，大的batch倾向于让人走进峡谷minima

---
### **区分方法3**：momentum(动量)(SGDM)
在GD中加入物理中的惯性
[![peHa2a4.png](https://s41.ax1x.com/2026/05/05/peHa2a4.png)](https://imgchr.com/i/peHa2a4)
在做GD时，向着$g$反方向和上一步移动方向的合方向移动
$\therefore m^i$为$g^0,g^1,...,g^{i-1}$的和
$$
m^0=0\\
m^1=-\eta g^0\\
m^2=-\lambda\eta g^0-\eta g^1\\
...
$$
$\lambda$是人为设定的参数，一般=0.9