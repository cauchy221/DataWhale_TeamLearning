## 报名

1. 注册华为账号
2. 点击比赛网址，创建队伍并按照要求提供团队信息
3. 报名：广告-信息流跨域CTR预估
4. 下载比赛数据，共包含四个csv文件

<img src="C:\Users\86139\AppData\Roaming\Typora\typora-user-images\image-20220913131742201.png" alt="image-20220913131742201" style="zoom:80%;" />

## 赛题理解

1. **什么是CTR：**

   Click-Through-Rate 点击通过率，指网络广告的点击到达率

   $$CTR=\frac{广告的实际点击次数}{广告的展现量}$$

2. **为什么要做CTR预测：**

   在广告实际投放情况中，需要综合考虑CTR以及平均点击价格ACP，决定广告的投放量

3. **什么是比赛数据中的目标域和源域数据：**

   目标域：收集广告任务产生的历史数据，但是用户行为数据稀疏，行为类型相对单一（用户个人信息、用户所点击的广告信息等）

   <img src="C:\Users\86139\AppData\Roaming\Typora\typora-user-images\image-20220913134453800.png" alt="image-20220913134453800" style="zoom:80%;" />

   源域：同一媒体的跨域数据，可以通过同一广告用户在其他域的行为数据，深度挖掘用户兴趣，丰富用户行为特征

   <img src="C:\Users\86139\AppData\Roaming\Typora\typora-user-images\image-20220913134828420.png" alt="image-20220913134828420" style="zoom:80%;" />

4. **赛题任务：**

   根据历史的用户行为特征（两个域的数据），预测是否会点击广告（二分类问题）

   不可以使用穿越信息（T时刻样本使用T时刻之前的信息，不能使用T时刻未来的信息）

5. **评价指标：**

   $$xAUC = \alpha*GAUC+\beta*AUC$$，越高越优（初赛$$\alpha=0.7,\beta=0.3$$）

   下面结合[这篇文章]([图解AUC和GAUC - 知乎 (zhihu.com)](https://zhuanlan.zhihu.com/p/84350940))，介绍一下ROC、AUC、GAUC

   6. **ROC前身：通用的对分类模型的评价**

      在对每一个样本计算出一个预测概率后，若选择不同的阈值，我们会得到不同的分类结果

      <img src="C:\Users\86139\AppData\Roaming\Typora\typora-user-images\image-20220913141002836.png" alt="image-20220913141002836" style="zoom:80%;" />

   7. **ROC曲线：**

      在上面的情况中，我们可以遍历所有的阈值，查看每一个阈值下的分类情况，并绘制ROC曲线

      纵坐标：$$TPR=\frac{TP}{TP+FN}$$

      横坐标：$$FPR=\frac{FP}{FP+TN}$$

      <img src="C:\Users\86139\AppData\Roaming\Typora\typora-user-images\image-20220913141619401.png" alt="image-20220913141619401" style="zoom:80%;" />

   8. **AUC：**

      在绘制出ROC曲线后，我们就可以计算出AUC的值

      $$AUC=\frac{ROC曲线下面积}{在x*y区域面积}$$

      将上面公式的积分过程拆开，我们可以用另一个角度计算AUC

      $$AUC=\frac{\sum每个预测为正的样本能比多少负样本大}{正样本数*负样本数}$$

      <img src="C:\Users\86139\AppData\Roaming\Typora\typora-user-images\image-20220913143408605.png" alt="image-20220913143408605" style="zoom:80%;" />

   9. **GAUC：**

      在广告推荐领域虽然仍是二分类模型，但是是更细粒度的二分类（对每个人进行二分类），因此传统的粗粒度的AUC并不适用

      GAUC其实是计算每一个用户的AUC，然后加权平均，这样可以减少不同用户之间不好比较的影响（因为不同用户之间对于广告偏好差距较大）

      实际处理时权重一般可以设为每个用户view或click的次数，而且会过滤掉单个用户全是正样本或负样本的情况