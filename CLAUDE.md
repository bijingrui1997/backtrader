# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## 项目概述

Backtrader 是一个用 Python 编写的回测和实时交易平台。这是一个成熟的量化交易框架，支持多种数据源、策略开发、技术指标、经纪商集成和可视化。

## 核心架构

### 主要组件结构
- **cerebro.py**: 核心引擎，协调所有组件运行
- **strategy.py**: 策略基类，用户策略继承此类
- **broker.py**: 经纪商模拟器，处理订单执行和资金管理
- **feed.py**: 数据源基类，支持多种数据格式
- **indicator.py**: 技术指标基类和指标系统
- **analyzer.py**: 分析器，用于计算策略性能指标

### 模块组织
- `feeds/`: 数据源实现（CSV、Yahoo Finance、Interactive Brokers等）
- `indicators/`: 内置技术指标库（122个指标）
- `strategies/`: 示例策略实现
- `analyzers/`: 性能分析工具
- `brokers/`: 经纪商接口实现
- `observers/`: 观察者模式组件用于数据记录

## 常用命令

### 运行示例
```bash
# 运行基本示例策略
python samples/sigsmacross/sigsmacross.py

# 使用 btrun 命令行工具
btrun --help
python tools/bt-run.py --help
```

### 测试
```bash
# 运行所有测试
python -m unittest discover -s tests -p "test_*.py"

# 运行单个测试文件
python -m unittest tests.test_ind_sma

# 运行特定测试
python -m unittest tests.test_ind_sma.TestSMA.test_sma
```

### 开发构建
```bash
# 安装开发依赖
pip install -e .
pip install -e .[plotting]  # 包含绘图功能

# 构建发布包
python setup.py bdist_wheel --universal

# 发布到 PyPI（维护者使用）
./pypi.sh
```

## 开发指南

### 策略开发模式
1. 继承 `bt.Strategy` 或 `bt.SignalStrategy`
2. 实现 `__init__()` 方法定义指标和参数
3. 实现 `next()` 方法定义交易逻辑
4. 使用 `cerebro.addstrategy()` 添加到引擎

### 技术指标开发
- 继承 `bt.Indicator` 基类
- 定义 `lines` 元组指定输出线
- 实现 `__init__()` 和计算逻辑
- 放置在 `indicators/` 目录下

### 数据源开发  
- 继承 `bt.feeds.DataBase` 或具体数据源类
- 实现 `_load()` 方法读取数据
- 支持 OHLCV 格式和自定义字段

## 测试架构

使用标准 `unittest` 框架：
- 测试文件位于 `tests/` 目录
- 命名模式：`test_*.py`
- 使用 `testcommon.py` 提供通用测试工具
- 主要测试类型：指标测试、策略测试、数据处理测试

## 依赖管理

### 核心依赖
- Python >= 3.2（支持 pypy）
- 无强制外部依赖

### 可选依赖  
- matplotlib: 绘图功能
- pandas: 数据处理
- IbPy: Interactive Brokers 集成
- oandapy: Oanda 集成
- ta-lib: TA-Lib 指标支持

## 项目特色

- 支持多时间框架同时运行
- 内置经纪商模拟器支持多种订单类型
- 丰富的技术指标库（122个内置指标）
- 支持实时交易和回测
- 灵活的佣金和滑点模型
- 完整的性能分析和可视化系统