<p align="center"><a href="#zh">中文</a> &nbsp;|&nbsp; <a href="#en">English</a></p>
<a id="zh"></a>

# 06 · 上台阶（DWAQ Stairs）

## 项目定位

这是独立的 DWAQ 楼梯训练项目。它复用行走的 actor、VAE、命令和奖励项，但使用独立 terrain generator 和
success-gated curriculum，因此任务 ID、实验和导出目录与 `03_walk` 分开。

- 训练任务：`LeggedLab-Isaac--DWAQ-Lens110-Stairs-v0`
- PLAY 任务：`LeggedLab-Isaac--DWAQ-Lens110-Stairs-PLAY-v0`
- 配置：`framework/isaaclab_shared/lens110/legged_lab_lbot/source/legged_lab/legged_lab/tasks/locomotion/dwaq/config/lens110/lens110_stairs_dwaq_env_cfg.py`
- 地形：`LENS110_RANDOM_STAIRS_ROUGH_TERRAINS_CFG`
- 难度范围：当前随机路径表面高度控制为约 `5..30 cm`

## 训练架构和演示

DWAQ、PPO、β-VAE、76/380/21 接口、完整 reward、楼梯 terrain 和成功门控 curriculum 见 [`docs/TRAINING_ARCHITECTURE.md`](docs/TRAINING_ARCHITECTURE.md)。

<video controls width="720" src="docs/media/stairs-blind-terrain-demo.mp4"></video>

[打开或下载盲走楼梯全地形演示视频](docs/media/stairs-blind-terrain-demo.mp4)

![盲走训练地形](docs/media/stairs-terrain.png)

### 附加视觉楼梯演示

以下视频是视觉上楼梯/视觉识别楼梯的补充演示，不属于当前 DWAQ 盲行走 actor 的输入契约；当前 DWAQ actor 仍只使用本体感觉、命令、历史和步态相位。

<video controls width="720" src="docs/media/stairs-vision-up-demo.mp4"></video>

[打开或下载视觉上楼梯演示](docs/media/stairs-vision-up-demo.mp4)

<video controls width="720" src="docs/media/stairs-vision-recognition-demo.mp4"></video>

[打开或下载视觉识别楼梯演示](docs/media/stairs-vision-recognition-demo.mp4)

## 目录

```text
06_stairs/
├── framework/isaaclab_shared -> framework/isaaclab_shared
├── data/terrain/source -> 共享地形实现
├── exports/packages/          # 仅存放楼梯独立验证后的导出包
├── exports/training_packages/ # 仅存放楼梯独立验证后的训练包
├── experiments/dwaq_runs -> 共享 DWAQ logs/checkpoints
└── docs/
```

## 训练

```bash
./scripts/train.sh \
  --headless --num_envs 4096
```

课程逻辑从 terrain row `0` 开始；完成接近完整 `60 s`、有足够行程且无失败的 episode 计为成功，连续两次才
升阶，摔倒/失败降阶。`terrain_levels` 是地形 row/平均等级，不是百分比。

## 当前状态

楼梯训练/PLAY 入口是本次整理新增的注册项，Python 编译已通过；在 `isaaclab` 环境中还需要重新执行配置加载、
地形可见性和短时 smoke test，不能仅凭注册成功或已有 DWAQ checkpoint 声称楼梯已经收敛。

奖励结构见 [`docs/REWARD_FRAMEWORKS.md`](docs/REWARD_FRAMEWORKS.md) 的楼梯章节。

<a id="en"></a>

## English

This repository contains the independent stair-walking project. It reuses the DWAQ family of PPO and beta-VAE components and adds a success-gated stair terrain curriculum. The stair training and PLAY task registrations are separate from flat walking.

Run `./scripts/train.sh --headless --num_envs 4096` after loading the local Isaac Lab environment. Validate terrain visibility, riser geometry, row-0 behavior, promotion/demotion logic, and short replay before publishing a checkpoint. The project has its own `exports/packages/` and `exports/training_packages/`; no walking package is treated as a stair package.

The current evidence covers registration and code-level checks, not convergence. Keep the terrain seed, curriculum state, checkpoint, joint order, and replay result with each release. Read `docs/REWARD_FRAMEWORKS.md` and `docs/INTERFACE_CONTRACTS.md` before reproduction.
