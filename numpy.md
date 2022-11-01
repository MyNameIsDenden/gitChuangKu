## numpy.linspace使用详解

numpy.linspace(start, stop, num=50, endpoint=True, retstep=False, dtype=None)

numpy.linspace(start, stop, num=50, endpoint=True, retstep=False, dtype=None)

在指定的间隔内返回均匀间隔的数字。

返回num均匀分布的样本，在[start, stop]。

这个区间的端点可以任意的被排除在外。

## np.random

1. **np.random.rand()** --> 生成指定维度的的**[0,1)范围**之间的随机数，输入参数为维度
2. **np.random.randn()** --> 生成指定维度的服从标准正态分布的随机数，输入参数为维度
3. **np.random.randint(low, high = None, size = None,dtype = 'l')**--> 返回随机数或者随机数组成的array
4. **np.random.random_integers(low,high = None,size = None)**-->返回范围为[low,high] **闭区间** 随机整数
5. **np.random.random(size = (2,2))**-->生成随机浮点数阵列
6. **np.random.choice(a, size = None, replace = True, p = None)** --> 从给定数组a中随机选择,p可以指定a中每个元素被选择的概率
7. **np.random.seed()** -->使随即数据可预测，对于同一个seed，生成的随机数相同
   