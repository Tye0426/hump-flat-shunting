# SCIP 联合 SAN 后端

使用 PySCIPOpt，无需 Gurobi 或其许可证。

在本目录打开 PowerShell：

```powershell
python -m venv .venv
.\.venv\Scripts\python -m pip install -r requirements.txt
.\.venv\Scripts\python -m uvicorn app:app --host 127.0.0.1 --port 8000
```

在解压后的网站根目录另开终端执行 `python -m http.server 8080`，打开
http://localhost:8080/，选择 SCIP，API 地址填写 `http://127.0.0.1:8000/solve`。

GET /health 实际求解二元模型；GET /self-test 运行四辆车案例。
POST /solve 保留请求结构，返回 solverEngine=scip、backendVersion=3.0.0-scip。

保留联合 t/x 模型、奖励目标、容量复核与外层搜索。候选生成通过 no-good 约束排除已找到赋值后再次求解；其搜索顺序和性能与 Gurobi 解池不同。
poolSize 限制候选赋值数，一批存储解可能略超限；0 对应内部安全上限10000。快速和强化阶段共用此上限。
时间或数量截断标记 candidatePoolRestricted，不能据此证明全局最优。
无合法 MIP 候选时沿用整组移动回退；前端继续与浏览器求解器并行比较。

环境变量 SCIP_SAN_FAST_MAX_SECONDS（默认0.5）、SCIP_SAN_MODEL_MAX_SECONDS（默认3）、SCIP_THREADS（0表示不覆盖默认设置）。实际每轮时限还受总时限和分配规则控制。

静态托管仅发布前端；后端部署 Python 服务或使用 Dockerfile。线上使用 HTTPS，并设置 ALLOWED_ORIGINS 为前端域名。
安装 httpx 后，在解压后的网站根目录执行 `python tests/verify_scip.py` 验证接口及动作回放。
