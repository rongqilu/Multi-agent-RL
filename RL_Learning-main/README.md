[🇨🇳 中文文档](#chinese) | [🇺🇸 English](#english)


# RL_Learning 🎉️
<a id="chinese"></a>
![Python](https://img.shields.io/badge/Python-3.7%2B-blue) ![状态](https://img.shields.io/badge/状态-重构中-orange) ![算法](https://img.shields.io/badge/算法-基础RL算法-green)


> 原始代码来源: https://github.com/jwk1rose/RL_Learning  
> 本人正在重构代码，尽量分解为更多独立模块并添加详细注释。

## 简介 📖
本项目为西湖大学赵世钰老师的强化学习课程代码实践，目前完成了1-9章的大部分代码，包括仿真环境的搭建、值迭代，策略迭代、蒙特卡洛、时序差分、状态值近似、DQN、Reinforce 等算法的实现。尽可能地追求复现，但是作者代码水平有限，不免存在许多bug以及效率低下之处，请大家仅作参考。

非常幸运能够发现这一门课，因为这门课我知道了RL。比较过市面上很多其他的资料，不管是课程还是教材的质量都是顶尖的。像赵老师一样愿意耗费如此心血，制作如此高质量的视频的老师已经很少了。谨以此开源仓库向赵老师致敬✋。

<div align="center">
  <img src="./scripts/Chapter4_Value iteration and Policy iteration/plot_figure/policy_iteration.png" width="45%" alt="值迭代算法可视化"/>
  <img src="./scripts/Chapter4_Value iteration and Policy iteration/plot_figure/value_iteration.png" width="45%" alt="策略梯度训练曲线"/>
  <p>从左到右: 策略梯度、值迭代算法可视化</p>
</div>

## 项目结构

```tree
RL_Learning-main/
├── scripts/            # 算法实现脚本
│   ├── Chapter4_Value iteration and Policy iteration/  # 第4章：值迭代和策略迭代
│   ├── Chapter5_Monte Carlo Methods/                  # 第5章：蒙特卡洛方法
│   ├── Chapter6_Stochastic_approximation/            # 第6章：随机近似
│   ├── Chapter7_Temporal-Difference learning/        # 第7章：时序差分学习
│   ├── Chapter8_Value Function Approximaton/         # 第8章：值函数近似
│   ├── Chapter9_Policy Gradient/                     # 第9章：策略梯度
│   ├── Chapter10_Actor Critic/                       # 第10章：演员-评论家方法
│   ├── grid_env.py                                   # 网格环境
│   ├── model.py                                      # 神经网络模型
│   ├── render.py                                     # 渲染工具
│   └── solver.py                                     # 求解器基类
└── README.md           # 项目说明
```

## 已实现算法

| 算法 | 状态 | 位置 | 说明 |
|------|------|------|------|
| 值迭代 (Value Iteration) | ✅ | `scripts/Chapter4_Value iteration and Policy iteration/` | 基于动态规划的最优值函数求解 |
| 策略迭代 (Policy Iteration) | ✅ | `scripts/Chapter4_Value iteration and Policy iteration/` | 基于动态规划的最优策略求解 |
| 蒙特卡洛方法 (Monte Carlo) | ✅ | `scripts/Chapter5_Monte Carlo Methods/` | 基于采样的值函数估计 |
| 时序差分学习 (TD Learning) | ✅ | `scripts/Chapter7_Temporal-Difference learning/` | 结合动态规划和蒙特卡洛的方法 |
| Q-learning | ✅ | `scripts/Chapter7_Temporal-Difference learning/` | 经典的离线强化学习算法 |
| n-step Sarsa | ✅ | `scripts/Chapter7_Temporal-Difference learning/` | 多步时序差分学习 |
| 状态值函数近似 (Value Approximation) | ✅ | `scripts/Chapter8_Value Function Approximaton/` | 使用函数近似代替表格型表示 |
| DQN (Deep Q-Network) | ✅ | `scripts/Chapter8_Value Function Approximaton/` | 深度Q网络算法 |
| Reinforce 算法 | ✅ | `scripts/Chapter9_Policy Gradient/` | 基础策略梯度算法 |
| Actor-Critic | ✅ | `scripts/Chapter10_Actor Critic/` | 结合策略梯度和值函数近似的方法 |


## 开发环境说明
### PyCharm用户注意事项
使用**PyCharm**打开本项目时，代码中的 `sys.path.append("..")` 导入语句不会报错。**PyCharm**会自动将项目根目录添加到`PYTHONPATH`中，确保模块导入正常工作。

如果您使用其他IDE或直接通过命令行运行脚本，可能需要手动设置`PYTHONPATH`环境变量
```bash
export PYTHONPATH=$PYTHONPATH:/path/to/RL_Learning-main
```
或
```python
# sys.path.append("..") # 注释掉此行
# 改为：
import os 
sys.path.append(os.path.join(os.path.dirname(os.path.dirname(os.path.abspath(__file__)))))
```

## 更新日志

<a id="fix-issue-9"></a>
**2026.4.8**  
1. 修复 [issue-9](https://github.com/Ronchy2000/Multi-agent-RL/issues/9)：修正 `scripts/Chapter5_Monte Carlo Methods/MC_epsilon_greedy.py` 中 Monte Carlo epsilon-greedy 控制实现的两处问题。  
  - 回报计算改为标准的逆序递推 `G = gamma * G + reward`，移除原先基于 `episode.index(step)` 的错位切片与重复累计逻辑，避免折扣回报被错误计算。  
  - 策略改进恢复为教材中的 epsilon-soft 更新公式，不再把策略退化为确定性的 greedy policy。  
  - 同时将 `np.divide(..., where=num_visits != 0)` 改为显式写入零值输出，避免未访问状态动作对产生未初始化的 `qvalue`。  

<a id="fix-issue-8"></a>
**2026.2.19**  
1. 修复 [issue-8](https://github.com/Ronchy2000/Multi-agent-RL/issues/8)：`obtain_episode()` 现在在达到终止状态（`done=True`）时提前结束采样，`length` 仅作为最大步数上限。  
  - 影响文件：`scripts/solver.py`、`scripts/Chapter5_Monte Carlo Methods/MC_Basic.py`、`scripts/Chapter5_Monte Carlo Methods/MC_Exploring_Starts.py`、`scripts/Chapter5_Monte Carlo Methods/MC_epsilon_greedy.py`。  
  - 这样可避免 episode 在终止后继续采样带来的回报偏差，更符合 episodic 任务定义。  

<a id="fix-issue-7"></a>
**2026.2.19**  
1. 修复 [issue-7](https://github.com/Ronchy2000/Multi-agent-RL/issues/7)：修正 `scripts/Chapter5_Monte Carlo Methods/MC_Basic.py` 中 `mc_basic_simple` 与 `mc_basic_simple_GUI` 的缩进错误。  
  - `sum_qvalue_list.append(sum_qvalue)` 现在位于 `for each_episode in episodes:` 循环内部，确保每条 episode 的回报都被统计。  
  - `episodes = []` 现在会在每个 `(state, action)` 上重新初始化，避免不同状态动作对的 episode 被错误混合到同一个 `Q(s,a)` 估计中。  
  - 修复后 `self.qvalue[state][action] = np.mean(sum_qvalue_list)` 将基于完整采样集合计算均值，不再只使用最后一条 episode 的回报。  

<a id="fix-issue-1"></a>
**2026.2.15**  
1. 修复 [issue-1](https://github.com/Ronchy2000/Multi-agent-RL/issues/1)：第8章 TD-Linear（线性函数逼近）实现中的两个问题：  
  - 修正 `scripts/Chapter8_Value Function Approximaton/1.TD-Linear.py` 中 `reward_list` 与 `scripts/grid_env.py` 的 `Rsa` 奖励索引顺序不一致的问题（索引约定固定为 `[other, target, forbidden, overflow]`），避免 `policy_evaluation()` 得到错误的状态值。  
  - `scripts/grid_env.py` 新增 `reward_list` 可选参数，使每个算法脚本都可以通过 `GridEnv(..., reward_list=[...])` 独立配置奖励函数（无需手动改 `grid_env.py`）。  
  - 修正 TD(0) 线性函数逼近权重更新遗漏 $\phi(s_t)$ 的问题。权重更新公式为 $w \leftarrow w + \alpha \delta_t \phi(s_t)$，其中 $\delta_t = r + \gamma \phi(s_{t+1})^\top w - \phi(s_t)^\top w$。


**2024.6.7**  
重大更新！原作者的渲染坐标与状态设置不一致，现已统一坐标为：  
![img.png](../img.png)

## 环境配置

```bash
# 创建虚拟环境
conda create -n rl_learning python=3.7
conda activate rl_learning

# 安装依赖
pip install numpy matplotlib torch gymnasium tensorboard
```

## 使用示例
```bash
# 运行值迭代算法
python scripts/chapter4/value_iteration.py

# 运行DQN算法
python scripts/chapter8/dqn.py
```

## 参考资料

- [赵世钰老师课程地址](https://www.bilibili.com/video/BV1sd4y167NS) 💌
- [强化学习的数学基础](https://github.com/MathFoundationRL/Book-Mathematical-Foundation-of-Reinforcement-Learning)

## 贡献指南

欢迎对本项目进行贡献！您可以通过以下方式参与：
1. 提交Issue报告bug或提出改进建议
2. 提交Pull Request修复bug或添加新功能
3. 完善文档和注释

## 致谢

感谢西湖大学赵世钰老师的精彩课程和原作者jwk1rose的开源贡献。

---

[🇨🇳 中文文档](#chinese) | [🇺🇸 English](#english)

<a id="english"></a>
# RL_Learning 🎉️

![Python](https://img.shields.io/badge/Python-3.7%2B-blue) ![Status](https://img.shields.io/badge/status-refactoring-orange) ![Algorithms](https://img.shields.io/badge/algorithms-basic%20RL-green)

> Original code source: https://github.com/jwk1rose/RL_Learning  
> I am refactoring the code, trying to divide it into more independent modules and adding detailed comments.

## Introduction 📖
This project implements the reinforcement learning course code from Professor Shiyu Zhao at Westlake University. It covers most of the code from chapters 1-9, including the construction of simulation environments, value iteration, policy iteration, Monte Carlo methods, temporal difference learning, state value approximation, DQN, Reinforce, and other algorithms. While striving for accurate reproduction, the author's coding skills are limited, so there may be bugs and inefficiencies. Please use it only as a reference.

I was very fortunate to discover this course, as it introduced me to reinforcement learning. Compared to many other resources available, both the course and textbook quality are top-notch. Professors like Prof. Zhao who are willing to invest so much effort in creating such high-quality videos are rare nowadays. I dedicate this open-source repository to honor Professor Zhao✋.

<div align="center">
  <img src="./scripts/Chapter4_Value iteration and Policy iteration/plot_figure/policy_iteration.png" width="45%" alt="Policy Iteration Visualization"/>
  <img src="./scripts/Chapter4_Value iteration and Policy iteration/plot_figure/value_iteration.png" width="45%" alt="Value Iteration Visualization"/>
  <p>From left to right: Policy Iteration, Value Iteration Visualization</p>
</div>

## Project Structure

```tree
RL_Learning-main/
├── scripts/            # Algorithm implementation scripts
│   ├── Chapter4_Value iteration and Policy iteration/  # Chapter 4: Value Iteration and Policy Iteration
│   ├── Chapter5_Monte Carlo Methods/                  # Chapter 5: Monte Carlo Methods
│   ├── Chapter6_Stochastic_approximation/            # Chapter 6: Stochastic Approximation
│   ├── Chapter7_Temporal-Difference learning/        # Chapter 7: Temporal Difference Learning
│   ├── Chapter8_Value Function Approximaton/         # Chapter 8: Value Function Approximation
│   ├── Chapter9_Policy Gradient/                     # Chapter 9: Policy Gradient
│   ├── Chapter10_Actor Critic/                       # Chapter 10: Actor-Critic Methods
│   ├── grid_env.py                                   # Grid environment
│   ├── model.py                                      # Neural network models
│   ├── render.py                                     # Rendering tools
│   └── solver.py                                     # Base solver class
└── README.md           # Project description
```

## Implemented Algorithms

| Algorithm | Status | Location | Description |
|------|------|------|------|
| Value Iteration | ✅ | `scripts/Chapter4_Value iteration and Policy iteration/` | Optimal value function solving based on dynamic programming |
| Policy Iteration | ✅ | `scripts/Chapter4_Value iteration and Policy iteration/` | Optimal policy solving based on dynamic programming |
| Monte Carlo Methods | ✅ | `scripts/Chapter5_Monte Carlo Methods/` | Value function estimation based on sampling |
| TD Learning | ✅ | `scripts/Chapter7_Temporal-Difference learning/` | Methods combining dynamic programming and Monte Carlo |
| Q-learning | ✅ | `scripts/Chapter7_Temporal-Difference learning/` | Classic off-policy reinforcement learning algorithm |
| n-step Sarsa | ✅ | `scripts/Chapter7_Temporal-Difference learning/` | Multi-step temporal difference learning |
| Value Approximation | ✅ | `scripts/Chapter8_Value Function Approximaton/` | Using function approximation instead of tabular representation |
| DQN (Deep Q-Network) | ✅ | `scripts/Chapter8_Value Function Approximaton/` | Deep Q-Network algorithm |
| Reinforce Algorithm | ✅ | `scripts/Chapter9_Policy Gradient/` | Basic policy gradient algorithm |
| Actor-Critic | ✅ | `scripts/Chapter10_Actor Critic/` | Methods combining policy gradient and value function approximation |

## Development Environment Notes
### Note for PyCharm Users
When opening this project with **PyCharm**, the `sys.path.append("..")` import statements in the code will work without errors. **PyCharm** automatically adds the project root directory to `PYTHONPATH`, ensuring that module imports work correctly.

If you are using another IDE or running scripts directly from the command line, you may need to manually set the `PYTHONPATH` environment variable:

```bash
export PYTHONPATH=$PYTHONPATH:/path/to/RL_Learning-main
```
or
```python
# sys.path.append("..") # Comment out this line
# Change to:
import os 
sys.path.append(os.path.join(os.path.dirname(os.path.dirname(os.path.abspath(__file__)))))
```

## Update Log

<a id="fix-issue-9-en"></a>
**2026.4.8**  
Fix [issue-9](https://github.com/Ronchy2000/Multi-agent-RL/issues/9): corrected two problems in the Monte Carlo epsilon-greedy control implementation in `scripts/Chapter5_Monte Carlo Methods/MC_epsilon_greedy.py`.  
- Return computation now uses the standard backward recursion `G = gamma * G + reward`, removing the misaligned slice based on `episode.index(step)` that caused incorrect and duplicated discounted returns.  
- Policy improvement now follows the textbook epsilon-soft update formula instead of collapsing to a deterministic greedy policy.  
- `np.divide(..., where=num_visits != 0)` now writes explicit zeros for unvisited state-action pairs, preventing uninitialized `qvalue` entries.  

<a id="fix-issue-8-en"></a>
**2026.2.19**  
Fix [issue-8](https://github.com/Ronchy2000/Multi-agent-RL/issues/8): `obtain_episode()` now stops early when `done=True`, with `length` used only as a maximum horizon.  
- Updated files: `scripts/solver.py`, `scripts/Chapter5_Monte Carlo Methods/MC_Basic.py`, `scripts/Chapter5_Monte Carlo Methods/MC_Exploring_Starts.py`, `scripts/Chapter5_Monte Carlo Methods/MC_epsilon_greedy.py`.  
- This avoids post-terminal sampling bias and better matches episodic-task return definitions.  

<a id="fix-issue-7-en"></a>
**2026.2.19**  
Fix [issue-7](https://github.com/Ronchy2000/Multi-agent-RL/issues/7): fixed an indentation bug in `scripts/Chapter5_Monte Carlo Methods/MC_Basic.py` (`mc_basic_simple` and `mc_basic_simple_GUI`).  
- `sum_qvalue_list.append(sum_qvalue)` is now inside the `for each_episode in episodes:` loop, so every sampled episode return is included.  
- `episodes = []` is now reinitialized for each `(state, action)` pair, so episode samples from different state-action pairs are no longer mixed into the same `Q(s,a)` estimate.  
- After the fix, `self.qvalue[state][action] = np.mean(sum_qvalue_list)` uses the full sampled set instead of only the last episode return.  

<a id="fix-issue-1-en"></a>
**2026.2.15**  
Fixes for Chapter 8 TD-Linear (linear function approximation):  
- Align `reward_list` in `scripts/Chapter8_Value Function Approximaton/1.TD-Linear.py` with `scripts/grid_env.py`'s `Rsa` reward-index convention by using `env.reward_list` (`[other, target, forbidden, overflow]`), so `policy_evaluation()` computes correct state values.  
- Make `scripts/grid_env.py` accept an optional `reward_list` argument so each algorithm script can configure its own reward scheme via `GridEnv(..., reward_list=[...])`.  
- Fix missing $\phi(s_t)$ in the TD(0) weight update. The weight update formula is $w \leftarrow w + \alpha \delta_t \phi(s_t)$, where $\delta_t = r + \gamma \phi(s_{t+1})^\top w - \phi(s_t)^\top w$.

**2024.6.7**  
Major update! The original author's render coordinates were inconsistent with the state settings. The coordinates have been unified as:  
![img.png](../img.png)

## Environment Setup

```bash
# Create virtual environment
conda create -n rl_learning python=3.7
conda activate rl_learning
# Install dependencies
pip install numpy matplotlib torch gymnasium tensorboard

```


## Usage Examples
```bash
# Run value iteration algorithm
python scripts/Chapter4_Value\ iteration\ and\ Policy\ iteration/value_iteration.py

# Run Q-learning algorithm
python scripts/Chapter7_Temporal-Difference\ learning/3.Q-learning.py

# Run DQN algorithm
python scripts/Chapter8_Value\ Function\ Approximaton/DQN.py
```


## References

- [Professor Zhao's Course](https://www.bilibili.com/video/BV1sd4y167NS) 💌
- [Mathematical Foundation of Reinforcement Learning](https://github.com/MathFoundationRL/Book-Mathematical-Foundation-of-Reinforcement-Learning)

## Contribution Guidelines

Contributions to this project are welcome! You can participate in the following ways:
1. Submit issues to report bugs or suggest improvements
2. Submit pull requests to fix bugs or add new features
3. Improve documentation and comments

## Acknowledgements

Thanks to `Professor Shiyu Zhao` from Westlake University for his excellent course and the original author `jwk1rose` for the open-source contribution.
