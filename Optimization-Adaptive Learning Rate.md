# Optimization(优化)
## gradient descent(梯度下降)
**Question**:类神经网络训练不起来
### Adaptive Learning Rate
#### Adagrad

[![peHdSQP.png](https://s41.ax1x.com/2026/05/05/peHdSQP.png)](https://imgchr.com/i/peHdSQP)
当loss稳定在很小的时候，gradient不一定等于或接近0，可能是在一个坑外反复横跳

[![peHduLT.png](https://s41.ax1x.com/2026/05/05/peHduLT.png)](https://imgchr.com/i/peHduLT)
当learning rate固定时，不总能很好的满足需求，所以考虑自动修改learning rate

[![peHdQwF.png](https://s41.ax1x.com/2026/05/05/peHdQwF.png)](https://imgchr.com/i/peHdQwF)
$$
\theta^{t+1}_i\leftarrow\theta^t_i-\frac{\eta}{\sigma^t_i}g^t_i\\
\sigma^t_i=\sqrt{\frac{1}{t+1}\sum_{i=0}^t{(g^t_i)^2}}
$$
这样，
$g$越大，$\sigma$越大，learning rate就越小；
$g$越小，$\sigma$越小，learning rate就越大。

---
#### RMSProp
*接下来，由于surface error可能会很复杂，我们期望learning rate在同一个参数，同一个方向也能动态变化*
[![peHdUOK.png](https://s41.ax1x.com/2026/05/05/peHdUOK.png)](https://imgchr.com/i/peHdUOK)
用$\alpha$自己决定之前的$g$和最近的$g$的重要性

---
#### Adam:RMSProp+Momentum
python中有函数，可直接使用

---
#### 出现的其他问题

[![peqZGHf.png](https://s41.ax1x.com/2026/05/07/peqZGHf.png)](https://imgchr.com/i/peqZGHf)
虽然$\sigma$很小，但会随着积累逐渐变得很大，使step变大，在某个地方井喷；
但是井喷后又走到$g$很大的地方，$\sigma$又变大，使step变小回来

**解决方法**：
**Learning Rate Scheduling**
让$\eta$跟时间有关
##### 1. Learning Rate Decay(Adam常用)

[![peqZcUU.md.png](https://s41.ax1x.com/2026/05/07/peqZcUU.md.png)](https://imgchr.com/i/peqZcUU)
让$\eta$随时间逐渐变小：随着参数更新，离终点越来越近，所以把learning rate减小，使更新能够刹车

##### 2. Warm Up(RAdam)

[![peqZ5K1.md.png](https://s41.ax1x.com/2026/05/07/peqZ5K1.md.png)](https://imgchr.com/i/peqZ5K1)
让learning rate先变大后变小
最大要多大、增大速率、何时开始减小之类的都是自己设定

使用Warm Up效果好的可能的解释：一开始的$\sigma$是不精准的，所以先用小$\sigma$在初始位置附近做一些探索，收集统计数据后，再逐渐增大learning rate，到一定程度再减小