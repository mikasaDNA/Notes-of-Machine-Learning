# Optimization(优化)
## gradient descent
**Question**:类神经网络训练不起来
在classification中计算loss的方法也会影响训练
### 计算Loss的方法

[![peqeDRH.png](https://s41.ax1x.com/2026/05/07/peqeDRH.png)](https://imgchr.com/i/peqeDRH)
1. **Mean Square Error(MSE)**:
2. **Cross-entropy**:更适合用来分类
pytorch中cross-entropy和softmax是绑定在一起的，不需要自己创建softmax

[![peqe6sI.png](https://s41.ax1x.com/2026/05/07/peqe6sI.png)](https://imgchr.com/i/peqe6sI)
将$y_3$的值设置的很小，使其对$y_1,y_2$的影响很小
预测为class1，$y_1$大$y_2$小的时候更符合结果，即图片右下角为small loss，左上角为big loss，training时参数最后要走到右下角(假设参数从左上角开始走)

MSE在误差较大的时候比较平坦，$g$比较小，用gradient deacent效果差(可使用wram up优化)
cross-entropy在左上角也有斜率，可以较快的更新到右下角