## Overview

这是一项Astar路径规划算法的研究，针对无人车在移动过程中可能遭遇的碰撞风险与频繁减速等问题，提出了一种基于Anytime Repairing A\*（ARA\*）算法的改进方案——Bidirectional Repairing A\*（BRA\*）算法。该算法旨在生成路径时，尽量避开狭窄区域或者远离障碍物，并在条件允许的情况下，最小化路径的转角个数，从而提高无人车的安全性和移动效率。

- [Overview](#overview)
- [Requirements and Features](#requirements-and-features)
- [Directory Structure](#directory-structure)
- [Guide](#guide)
  - [Abstract](#abstract)
  - [Results](#results)
    - [Staircase](#staircase)
    - [Maze](#maze)
    - [Spiral](#spiral)
    - [Simple](#simple)
    - [Complex](#complex)
  - [More](#more)



## Requirements and Features

- 算法将双向搜索策略与ARA\*算法相结合，以弥补ARA\*算法在路径优化中偏向于目标点附近节点的不足。双向搜索策略能够减少冗余节点，提高路径搜索效率，但难以保证两棵搜索树前沿的交点是最优的，而ARA\*算法恰好弥补了这一不足。
- 采用Octile距离计算当前节点到目标节点的估计代价，在总代价函数中引入了转角损失项，减少了路径长度和转角个数。
- 为了降低无人车与障碍物发生碰撞的可能性，对地图中的障碍物进行了膨胀处理，采用自适应函数设计动态权重，降低了搜索列表中离障碍物较近的节点的搜索优先级。
- 针对无人车安全性和移动效率，专门设计了新的优化收敛性判定准则，提升了算法效率，并改善了双向搜索中搜索树交点的质量。



## Directory Structure

```txt
│   detail.pdf
│   LICENSE
│   README.md
│
├───code
│   ├───algorithm
│   │       ARAstar.py
│   │       Astar.py
│   │       Bidirectional_Astar.py
│   │       BRAstar.py
│   │
│   └───map
│           Env.py
│           Map.py
│           Plot.py
│
└───img
```



## Guide

### Abstract
<p align = "center">    
  <img src="./img/BRAstar算法流程.png"  align="middle"  width="600" />
</p>

### Results

图左上、右上、左下、右下分别呈现了四种算法A\*、Bidirectional A\*、ARA\*和BRA\*在不同地图上的路径规划结果，表对应之。

#### Staircase

<div align=left>
<table>
  <tr>
    <td><img src=".\img\阶梯地图a.png" width="250"/></td>
    <td><img src=".\img\阶梯地图b.png" width="250"/></td>
  </tr>
  <tr>
    <td><img src=".\img\阶梯地图c.png" width="250"/></td>
    <td><img src=".\img\阶梯地图d.png" width="250"/></td>
  </tr>
</table>
</div>

| 算法              | 搜索时间 | 搜索节点数 | 转角个数 | 安全系数 | 路径长度 |
| ----------------- | -------- | ---------- | -------- | -------- | -------- |
| A*                | 116      | 1170       | 18       | 54.5%    | 55       |
| Bidirectional  A* | 59       | 1363       | 22       | 72.7%    | 55       |
| ARA*              | 65       | 924        | 19       | 50.9%    | 55       |
| BRA*              | 61       | 1311       | 6        | 0.0%     | 59       |

#### Maze

<div align=left>
<table>
  <tr>
    <td><img src=".\img\迷宫地图a.png" width="250"/></td>
    <td><img src=".\img\迷宫地图b.png" width="250"/></td>
  </tr>
  <tr>
    <td><img src=".\img\迷宫地图c.png" width="250"/></td>
    <td><img src=".\img\迷宫地图d.png" width="250"/></td>
  </tr>
</table>
</div>

| 算法              | 搜索时间 | 搜索节点数 | 转角个数 | 安全系数 | 路径长度 |
| ----------------- | -------- | ---------- | -------- | -------- | -------- |
| A*                | 89       | 1429       | 21       | 71.4%    | 77       |
| Bidirectional  A* | 42       | 830        | 22       | 80.5%    | 77       |
| ARA*              | 111      | 1733       | 18       | 81.8%    | 77       |
| BRA*              | 23       | 616        | 23       | 44.6%    | 83       |

#### Spiral

<div align=left>
<table>
  <tr>
    <td><img src=".\img\螺旋地图a.png" width="250"/></td>
    <td><img src=".\img\螺旋地图b.png" width="250"/></td>
  </tr>
  <tr>
    <td><img src=".\img\螺旋地图c.png" width="250"/></td>
    <td><img src=".\img\螺旋地图d.png" width="250"/></td>
  </tr>
</table>
</div>

| 算法              | 搜索时间 | 搜索节点数 | 转角个数 | 安全系数 | 路径长度 |
| ----------------- | -------- | ---------- | -------- | -------- | -------- |
| A*                | 135      | 1424       | 30       | 95.9%    | 172      |
| Bidirectional  A* | 48       | 1330       | 31       | 97.7%    | 173      |
| ARA*              | 103      | 1590       | 29       | 95.3%    | 172      |
| BRA*              | 91       | 1720       | 25       | 9.8%     | 193      |

#### Simple

<div align=left>
<table>
  <tr>
    <td><img src=".\img\简单地图a.png" width="250"/></td>
    <td><img src=".\img\简单地图b.png" width="250"/></td>
  </tr>
  <tr>
    <td><img src=".\img\简单地图c.png" width="250"/></td>
    <td><img src=".\img\简单地图d.png" width="250"/></td>
  </tr>
</table>
</div>

| 算法              | 搜索时间 | 搜索节点数 | 转角个数 | 安全系数 | 路径长度 |
| ----------------- | -------- | ---------- | -------- | -------- | -------- |
| A*                | 96       | 979        | 18       | 43.5%    | 69       |
| Bidirectional  A* | 55       | 1426       | 22       | 58.0%    | 69       |
| ARA*              | 88       | 1298       | 17       | 49.3%    | 69       |
| BRA*              | 66       | 1322       | 13       | 6.0%     | 83       |

#### Complex

<div align=left>
<table>
  <tr>
    <td><img src=".\img\复杂地图a.png" width="250"/></td>
    <td><img src=".\img\复杂地图b.png" width="250"/></td>
  </tr>
  <tr>
    <td><img src=".\img\复杂地图c.png" width="250"/></td>
    <td><img src=".\img\复杂地图d.png" width="250"/></td>
  </tr>
</table>
</div>

| 算法              | 搜索时间 | 搜索节点数 | 转角个数 | 安全系数 | 路径长度 |
| ----------------- | -------- | ---------- | -------- | -------- | -------- |
| A*                | 24       | 245        | 18       | 78.0%    | 41       |
| Bidirectional  A* | 16       | 234        | 22       | 73.2%    | 41       |
| ARA*              | 7        | 66         | 15       | 78.0%    | 41       |
| BRA*              | 24       | 456        | 12       | 20.8%    | 48       |

### More

本仓库包括源代码和详细文档，您可以通过阅读  [detail.pdf](detail.pdf) 了解算法的详细设计、原理和实验。
