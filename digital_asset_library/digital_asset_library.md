# 工业机器人数字资产库（LoongEnv-Assets）规范（MVP）

LoongEnv-Assets 的目标：把工业机器人“设计–建模–控制–实机–监控”全链路的关键产物
沉淀为可复用的数字资产，并提供版本管理、追溯与质量门禁，支撑可复现与工程化交付。

---

## 1. 资产对象模型

### 1.1 Asset vs AssetVersion
- **Asset**：逻辑资产（例如：ER15-1400三维模型、运动动力学包）
- **AssetVersion**：不可变版本（append-only），任何修改都是新版本

### 1.2 必备字段（MVP）
每个版本必须包含：
- `asset_id`：全库唯一
- `type`：资产类型
- `version`：语义化版本（SemVer）
- `artifacts`：文件清单（role + path）

---

## 2. 资产类型

- robot_variant：机型/构型
- geometry_pkg：几何与装配
- dynamics_pkg：动力学与标定
- limits_pkg：约束边界
- controller_profile：控制配置
- dataset：仿真/实测数据集（日志、标注、切片）
- test_plan：测试方案
- test_report：测试报告
---

## 3. 质量门禁（建议最小规则）

自动校验建议（按类型）：
- URDF/MJCF：关节链合法、惯量正定/半正定、单位一致
- limits：维度一致、单位一致、边界合理
- retiming_result：给出 feasible / infeasible + reason + 诊断包

---

## 4. 与 LoongEnv 模块的接口约定（概念级）

- Studio：产出 task_template / controller_profile / limits_pkg
- Twin：消费 robot_variant + scene_pkg + trajectory；产出 dataset / test_report
- Net：消费 dataset + controller_profile；产出改进版 controller_profile（新版本）
- Eye：产出运行日志 dataset；触发 evidence_pack 归档

