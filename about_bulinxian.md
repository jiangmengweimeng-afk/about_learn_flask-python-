### 什么是 numpy 
- python底层做数学计算的基础库，提供了多为数组相对来说比python自带的列表快得多 功能也多。同时也是pandas,matplotlib等库的底层依赖
- 提供了大量数学函数: mean（均值）、std（标准差）、sqrt（开方）、sin/cos、线性代数、随机数等

### 什么是pandas
- 它是专门处理表格型数据的库，相当于python的excel，用于处理股票数据、销售记录、实现数据等表格
- 核心数据结构: DataFrame（二维表格）、Series（一列数据），能做CSV/Excel、筛选列行、缺失值处理、分组(group by)、合并表、时间序列处理，是数据分析的基本工具

### 什么是matplotlib.pyplot 
- 它是绘图库的一个子模块，专门负责画图，可以设置坐标、坐标轴标签、图例等
- plt.plot() 拆柱图
- plt.bar() 柱状图
- plt.scatter() 散点图
- plt.hist() 直方图
  ![alt text](imgs/about_bulinxian.image.png)

### 布林线完整示意图
- 1.0 布林带宽图：
  - 黑色线：实际价格走势
  - 蓝色线：中轨（20日均线）
  - 红色虚线：上轨（压力线）
  - 绿色虚线：下轨（支撑线）
  - 灰色区域：布林通道范围
  - 绿色：买入信号（价格跌倒下轨）
  - 红色：卖出信号（价格涨到上轨）
  - 红/绿色背景：超买/超卖区域
- 1.1 布林宽带图
  - 紫色区域：带宽变化趋势
  - 橙色区域：低波动（可能变盘）
  - 蓝色区域：高波动（趋势延续）
  ![alt text](imgs/about_bulinxian.image-1.png)
- 1.2 关键视觉特征
  - 价格大部分时间在上下轨之间波动
  - 触及上轨时通常会回落
  - 触及下轨时通常会反弹
  - 通道宽窄代表市场波动大小
- 1.3 三条线的含义
  - 中轨：N周期简单移动平均线，为了判断趋势方向
  - 上轨：中轨 + K倍标准差，压力位/超买
  - 下轨：中轨 — K倍标准查差，支撑位/超卖
  - 经典参数：N=20， K=2（即20日均线，上下2个标准差）
  ![alt text](imgs/about_bulinxian.image-2.png)

- 2.1 完整公式
  ![alt text](imgs/about_bulinxian.image-3.png)
  ![alt text](imgs/about_bulinxian.image-4.png)
- 2.2 标准差
  - 标准差衡量数据偏离平均值的程度，波动越大，标准差和越大，布林带越宽