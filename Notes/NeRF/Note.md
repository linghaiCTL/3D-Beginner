# NeRF
**论文链接** https://arxiv.org/abs/2003.08934  

## 内容概述
这篇论文中的方法主要做的事情是：  
对于一个3D场景  
1.从不同角度拍很多张照片并记录相机的位置和角度构建训练数据集  
2.基于训练数据集，训练一个多层mlp  
3.对于任意一个新输入的相机位置和角度，使用多层mlp来预测在新角度下，相机能拍摄到的2D图像  

Note：  
1.3D场景的信息是存储在训练之后的mlp参数中的。任意一次训练得到的一组mlp参数只能用于生成同一个3D场景的新视角照片  
2.mlp不直接预测整张图片，而是只预测空间中一个点，在特定观察方向上表现出来的颜色和遮光率

## 体素渲染算法原理
我们把3D场景看成空间中的一堆发光点，每个点在特定角度看会有一定的颜色和遮光率（因为它们互相遮蔽，靠近相机的点会遮住它身后点发出的光线）。  
体素渲染算法解决的问题是：假设我们已经知道了空间中每个点的在任意角度的颜色和遮光率（由训练的mlp给出），如何重建从特定位置特定角度看到的2D图像。 

Note：  
mlp是把输入（空间中某个点的坐标x，y，z，和看它的方向$\theta,\phi$）映射到输出（这个点在这个方向上的颜色r，g，b和遮光率$\sigma$）的**连续**函数。因此虽然说把3D场景看成很多点，其实这些点在空间中是稠密的，处处都是。只不过对应这空气的点从各个角度看遮光率都近似是0，对应不透光表面的点从特定角度看遮光率近似为1  

