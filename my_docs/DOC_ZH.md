# SolveSpace 求解器文档

## 简介

SolveSpace中的草图由三个基本元素组成：参数、实体和约束。

**参数(Slvs_Param)** 是一个实数，内部用双精度浮点变量表示。这些参数是求解器为满足约束条件而修改的未知变量。

**实体(Slvs_Entity)** 是几何对象，如点、线段或圆。实体由参数和其他实体定义。例如，三维空间中的点由三个参数表示，对应于其在基础坐标系中的x、y和z坐标。

**约束(Slvs_Constraint)** 是实体的几何属性或多个实体间的关系。例如，点-点距离约束将设置两个点实体之间的距离。

## 核心概念

### 句柄系统
参数、实体和约束通常通过它们的句柄(Slvs_hParam, Slvs_hEntity, Slvs_hConstraint)引用。这些句柄是从1开始的32位整数值。使用句柄而非指针有助于避免内存损坏。

### 分组机制
实体和约束被分配到组中。组是一组同时求解的实体和约束。在参数化CAD系统中，一个组通常对应一个草图。

### 求解过程
1. 定义参数、实体和约束集
2. 为每个参数提供初始猜测值
3. 对指定组运行求解器
4. 求解结果有三种可能：
   - 所有约束满足(成功)
   - 约束不一致(生成不一致约束列表)
   - 无法找到解(生成未满足约束列表)

## 实体类型

### SLVS_E_POINT_IN_3D
三维空间中的点，由三个参数定义：
- param[0] x坐标
- param[1] y坐标
- param[2] z坐标

### SLVS_E_POINT_IN_2D
工作平面内的点，由工作平面和两个参数定义：
- wrkpl 工作平面
- param[0] u坐标
- param[1] v坐标

### SLVS_E_NORMAL_IN_3D
三维法向量，由单位四元数定义：
- param[0] w
- param[1] x
- param[2] y
- param[3] z

### SLVS_E_LINE_SEGMENT
线段，由两个端点定义：
- point[0] 起点
- point[1] 终点

### SLVS_E_CIRCLE
完整的圆，由以下定义：
- normal 平面法线
- point[0] 圆心
- distance 半径

## 约束类型

### SLVS_C_PT_PT_DISTANCE*
两点(ptA和ptB)之间的距离等于valA(无符号距离)

### SLVS_C_POINTS_COINCIDENT*
两点(ptA和ptB)重合(完全重叠)

### SLVS_C_PT_ON_LINE*
点(ptA)位于线(entityA)上

### SLVS_C_ANGLE*
两条线(entityA和entityB)之间的角度等于valA(度)

### SLVS_C_PARALLEL*
两条线(entityA和entityB)平行

### SLVS_C_ARC_LINE_TANGENT**
圆弧(entityA)与直线(entityB)相切

## 使用求解器

求解器以DLL形式提供，可用于大多数基于Windows的开发工具。示例包括：
- C/C++: CDemo.c
- VB.NET: VbDemo.vb

## 版权声明
版权所有 2009-2013 Jonathan Westhues