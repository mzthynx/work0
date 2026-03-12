# CG-Lab：万有引力粒子群仿真项目

基于 Taichi 实现的 GPU 并行万有引力粒子群仿真，支持交互式渲染与模块化开发。

---

## 项目架构

本项目严格遵循 `src` 布局规范，目录结构如下：

---

```text
CG-Lab/
├── .gitignore          # Git忽略配置，屏蔽虚拟环境、IDE配置等文件
├── README.md           # 项目说明文档
├── pyproject.toml      # uv包管理配置文件
├── uv.lock             # uv依赖版本锁定文件
└── src/
    └── Work0/
        ├── __init__.py  # Python包标识文件
        ├── config.py    # 全局物理/渲染参数配置
        ├── physics.py   # 粒子物理逻辑核心实现（GPU并行）
        └── main.py      # 项目主入口，负责Taichi初始化、GUI渲染、交互驱动
```
---
## 环境依赖
- **IDE**: Trae IDE
- **包管理**: uv
- **核心库**: Taichi (GPU 并行计算)
- **Python 版本**: 3.10+

---

## 快速搭建

### 1. 初始化项目与虚拟环境
```bash
uv init CG-Lab
cd CG-Lab
```
---
### 2. 安装Taichi依赖
```bash
uv add taichi
```
---
### 3. 运行项目
```bash
uv run -m src.Work0.main
```
---
## 核心参数配置（config.py）

所有可配置参数均在 src/Work0/config.py 中定义，支持按需修改：
|参数名	|示例值	|功能说明|
|------|------|------|
|NUM_PARTICLES	|10000	|粒子总数|
|GRAVITY_STRENGTH	|0.001	|鼠标对粒子的引力强度|
|DRAG_COEF	|0.98	|空气阻力系数（0~1，越小阻力越大）|
|BOUNCE_COEF	|-0.8	|边界反弹系数（负数表示反向，绝对值越小损耗越大）|
|WINDOW_RES	|(800, 600)	|仿真窗口分辨率|
|PARTICLE_RADIUS	|1.5	|粒子绘制半径|
|PARTICLE_COLOR	|0x00BFFF	|粒子颜色（十六进制，如天蓝色）|
---
## 代码逻辑
---
### 1. 配置层（config.py)


统一管理全局参数，实现参数与计算逻辑解耦，修改参数无需改动核心物理代码。

---
### 2. 物理层（physics.py）


GPU 数据结构：用 ti.Vector.field 在显存中存储粒子位置（pos）、速度（vel）
初始化：init_particles() 为所有粒子分配随机初始位置
并行更新：update_particles()（@ti.kernel 装饰器）实现核心物理逻辑：
计算粒子到鼠标的方向与距离
施加鼠标引力，更新粒子速度
空气阻力衰减，模拟速度损耗
边界碰撞检测，实现弹性反弹并扣除能量

---
### 3. 主入口（main.py）


初始化 Taichi 运行时（自动选择 GPU/CPU 后端）
调用物理层完成粒子初始化，编译 GPU 内核
启动 GUI 渲染循环，处理鼠标交互
实时绘制粒子群动态效果

---
## 功能实现


✅ GPU 并行计算：基于 Taichi 的 ti.kernel 实现粒子群并行更新
✅ 交互式渲染：支持鼠标拖拽粒子、窗口缩放等交互操作
✅ 模块化设计：代码分层清晰，便于后续扩展新的物理模型或渲染效果
✅ 标准化运行：通过 uv run -m src.Work0.main 一键启动仿真

---
## 效果展示

<img src="https://github.com/mzthynx/work0/blob/master/work0.gif" width="600" alt="功能演示GIF">
演示视频 / GIF 展示：粒子群在万有引力作用下的聚集、碰撞与交互效果，可体现 GPU 并行计算的流畅性。
交互说明
鼠标拖拽：可拖动单个粒子改变其位置与速度
窗口缩放：自动适配不同分辨率，保持渲染比例

---


