## 更新日志

> 说明：本文件分为上下两部分。
> - 上半部分（本次发布）会被 GitHub Action 自动读取，可一次填写多条记录。
> - 下半部分（历史归档）不会自动用于 Release，用于长期查阅。

## 本次发布（上半部分，自动用于 Release）
<!-- RELEASE:START -->
<a id="feat-seed-marl-20260226"></a>
**2026.2.26**
1. 在 `HAPPO`、`MADDPG`（原描述中有时写作 `MADDOG`）、`MAPPO`、`MATD3` 中新增 `seed` 参数（随机种子）配置，提升训练与评估的可复现性。
  - 用于统一控制环境初始化、采样过程及模型训练中的随机性来源，便于复现实验结果。

<a id="fix-issue-8"></a>
**2026.2.19**
1. 修复 [issue-8](https://github.com/Ronchy2000/Multi-agent-RL/issues/8)：`obtain_episode()` 现在在达到终止状态（`done=True`）时提前结束采样，`length` 仅作为最大步数上限。
  - 影响文件：`solver.py`、`MC_Basic.py`、`MC_Exploring_Starts.py`、`MC_epsilon_greedy.py`。
  - 这样可避免 episode 在终止后继续采样带来的回报偏差，更符合 episodic 任务定义。

<a id="fix-issue-7"></a>
**2026.2.19**
1. 修复 [issue-7](https://github.com/Ronchy2000/Multi-agent-RL/issues/7)：修正 `MC_Basic.py` 中 `mc_basic_simple` 与 `mc_basic_simple_GUI` 的缩进错误。
  - `sum_qvalue_list.append(sum_qvalue)` 现在位于 `for each_episode in episodes:` 循环内部，确保每条 episode 的回报都被统计。
  - 修复后 `self.qvalue[state][action] = np.mean(sum_qvalue_list)` 将基于完整采样集合计算均值，不再只使用最后一条 episode 的回报。

<a id="fix-issue-1"></a>
**2026.2.15**
1. 修复 [issue-1](https://github.com/Ronchy2000/Multi-agent-RL/issues/1)：第 8 章 TD-Linear（线性函数逼近）实现中的两个问题：
  - 修正 `1.TD-Linear.py` 中 `reward_list` 与 `grid_env.py` 的 `Rsa` 奖励索引顺序不一致的问题（索引约定固定为 `[other, target, forbidden, overflow]`），避免 `policy_evaluation()` 得到错误的状态值。
  - `grid_env.py` 新增 `reward_list` 可选参数，使每个算法脚本都可以通过 `GridEnv(..., reward_list=[...])` 独立配置奖励函数（无需手动改 `grid_env.py`）。
  - 修正 TD(0) 线性函数逼近权重更新遗漏 $\phi(s_t)$ 的问题。
    权重更新公式：
    ```math
    w \leftarrow w + \alpha \delta_t \phi(s_t)
    ```
    其中：
    ```math
    \delta_t = r + \gamma \phi(s_{t+1})^\top w - \phi(s_t)^\top w
    ```

<!-- RELEASE:END -->

## 历史归档（下半部分，不自动用于 Release）
<a id="history-legacy-format"></a>
**历史记录（旧格式归档）**
1. 新功能
  - 🚀 为 `MATD3` 添加了训练好的模型，可无需训练直接下载使用。
  - 🌟 新增 `HAPPO-MAPPO_Continous_Heterogeneous` 算法，支持异质智能体训练。
  - ✨ 新增 `MAPPO_Continous_Homogeneous` 算法，优化同质智能体训练效率。
  - 📦 将新算法添加到自动化打包发布流程中。
2. 修复
  - 🐛 无。
3. 文档
  - 📝 添加了 `MATD3` 追逃环境效果 GIF。
  - 📖 添加了新算法的使用文档和示例。
  - 🌐 更新 `README.md`、`README_en.md` 以包含新算法介绍。
