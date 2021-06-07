# 蚁群算法求解TSP问题 

## 求解具体问题前的一些思考

![求解问题](https://gitee.com/cpicture/picture-1/raw/master/20210606230603.png)

**语言选择**：**C**

**地图的存储方式**：**关系矩阵**

就目前所学，C语言中表示一幅如图所示的图像有**关系矩阵**、**链表**两种方式。

与关系矩阵相比，链表方式实现难以直接借鉴，选择关系矩阵方式。

所给图像不是**完全图**，存在一些不能一步转移的路径，如1->5。通过将其路径长度设为一个较大的数解决。

**模型选择**：蚁周模型

> 蚂蚁系统有三种基本模型分别是蚁周模型、蚁密模型、蚁量模型。三种模型的实现大致相同,主要区别是在信息素的更新方式上。
>

**我认为的蚁群算法求解TSP**：课后

- 将m只蚂蚁放在同一起点处，每一只蚂蚁途径每一个城市一次仅一次，最终回到起点
- 在一次迭代过程中，信息素蚂蚁每走一步，便会更新一次（q/D）
- 蚂蚁的下一城市选择是依据实时信息素浓度的
- 在迭代过程中，蚂蚁数量不会增加，每一次迭代过程中的蚂蚁还是原来的那些蚂蚁
- 最终的最优解是根据信息素浓度给出的

**具体实现**：

- M只蚂蚁随机放置到N个城市中
- 进行完一次迭代过程后，进行一次信息素更新
- 在第N次迭代过程中，信息素的依据是上一轮迭代的结果，并且，蚂蚁的一生是确定的，迭代刚开始就会规划每一只蚂蚁的路线
- 每一轮迭代的最后会将蚂蚁数据free(),
- 最终的解：将每一迭代过程中的最优解，与当前最优解进行比较，最终得出的。感觉有点**遗传算法**的味道

## 算法的主要步骤

![](https://gitee.com/cpicture/picture-1/raw/master/20210607165654.png)

### 初始化参数

- 关系(地图)矩阵graph，城市数目N，信息素浓度矩阵phe
- 信息素因子α，启发函数因子β，信息素浓度保持度rout，信息素常量Q
- 蚂蚁数量M，最大迭代次数RcMax,信息素初始浓度IN
- 启发函数矩阵(1/d)，信息素增量矩阵∆τ_ij
- 二维矩阵map表示每只蚂蚁的路线，二维矩阵via表示每只蚂蚁访问过的城市(禁忌表)
- 当前最优解

### 迭代过程

- 将M只蚂蚁随机分配到N个城市

  ```c
  for(int i = 0;i<M;i++)//y表示当前迭代次数，M蚂蚁数，N城市数
  {
      map[i][0] = (y+i)%N;//蚂蚁路线
      vis[i][ map[i][0]] = 1;//访问城市(禁忌表)
  }
  ```

- 根据信息素浓度，为每一只蚂蚁安排“一生”的路径

  ```c
  for城市数
      for蚂蚁数
      	根据禁忌表算出转移概率
      	轮盘赌方式确定下一步
      ·	更新蚂蚁下一步路线，更新禁忌表
      end
  end
  ```

- 更新最优解

  ```c
  if	该轮迭代最优路径长度<当前最优路径长度
      更新最优路径和长度
  end
  ```

- 更新信息素浓度

  ```c
  solution(i)//每只蚂蚁走过的总路径长度
  add(i,j)=Q/solution(i)//∆τ_ij
  phe(i,j)=phe(i,j)*rout+add(i,j)
  ```

- 蚂蚁数据初始化

  初始化蚂蚁路线矩阵矩阵和禁忌表

## 关键代码

- 将M只蚂蚁随机分配到N个城市中

  ```c
  for(int i = 0;i<M;i++)//y表示当前迭代次数，M蚂蚁数，N城市数
  {
      map[i][0] = (y+i)%N;//蚂蚁路线
      vis[i][ map[i][0]] = 1;//访问城市(禁忌表)
  }
  ```

- 转移概率计算

  ```c
  psum += pow(phe[map[i][s-1]][j],alphe) * pow(expectVa[map[i][s-1]][j],betra)
  //phe信息素浓度，map蚂蚁路线，expectVa启发函数
  ```

- 轮盘赌方式

  ```c
  double drand = (double)(rand())/(RAND_MAX+1);//(0,1]随机数
  for(j = 0;j<N;j++)
  {
      if(vis[i][j]==0)//0表示未访问
      {
  		pro += (pow(phe[map[i][s-1]][j],alphe) * pow(expectVa[map[i][s-1]][j],betra))/psum;
  		if(pro>=drand)break;
  	}
  }
  ```

- 信息素更新

  ```c
  //完成add矩阵的构建---∆τ_ij
  add[map[i][j]][map[i][j+1]]=Q/solu;//每只蚂蚁的路线的总距离solu
  //完成信息素浓度的更新
  phe[i][j] = phe[i][j]*rout + add[i][j];
  ```

## 运行结果

**M**：9	**迭代**：20 	**alphe** = 2	**betra** = 2	**rout** = 0.7	**Q** = 10

![](https://gitee.com/cpicture/picture-1/raw/master/20210607215925.png)

**M**：9	**迭代**：20 	**alphe** = 0	**betra** = 2	**rout** = 0.7	**Q** = 10  **只根据路径长度**

![](https://gitee.com/cpicture/picture-1/raw/master/20210607213623.png)

**M**：9	**迭代**：20 	**alphe** = 2	**betra** = 0	**rout** = 0.7	**Q** = 10  **只根据信息素浓度**

![](https://gitee.com/cpicture/picture-1/raw/master/20210607220011.png)

**M**：9	**迭代**：1 	**alphe** = 2	**betra** = 3	**rout** = 0.7	**Q** = 10  **只迭代一次，两次就会得到19**

![](https://gitee.com/cpicture/picture-1/raw/master/20210607214105.png)

### 将M个蚂蚁放到同一个起点

**M**：9	**迭代**：180 	**alphe** = 2	**betra** = 2	**rout** = 0.7	**Q** = 10 **第一个城市为4**

![](https://gitee.com/cpicture/picture-1/raw/master/20210607214955.png)

**M**：9	**迭代**：5	**alphe** = 2	**betra** = 2	**rout** = 0.7	**Q** = 10 **第一个城市为4**

![](https://gitee.com/cpicture/picture-1/raw/master/20210607215136.png)

**M**：9	**迭代**：20	**alphe** = 2	**betra** = 2	**rout** = 0.7	**Q** = 10 **第一个城市为2**   **????**

![疑惑[doge]](https://gitee.com/cpicture/picture-1/raw/master/20210607215309.png)

## 代码实现

```c
#include <stdio.h>
#include <stdlib.h>
#include <time.h>


	double graph[6][6]=
{
    {0.0,6,2,1,1000,1000},
    {6,0.0,6,1000,3,1000},
    {2,6,0.0,2,2,4},
    {1,1000,2,0.0,1000,5},
    {1000,3,2,1000,0.0,3},
    {1000,1000,4,5,3,0.0}
};

int main()
{
    int N = 6;//城市的个数
    int M = 9;//每一轮中蚂蚁的个数
    int RcMax = 2;//迭代次数
    int IN = 1;//信息素的初始量
	double add[N][N];//每一段的信息素增量数组
	int map[M][N];//每一只蚂蚁的所走的路线记录
	int vis[M][N];//记录每一只访问过的城市，0表示未访问，1表示以访问
	double expectVa[N][N];//启发函数 = 1.0/距离
	double phe[N][N];//每一段路径上的信息素
	double MAX = 0x7fffffff;
	double alphe,betra,rout,Q;//alphe信息素的影响因子，betra路线距离的影响因子，rout信息素的保持度，Q用于计算每只蚂蚁在其路迹留下的信息素增量
	double bestSolution = MAX;//最短距离
	int bestWay[N];//最优路线

	//初始化变量参数和信息数组
	alphe = 2;
	betra = 3;
	rout = 0.7;
	Q = 10;
	//初始化城市图  TODO
	for(int i = 0;i<N;i++ )
	{
		graph[i][i] = 1000;
	}
	//初始化路线记录数组
	for(int i = 0;i<M;i++ )
	{
        for(int j = 0;j<N;j++)
        {
            map[i][j] = -1;
            vis[i][j] = 0;
        }
	}
	//初始化启发函数
	for(int i = 0;i<N;i++ )
	{
        for(int j = 0;j<N;j++)
        {
            expectVa[i][j] = 1.0/graph[i][j];
        }
	}
	//初始化信息素数组
	for(int i = 0;i<N;i++ )
	{
        for(int j = 0;j<N;j++)
        {
            phe[i][j] = IN;
        }
	}
	//迭代运算
	for(int y = 0; y<RcMax;y++)
	{
        for(int i = 0;i<M;i++)//为每只蚂蚁分配起点
        {
            map[i][0] = (y+i)%N;
            // map[i][0] = 1;
            vis[i][ map[i][0]] = 1;
        }
	//完成该轮中每一只蚂蚁的路线选择
        int s = 1;
        while(s<N)
        {
            for(int i = 0;i<M;i++)
            {
                double psum = 0;
                for(int j = 0;j<N;j++)
                {
                    if(vis[i][j]==0)
                    {
                        psum += pow(phe[map[i][s-1]][j],alphe) * pow(expectVa[map[i][s-1]][j],betra);
                    }
                }
                //srand((unsigned)time(NULL));
                double drand = (double)(rand())/(RAND_MAX+1);
                double pro = 0;
                int j;
                for(j = 0;j<N;j++)
                {
                if(vis[i][j]==0)
                {
                    pro += (pow(phe[map[i][s-1]][j],alphe) * pow(expectVa[map[i][s-1]][j],betra))/psum;
                    if(pro>=drand)
                    {
                    break;
                    }
                }

                }
                vis[i][j] = 1;
                map[i][s] = j;
            }
        s++;
        }
	//保存最优路线
        double solution = 0;
        for(int i = 0;i<M;i++)
        {
            solution = 0;
            for(int a =0;a<N-1;a++)
            {
                solution+= graph[map[i][a]][map[i][a+1]];
            }
            solution+=graph[map[i][0]][map[i][5]];
            if(solution<bestSolution)
            {
                for(int j = 0;j<N;j++)
                {
                    bestWay[j] = map[i][j];
                }
                bestSolution = solution;
            }
        }

	//计算每一只蚂蚁在其每一段路径上留下的信息素增量
	//初始化信息素增量数组
        for(int i = 0;i<N;i++)
        {
            for(int j = 0;j<N;j++)
            {
                add[i][j] = 0;
            }
        }
        for(int i = 0;i<M;i++)
        {
            //先算出每只蚂蚁的路线的总距离solu
            double solu = 0;
            for(int a =0;a<N-1;a++)
            {
                solu += graph[map[i][a]][map[i][a+1]];
            }
            solu+=graph[map[i][0]][map[i][5]];
            int j;
            double d = Q/solu;
            for( j = 0 ;j<N-1;j++)
            {
                add[map[i][j]][map[i][j+1]] += d;
            }
	 //注意由于每一只蚂蚁的起始点是随机设置的，所以从终点到起点也要有增加信息素
            add[map[i][N-1]][map[i][0]] += d;
        }

	//更新信息素
        for(int i=0;i<N;i++)
        {
            for(int j = 0;j<N;j++)
            {
                phe[i][j] = phe[i][j]*rout + add[i][j];
                //为信息素设置一个下限值和上限值
                if(phe[i][j]<0.0001)
                {
                    phe[i][j] = 0.0001;
                }
                if(phe[i][j]>200)
                {
                    phe[i][j] = 200;
                }
            }
        }
        //恢复路线记录数组
        for(int i = 0;i<M;i++ )
        {
            for(int j = 0;j<N;j++)
            {
                map[i][j] = -1;
                vis[i][j] = 0;
            }
        }
    }

//输出最优路线结果
   for(int i = 0;i<N;i++ )
	{
	    printf("-%d",bestWay[i]+1);
	}
	printf("\n");
	printf("最优路线的总长度是：%f\n",bestSolution);
}
```



