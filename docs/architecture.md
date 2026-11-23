# 总体架构

## 分层模型

```text
平台适配层
  Linux / Fcitx5
  Windows / TSF
  macOS / InputMethodKit
          |
          v
输入交互运行时
  输入会话、组合态、上下文、候选、提交、撤销
          |
          v
推理编排层
  设备错误模型、模型路由、取消、超时、结果校验
          |
          v
模型 Provider
  本地 LLM / 远程 LLM / 传统模型 / 规则引擎
```

## 主要模块

### Platform Adapter

把操作系统输入法框架转换成 52Hz 的统一输入事件，并负责显示候选、提交文本和处理平台生命周期。

平台适配层不应该包含语言理解逻辑。

### Interaction Runtime

维护当前输入会话：

- 原始按键和字符流
- 当前组合文本
- 光标位置
- 已提交文本
- 当前应用上下文
- 用户确认和撤销状态

它是输入法交互的状态机，不直接绑定某一个 LLM。

### Inference Orchestrator

负责把一次输入请求路由到合适的模型或算法：

- 判断是否需要调用模型
- 选择本地或远程 Provider
- 取消已经过期的请求
- 控制超时和资源预算
- 合并多个候选结果
- 验证模型输出格式
- 根据置信度决定提交、展示或等待

### Device and Error Model

描述当前设备的输入观测规律，包括键盘布局、触摸区域和用户特定错误模式。

### Model Provider

所有模型能力都通过统一接口接入。模型可以是：

- 本地小模型
- 本地量化 LLM
- 云端 OpenAI-compatible API
- 专用拼音和纠错模型
- 传统规则和词典引擎

不同 Provider 的结果需要转换成统一的结构化响应。

## 核心数据对象

初步需要定义以下对象：

```text
DeviceProfile
InputEvent
CompositionState
ContextSnapshot
InferenceRequest
InferenceResponse
Candidate
CommitAction
```

这些对象后续应在公共协议中版本化，但当前阶段先在文档中保持概念稳定，避免过早锁定序列化格式。

## 延迟原则

输入法的基础交互必须保持低延迟：

- 按键事件处理不等待远程服务
- 新输入到达时可以取消过期推理
- 云端调用失败时可以降级到本地能力
- 高延迟模型用于整句增强，而不是阻塞基础输入

## 数据流

```text
Keyboard Event
      |
      v
Platform Adapter
      |
      v
Composition State
      |
      +--> Device Error Model
      |
      +--> Context Snapshot
      |
      v
Inference Orchestrator
      |
      v
Validated Candidates / Actions
      |
      v
Commit, Display, or Wait
```
