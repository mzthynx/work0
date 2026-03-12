# CG-Lab：万有引力粒子群仿真项目

基于 Taichi 实现的 GPU 并行万有引力粒子群仿真，支持交互式渲染与模块化开发。

---

## 项目架构
本项目严格遵循 `src` 布局规范，目录结构如下：
CG-Lab/
├── .gitignore # 忽略本地环境文件（如 .venv/）
├── README.md # 项目说明文档
├── pyproject.toml # uv 项目配置与依赖管理
├── uv.lock # uv 依赖锁定文件
└── src/
└── Work0/
├── init.py # 包标识文件
├── config.py # 全局配置（粒子数量、引力常数、渲染参数等）
├── physics.py # 物理计算模块（万有引力公式、位置 / 速度更新逻辑）
└── main.py # 主入口（初始化 Taichi、启动仿真与交互渲染）
---

## 环境依赖
- **IDE**: Trae IDE
- **包管理**: uv
- **核心库**: Taichi (GPU 并行计算)
- **Python 版本**: 3.10+

### 快速搭建
1. 初始化项目与虚拟环境：
   ```bash
   uv init CG-Lab
   cd CG-Lab
2.安装Taichi依赖
  ```bash
  uv add taichi
3.运行项目
  ```bash
  uv run -m src.Work0.main
