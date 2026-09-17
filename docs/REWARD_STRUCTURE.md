# 上台阶 DWAQ 奖励结构

楼梯项目复用 `03_walk` 的 DWAQ 环境奖励，不另造一套会与行走不一致的 reward；新增的是
`LENS110_RANDOM_STAIRS_ROUGH_TERRAINS_CFG` 和 `terrain_levels_completed` 课程。课程从 row 0 开始，
满足接近完整 60 s、有足够行程且无失败才计成功，连续两次升阶，摔倒/失败降阶。

独立配置在共享基座的
`source/legged_lab/legged_lab/tasks/locomotion/dwaq/config/lens110/lens110_stairs_dwaq_env_cfg.py`，
逐项奖励和课程机制见 [`docs/REWARD_FRAMEWORKS.md`](../../../docs/REWARD_FRAMEWORKS.md)。
