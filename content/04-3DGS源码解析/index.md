---
title: 04 · 3DGS 源码解析
description: 3D Gaussian Splatting 算法与底层实现
---

# 3D Gaussian Splatting

本模块记录 3DGS 的数学原理、训练流程、渲染器与源码实现。

## 学习内容

### 01. 基础原理
- 三维高斯表示
- 协方差矩阵
- 球谐函数
- Alpha Compositing

### 02. 训练与优化
- Gaussian Densification
- Gaussian Pruning
- Loss Functions
- Adaptive Density Control

### 03. CUDA Rasterizer
- Gaussian Projection
- Preprocess CUDA
- Tile-based Rendering
- Forward Rendering
- Backward Propagation

### 04. 高性能优化
- Memory Optimization
- Rasterization Acceleration
- Large-scale Reconstruction

## 阶段目标

深入理解 3DGS 训练和渲染流程，能够自主修改底层 CUDA 模块。
