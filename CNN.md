# Convolutional Neural Network(卷积神经网络)
一种network框架，用于影像

[![peOaMxs.png](https://s41.ax1x.com/2026/05/11/peOaMxs.png)](https://imgchr.com/i/peOaMxs)
图片在交给电脑处理前，需先处理为相同大小

每张图片被转换为三维的tensor(张量)，三维分别代表图片的宽、长、channel，其中黑白图片的channel为1层，彩色图片的channel为三层，表示RGB

由于model只能处理一维向量，所以将张量拉直为一个100×100×3的向量后当作输入

[![peOavyn.png](https://s41.ax1x.com/2026/05/11/peOavyn.png)](https://imgchr.com/i/peOavyn)
将全部像素都放入输入后，再乘上neuron的数量，参数非常大，容易过拟合

## 影像辨识问题
两个不同方向的解释
### Neuron Version
#### 1. Receptive Field
**observation**：
[![peOw1EV.md.png](https://s41.ax1x.com/2026/05/11/peOw1EV.md.png)](https://imgchr.com/i/peOw1EV)
图片辨识只需要抓住关键点，所以neuron只需要把特别关键的部分当作输入即可
**simiplification(简化)**：
[![peOd5p4.md.png](https://s41.ax1x.com/2026/05/11/peOd5p4.md.png)](https://imgchr.com/i/peOd5p4)
对一张图片设置多个Receptive field，，将receptive field拉直为3×3×3的向量后，每个neuron只关心自己的receptive field
receptive field是自己设定的：
- 不同的neural可以重叠
- 同一个field可以用多个neural
- receptive field大小形状可以不同
- 一个receptive field可以只关注某一个channel

**typical setting**：
[![peOwBE6.png](https://s41.ax1x.com/2026/05/11/peOwBE6.png)](https://imgchr.com/i/peOwBE6)
将receptive field设为3×3×3，每个channel有一组neurons去守备(64或128个)
将receptive field移动一个stride后设置新的receptive field，stride需自己调，一般为1或2，所以不同receptive field之间一般有重叠(防止pattern出现在两field边界而被忽略)
超出边界的范围padding(补0)

#### 2. Parameter Sharing
**observation**：
[![peOD3UP.md.png](https://s41.ax1x.com/2026/05/11/peOD3UP.md.png)](https://imgchr.com/i/peOD3UP)
守备不同区域但是有同种pattern的neuron所做的工作是一样的
**simplification**：
[![peOD1Et.md.png](https://s41.ax1x.com/2026/05/11/peOD1Et.md.png)](https://imgchr.com/i/peOD1Et)
对于上述观察的neuron，可以共享参数，由于输入不同，输出也不同

**typical setting**：
[![peODY8S.png](https://s41.ax1x.com/2026/05/11/peODY8S.png)](https://imgchr.com/i/peODY8S)
因此，守备不同field的neurons的参数都是一样的
*filter(卷积核)*：参数一样的neurons
![alt text](image.png)

### Filter Version
#### Convolutional Layer
![alt text](image-1.png)
每个convolution layer中有很多filter，每个filter是3×3×channel的tensor
filter里的数值通过GD寻找

![alt text](image-2.png)
先将filter1中的值放在图片的左上角，跟图片中的值相乘再相加得到一个值，将位置移动一个stride再算...
每个filter都这样做一遍，每个filter会得到一个新的channel
*Feature Map*：每个filter对图片昨晚计算之后得到的数字群

![alt text](image-4.png)
将feature map看成有(filter数量)个channel的新图片，会得到第二层的convolution，也有对应的filter

每层filter大小都设为3×3，通过多层convolution叠加，可以看到更大范围

#### Pooling(池化)
**observation**：
将图片缩小(subsampling)一般不影响它原来是什么东西

**Pooling**：
![alt text](image-5.png)
将每个filter产生的数字分为几组，每组里选一个代表(maxpooling、meanpooling)

![alt text](image-6.png)
在实践中往往convolution和pooling交替使用，做几次convolution做一次pooling(现在GPU算力充足，pooling主要目的是降维防止过拟合，本身就轻量化的model不需要pooling)

### The Whole CNN
![alt text](image-7.png)
将最后得到的矩阵拉直变为一个向量，经过fully connect layers(全连接层)，将分散的局部特征整合起来，得到影像辨识结果(可能会用到softmax等非线性变换)

## 其他应用、缺陷
应用：
围棋：不pooling
语音、文字辨识：使用方式略有区别

缺陷：
不能很好的处理图片放缩和旋转问题
解决方法：截取小的图片，并进行不同旋转分别进行训练