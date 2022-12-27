![](https://graphcoloring-1312128486.cos.ap-nanjing.myqcloud.com/Paper.jpg)

# Welsh-Powell算法在图着色问题中的应用

参考资料：

* oiwiki：https://oi-wiki.org//graph/color/

* wikipedia：https://en.wikipedia.org/wiki/Graph_coloring

参考文献：

* Welsh, D.J.A. and Powell, M.B. (1967) An Upper Bound for the Chromatic Number of a Graph and Its Application to Timetabling Problems. 《The Computer Journal》, 10, 85-86. 

<!--more-->

# Welsh-Powell算法

## 算法简介

1967年，Welsh和Powell算法引入了图色数的上界，其提供了一个在静态图上运行的贪心算法。

顶点根据其度数排序，得到的贪心着色最多使用颜色，最多比图的最大度数多一个。这种启发式被称为Welsh-Powell（威尔士-鲍威尔）算法。

## 算法详解

**此点着色算法并不总能得到最小点着色数目**

Welch Powell 算法其实就是在进行贪心算法之前先对节点排了下序，具体过程如下：

*  将图 G 中的节点按度数的递减顺序进行排列（这种排列可能不是唯一的，因为有些节点的度数可能相同）

*  用第一种颜色对第一个节点着色，并按排列顺序对与前面着色节点不邻接的每一节点着上同样的颜色

*  用第二种颜色对尚未着色的节点重复步骤 (2)，用第三种颜色继续这种做法，直到所有的节点全部着上色为止



## 伪代码：

```python
color kind = 0
SortedNode = copy.deepcopy(node)
# Sort in descending order by degree
SortedNode = sorted(SortedNode , key=cmp to key(compare))
while SortedNode : color kind += 1
	i=0
	while i < len(SortedNode):
    check = True
    for j in range(len(SortedNode[i].connect)):
        if node[SortedNode[i].connect[j]].color == color kind: 
          check = False
        	break
        if check:
          node[SortedNode[i].node_number].color = color kind 
          SortedNode.remove (SortedNode[i])
          i −= 1
        i += 1
```



## Python代码

```python
import copy
from functools import cmp_to_key

global max_degree
max_degree = -1


class NODE:
    def __init__(self, number, sub_matrix):
        global max_degree
        self.node_number = number
        self.connect = []
        self.color = -1  # Which means this node haven't been colored
        for i in range(len(sub_matrix)):
            if sub_matrix[i]:
                self.connect.append(i)
        self.degree = len(self.connect)  # The number of nodes connect to it
        max_degree = max(max_degree, self.degree)


def compare(x, y):  # Sort in descending order by degree
    if x.degree < y.degree:
        return 1
    elif x.degree > y.degree:
        return -1
    return 0


node_size = -1
AdjacencyMatrix, node = [], []

node_size = int(input())
for i in range(node_size):  # Load in AdjacencyMatrix
    AdjacencyMatrix.append([])
    line = input()
    for j in range(node_size):
        AdjacencyMatrix[i].append(int(line[j]))


for i in range(node_size):  # Construct new node
    node.append(NODE(i, AdjacencyMatrix[i]))


##################################
###---Welsh-Powell Algorithm---###
##################################
color_kind = 0

SortedNode = copy.deepcopy(node)
SortedNode = sorted(SortedNode, key=cmp_to_key(compare))  # Sort in descending order by degree

while SortedNode:
    color_kind += 1
    s = ""
    i = 0
    while i < len(SortedNode):
        check = True
        for j in range(len(SortedNode[i].connect)):
            if node[SortedNode[i].connect[j]].color == color_kind:
                check = False
                break
        if check:
            node[SortedNode[i].node_number].color = color_kind
            SortedNode.remove(SortedNode[i])
            i -= 1
        i += 1

print("图中结点最大度：" + str(max_degree) + "\n根据Welsh-Powell算法分配出来的最大颜色数量："+str(color_kind))

print("详细分配情况：")
for i in range(node_size):
    print(str(i+1)+": "+str(node[i].color))

print("\n颜色对应结点情况：")
for i in range(color_kind):
    s = str(i+1) + ": "
    for j in range(node_size):
        if node[j].color == i+1:
            s += " " + str(node[j].node_number+1)
    print(s)
```



# 实验

**对中国地图进行着色：**

* 待着色原图

  ![](https://graphcoloring-1312128486.cos.ap-nanjing.myqcloud.com/MapOfChina%20original.jpg)

* 对着色块进行数字标记

  ![](https://graphcoloring-1312128486.cos.ap-nanjing.myqcloud.com/MapOfChina%20numbered.jpg)

* 抽象出邻接信息，转换为邻接矩阵

  ![](https://graphcoloring-1312128486.cos.ap-nanjing.myqcloud.com/table1.jpg)

* 将输入带入模型，得到结果

  ![](https://graphcoloring-1312128486.cos.ap-nanjing.myqcloud.com/table2.jpg)

* 在图上着色，得到结果

  ![](https://graphcoloring-1312128486.cos.ap-nanjing.myqcloud.com/MapOfChina%20colored.jpg)



经过验证，可以发现此时着色数（4）是最优解，并且在图中相邻块颜色无冲突，实验评估为模型成功。

