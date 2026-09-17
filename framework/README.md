# 上台阶训练框架入口

`isaaclab_shared` 指向共享 DWAQ/地形源码。独立楼梯配置通过复制地形配置对象并启用
`terrain_levels_completed` 注册为 `LeggedLab-Isaac--DWAQ-Lens110-Stairs-v0`，不会改变平地 DWAQ 的
module-level terrain singleton。
