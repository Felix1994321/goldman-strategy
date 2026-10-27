# 金本位元帅历史策略审计

> 微博：金本位元帅 策略群组发言策略回溯分析
> 自 2025 年 10 月 1 日起的历史策略发言、市场行情与后续表现审计。

**在线网站：** https://felix1994321.github.io/goldman-strategy/  
**项目仓库：** https://github.com/Felix1994321/goldman-strategy

## 一、项目介绍

金本位元帅 策略群组发言策略回溯分析，统计其策略胜率、策略表现等内容，压缩包源码支持本地化拉取数据。针对明确买入的可特别筛选。脚本主要拉取从策略发出后1小时、4小时等多个级别走势结果，以K线图呈现。

### 主要功能

- **数据总览**：展示策略总数、有效行情记录数、不同时间窗口的上涨/下跌数量与占比、涨跌幅统计、异常下跌案例等。

- **明确买入信号审计**：可在「全部策略」与「仅明确买入／已买」间切换，查看该类信号发出后 1 小时、4 小时、24 小时、48 小时和一周的表现。

- **交互式 K 线**：查看发言前后的市场走势，支持切换 K 线周期与观察窗口；图中标注发言时间及当时价格。

- **策略详情与完整数据表**：按发布时间查看原文、标的、目标价（如有）、各固定时间点的价格与涨跌幅，并逐条查看 K 线；支持搜索、筛选与时间排序。

  

---

## 二、源码本地运行、数据拉取与部署

以下命令以 **Windows PowerShell** 为例。需要 Python 3.10+、Git，以及能够访问币安历史行情接口的网络环境。

### 1. 准备本地源码

下载并解压项目源码，用 VS Code 打开源码目录。确认至少包含以下文件：

```text
source/
├── build.py                 # 数据采集与静态网站构建
├── index.html               # 网站首页
├── comments.html            # 留言广场
├── comments-config.json     # 留言板配置
├── mapping.json             # 币种与合约映射
├── strategy_metadata.csv    # 策略分类与目标价等元数据
├── requirements.txt         # Python 依赖
└── 历史策略.xlsx              # 自行放入的输入数据（示例文件名）
```

输入 Excel 至少应有 **序号、发言时间、发言内容、标的** 四列；如包含发言时价格和后续时点的行情快照，脚本也会利用这些数据。原始 Excel、个人信息及凭据不应直接发布到公共仓库。

### 2. 安装依赖

在 VS Code 终端或 PowerShell 中切换到源码目录：

```powershell
cd "D:\StrategyAudit\source"  # 改成自己的实际源码路径
python -m pip install -r requirements.txt
```

### 3. 拉取行情并重新 Build

```powershell
python build.py ".\weibo_group_messages (since20251001)_binance_backtest.xlsx" --out site
```

构建时脚本会请求历史行情，并把网页和数据生成到 `site/`；行情下载缓存会保存在本地 `cache/`。

### 4. 本地预览

```powershell
cd site
python -m http.server 8000
```

浏览器打开 http://localhost:8000 。
