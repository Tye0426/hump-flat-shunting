# 本次更换范围

基准：C:/Users/10763/Desktop/rail-shunting-h5-updated。
只交付平面调车，桌面原文件未修改。原“更新说明.md”的容量校验与通知更新仍适用。

Gurobi后端替换为PySCIPOpt 6.3.0；联合t/x模型、目标、解码、容量复核与外层搜索保持原逻辑。
候选生成使用排除已找到赋值的约束重复求解，达到预算后标记受限。
前端引擎标识、统计、比较和导出改为SCIP；修复null成本被当作0成本比较的问题。
默认浏览器模式沿用原版；使用SCIP需选择该模式并填写后端地址。
styles.css、yard-model.js与solver-worker.js保持字节一致，来源与输出摘要见基准校验.json。

PySCIPOpt文档：https://pyscipopt.readthedocs.io/en/latest/api/model.html
