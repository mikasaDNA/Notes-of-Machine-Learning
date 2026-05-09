# New Optimazation For Deep Learning
some Notations
- $\theta_t$:在第t步的模型参数
- $\nabla L(\theta_t)$ or $g_t$:$\theta_t$的梯度，用来计算$\theta_{t+1}$
- $m_{t+1}$:从第0步到第t步的动量，用来计算$\theta_{t+1}$

###
[![peqm476.md.png](https://s41.ax1x.com/2026/05/07/peqm476.md.png)](https://imgchr.com/i/peqm476)
[![peqmh0x.md.png](https://s41.ax1x.com/2026/05/07/peqmh0x.md.png)](https://imgchr.com/i/peqmh0x)
- On-line learning:一次处理一次一个x和y
- Off-line learning:一次处理所有的x和y

###
20年主流模型都在使用Adam和SGDM进行优化
[![peqHTIO.png](https://s41.ax1x.com/2026/05/08/peqHTIO.png)](https://imgchr.com/i/peqHTIO)
Adam能更快的训练，泛化间隙大，但不稳定
SGDM更稳定，泛化间隙小，收敛效果好

---
### SWATS
**考虑将Adam和SGDM结合**
一开始先用Adam，最后收敛的时候用SGDM

## 考虑优化Adam
### AMSGrad
[![peqHzeP.png](https://s41.ax1x.com/2026/05/08/peqHzeP.png)](https://imgchr.com/i/peqHzeP)
当前面的$g$都很小的时候，在后面即使出现一个很大的$g$，对之后$g$的影响也会很小

[![peqO51e.png](https://s41.ax1x.com/2026/05/08/peqO51e.png)](https://imgchr.com/i/peqO51e)
考虑记录并使用最大的$v_t$，会出现和Adagrad同样的问题，后期更新过慢

### AdaBound
[![peLASiQ.png](https://s41.ax1x.com/2026/05/09/peLASiQ.png)](https://imgchr.com/i/peLASiQ)
注：$\beta_2$即Adam中当分母的$\beta_2$
把learning rate做$Clip$，但$Clip$函数是经验公式,lower bound和upper bound已经固定，不算真正的Adaptive learning rate
$\therefore$不好用

---
## 考虑优化SGDM
### 
考虑将learning rate一会调大，一会调小
[![peLAKzR.png](https://s41.ax1x.com/2026/05/09/peLAKzR.png)](https://imgchr.com/i/peLAKzR)
[![peLAuW9.png](https://s41.ax1x.com/2026/05/09/peLAuW9.png)](https://imgchr.com/i/peLAuW9)