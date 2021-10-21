# 第6章	LR分析

## 6.1自上而下分析及其LR分析概述

自上而下分析算法：能力强、构造复杂

**移进--归约**

<img src="https://gitee.com/cpicture/picture-1/raw/master/202110181604032.png" style="zoom:67%;" />

**活前缀是规范句型的前缀，但不包括句柄**

<img src="https://gitee.com/cpicture/picture-1/raw/master/202110181607716.png" style="zoom: 67%;" />

> 规范推导：最右推导
>
> 规范句型
>
> 规范归约

### LR分析

> L：自左向右
>
> R：最右推导

**LR分析器模型**

<img src="https://gitee.com/cpicture/picture-1/raw/master/202110181611963.png" style="zoom:67%;" />

> ACTION：归约or移进
>
> GOTO：归约后到达的状态

**注意：归约后使用伪栈顶**

<img src="https://gitee.com/cpicture/picture-1/raw/master/202110181616082.png" style="zoom:67%;" />

<img src="https://gitee.com/cpicture/picture-1/raw/master/202110181616833.png" style="zoom:67%;" />
