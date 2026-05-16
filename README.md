# GitHub Trending 动量因子分析

抢先发现 GitHub 上正在爆发的 AI 项目，用量化思维做内容运营。

## 核心思路

把 GitHub star 当股价，每小时快照当 K 线，用动量因子捕捉**正在暴力增长**的项目。

**四因子模型：**

| 因子 | 公式 | 捕捉什么 |
|------|------|----------|
| 价格动量 | `total_stars / MA(n) - 1` | star 总量偏离均线 → 正在被大量关注 |
| 增速动量 | `delta / MA(delta, n) - 1` | 每小时新增的加速度 → 爆发中 |
| 量能突破 | `today_stars / MA(today, n) - 1` | today_stars 突然飙升 → 短期热度爆棚 |
| 新鲜度 | 首次上榜时间 + 建仓时间 | 刚被发现的项目 → 信息差最大 |

新鲜度权重最高（0.40），确保**刚上榜的项目优先推送**，抢在别人前面出视频。
<div align="center">
注册一键对接
【aiyiwei.vip】开发者和AI爱好者调用中转：100ms响应，1元开票，30+工具零改造
<p>claude code推荐特价 Claude Code分组。 gpt推荐codex专属分组和纯az分组。gemini使用gemini-cli分组 新增claude code4.7</p>
<p>限时特价分组，1毛到2毛每百万token gpt-5-nano-2025-08-07 专属养虾</p>
<p>看过来 👉https://aiyiwei.vip/register?aff=9RDC（尾部带个人邀请码，介意可删除尾部字母）</p>
<p>-官网 1-2折，534个全球模型统一管控。</p>
<p>-0.5元到0.7元人民币每刀</p>
<p>-最低1元起充，按需使用无现金流压力</p>
<p>-💼 财务合规无忧</p>
<p>-每笔充值均可开电子发票，最低 1元 起开</p>
<p>-注册就送 $0.2，每天签到领 $0.2-$1</p>
<p>-告别代充灰色渠道，审计直接过	</p>
<p>-🛠️ 30+企业工具一键接入，现有系统零改造</p>
<p>-Claude Code/Cline/Cursor企业部署 → 文档已备</p>
<p>常用龙虾文档:https://migxy8em66.apifox.cn/doc-8196816</p>
<p>-Claude Code → https://migxy8em66.apifox.cn/doc-8196820</p>
<p>-Cursor → https://migxy8em66.apifox.cn/doc-8196829</p>
<p>-Cline → https://migxy8em66.apifox.cn/doc-8196827</p>
<p>-等30多个代码和开发工具适配文档已备齐</p>
<p>-一个接口自动适配，标准OpenAI格式，现有代码改个base_url直接跑，1小时完成接入</p>
<p>-5分钟配通工具，满意再规模化——让AI基础设施像水电一样即开即用</p>
<p>-推广有邀请奖励：推广奖励支持支付宝提现</p>

</div>
## 快速开始

```bash
# 安装依赖
pip install -r requirements.txt

# 复制配置文件并填入你的信息
cp config.yaml.example config.yaml

# 手动抓取一次 + 分析
python main.py --now

# 只看动量分析（不抓取）
python main.py --momentum

# 看所有项目（不限 AI）
python main.py --momentum --all

# 后台常驻：每小时自动抓取 + 分析 + 推送
python main.py
```

## 输出示例

```
 1. microsoft/hve-core  [强势爆发] [加速中] [刚上榜]
    A refined collection of Hypervelocity Engineering components
    ⭐ 458 (+275 today) | PowerShell
    📊 动量因子:
       价格动量 = +0.33%  (star 偏离均线)
       增速动量 = +300.00%  (增量加速度)
       量能突破 = +0.00%  (today_stars 偏离)
       新鲜度   = 1.00    (首次上榜: 2小时前 建仓123天)
    🎯 综合得分 = 1.0007
```

## 配置说明

编辑 `config.yaml`：

```yaml
# GitHub Token（推荐配置，避免 API 限流）
github_token: "github_pat_xxx"

# 企业微信机器人 Webhook（可选）
wechat_webhook: "https://qyapi.weixin.qq.com/cgi-bin/webhook/send?key=xxx"

# 四因子权重（可调优）
momentum:
  w_momentum: 0.20
  w_delta: 0.20
  w_volume: 0.20
  w_freshness: 0.40   # 新鲜度最高，抢信息差
  ma_window: 6         # 均线窗口（6 = 近 6 小时）
  top_n: 10
  ai_only: true        # 只看 AI 项目
```

## 项目结构

```
├── main.py                  # 入口：调度器 + CLI
├── scraper/
│   ├── trending.py          # 抓取 GitHub Trending 页面
│   └── repo_detail.py       # GitHub API 获取仓库详情
├── storage/
│   ├── database.py          # SQLite 快照存储
│   └── models.py            # 数据模型
├── analyzer/
│   ├── momentum.py          # 动量因子引擎（核心）
│   └── trend.py             # 基础趋势分析
├── notifier/
│   └── wechat.py            # 企业微信通知
├── config.yaml.example      # 配置模板
└── requirements.txt
```

## 数据积累

系统每小时抓取一次快照，数据越多因子越准：

- **第 1 天**：所有项目新鲜度都是 1.0，主要靠增速动量区分
- **第 2-3 天**：新鲜度开始分层，新上榜 vs 老面孔拉开差距
- **第 7 天+**：均线窗口填满，四个因子协同工作，排名非常稳定

建议用 `crontab` 或 `screen/tmux` 保持 `python main.py` 常驻运行。
