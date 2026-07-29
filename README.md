# laughing-octo-goggles"""
东野圭吾逝世事件 网络舆论周期推演模型
Public Opinion Forecast: Keigo Higashino Passing Event
适用于舆情演变三阶段仿真，自动输出Markdown分析报告
"""
import matplotlib.pyplot as plt
import numpy as np

# ===================== 事件基础配置 =====================
event_info = {
    "event_name": "东野圭吾逝世",
    "announce_date": "2026-07-27",
    "source": "日本讲谈社官方讣告",
    "stage_short": "0~7天 短期爆发",
    "stage_mid": "7~30天 中期分化讨论",
    "stage_long": "1~12个月 长期长尾舆情"
}

# 热度模拟数据
days = np.arange(0, 90)
# 热度曲线：快速冲高→缓慢回落→长期低热度长尾
heat_curve = np.zeros_like(days, dtype=float)
peak_day = 3

for i, d in enumerate(days):
    if d <= peak_day:
        heat_curve[i] = 95 * np.exp(-0.12 * (peak_day - d))
    elif peak_day < d <= 30:
        heat_curve[i] = 82 * np.exp(-0.06 * (d - peak_day))
    else:
        heat_curve[i] = 7 * np.exp(-0.011 * (d - 30))

# ===================== 舆情文本推演逻辑 =====================
def generate_opinion_analysis():
    analysis_text = f"""# {event_info['event_name']} 舆论推演报告
> 事件官宣时间：{event_info['announce_date']}
> 信息来源：{event_info['source']}

## 一、短期舆情【{event_info['stage_short']}】
主流情绪：惋惜、集体青春怀旧
1. 《解忧杂货店》《白夜行》语录大规模传播；社交媒体悼念刷屏
2. 图书订单暴涨，遗作受到全网关注
3. 舆论基调统一温和，负面争议较少

## 二、中期舆情【{event_info['stage_mid']}】
1. 读书博主批量产出作品盘点、人物深度解析内容
2. 温和观点分化：
✅正方：东亚通俗推理文学标志性人物
⚠️理性质疑：部分作品商业化严重、剧情套路化
3. “死后封神”现象，弱化生前流水线写作争议

## 三、长期长尾舆情【{event_info['stage_long']}】
1. 每年诞辰、忌日周期性产生怀旧流量
2. 影视剪辑、小说解说持续稳定产出二创内容
3. 二手绝版书籍市场价格出现波动

## 潜在衍生争议话题
1. 讣告延迟4天才对外公开，引发隐私与公众知情权讨论
2. 后续手稿版权、影视改编权益持续受到关注
3. 收藏群体炒作实体书的讨论
"""
    return analysis_text

# ===================== 保存报告+绘图 =====================
def save_report(content):
    with open("forecast_report.md", "w", encoding="utf-8") as f:
        f.write(content)
    print("✅ 舆情报告已生成：forecast_report.md")

def draw_heat_chart():
    plt.rcParams["font.sans-serif"] = ["SimHei"]
    plt.rcParams["axes.unicode_minus"] = False
    plt.figure(figsize=(10,5))
    plt.plot(days, heat_curve, color="#4477bb", lw=2.5)
    plt.title(f"{event_info['event_name']}全网热度变化模拟曲线")
    plt.xlabel("官宣后天数")
    plt.ylabel("网络话题热度指数")
    plt.grid(alpha=0.3)
    plt.axvline(x=7, label="短期/中期分界", ls="--", c="orange")
    plt.axvline(x=30, label="中期/长尾分界", ls="--", c="red")
    plt.legend()
    plt.savefig("heat_trend.png", dpi=150)
    print("✅ 热度趋势图已保存：heat_trend.png")

if __name__ == "__main__":
    report = generate_opinion_analysis()
    save_report(report)
    draw_heat_chart()
