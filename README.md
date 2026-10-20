<p align="center"><img src="assets/icon.svg" alt="留子厨房图标" width="144" /></p>
<h1 align="center">留子厨房 · Liuzi Kitchen</h1>
<p align="center"><strong>先看冰箱、预算和厨具，再决定今天吃什么。</strong><br>A bilingual kitchen skill for the reality of student cooking.</p>
<p align="center"><a href="#三步开始">开始使用</a> · <a href="#一分钟看懂采购量助手">实际输出</a> · <a href="#六种模式一套厨房">六种模式</a> · <a href="#english">English</a></p>

一口锅、一个电饭煲、共用冰箱，和不想再浪费的半颗白菜。留子厨房不是静态菜谱合集，而是一个 **Codex Skill + 三个 Python 小工具**：从人数、库存、过敏限制、时间与预算出发，把做饭拆成能采购、能执行、能调整的下一步。

> 想好吃什么 → 算清买多少 → 检查安全与限制 → 做饭中调整 → 安全地复用剩菜

## 一分钟看懂采购量助手

**示例条件：1 人、3 餐、豆腐 + 米饭 + 蔬菜。** 下表来自仓库脚本实际运行，不是实时超市报价，也不是个人营养处方。

| 采购项 | 脚本估算量 |
| --- | ---: |
| 豆腐 | 450 g |
| 米 | 225 g 干重或等量主食 |
| 新鲜 + 冷冻蔬菜 | 750 g |
| 酱料 / 调味 | 约 4.5 tbsp |
| 食用油 | 约 2.2 tbsp |

```bash
python scripts/grocery_planner.py \
  --servings 1 --meals 3 --protein tofu --lang zh-en
```

脚本还输出菜品方向、储存与复热提醒、健康适配提示和减少浪费的建议。**它根据固定份量模板做数量计算，不查询库存或价格，也不自动判断你的过敏与健康条件。** 请先调整个人限制，再用于实际采购。

## 三步开始

### 1. 安装到 Codex

将仓库放入本地 Skills 目录：

```bash
git clone https://github.com/yuzhang-zhong/liuzi-kitchen.git ~/.codex/skills/liuzi-kitchen
```

若该目录已存在，请在原安装中更新，不要重复克隆。安装后重新加载 Codex 的 Skills。也可使用兼容的 Skills 安装器，从本仓库根目录安装 [`SKILL.md`](SKILL.md)。

仅试用三个辅助脚本时，普通克隆即可：

```bash
git clone https://github.com/yuzhang-zhong/liuzi-kitchen.git
cd liuzi-kitchen
python scripts/grocery_planner.py --help
```

需要 Python 3；三个脚本只使用标准库，无需 API key。完整对话与网页研究需要宿主 Agent 自己具备相应能力。

### 2. 用你真实的条件开场

```text
使用 $liuzi-kitchen。我一个人，有电饭煲和一个平底锅，冰箱里有豆腐、鸡蛋、卷心菜和米。
请先问清过敏和饮食限制，再安排今晚与明天午饭；每次做饭不超过 25 分钟，中英双语。
```

想规划一周时，再补充城市、可到达的商店、预算、冷冻空间和每餐人数。没有这些条件，便宜的“大包装方案”也可能变成浪费。

### 3. 情况变了，直接继续

```text
炒到一半发现太咸，而且锅里水很多。先帮我判断是否熟透，再说下一步怎么救。
```

```text
昨天剩下的米饭还能不能用？先问我储存时间和温度，不要直接给炒饭菜谱。
```

## 六种模式，一套厨房

| 模式 | 适合什么时候 | 希望拿到什么 |
| --- | --- | --- |
| **Recommend · 推荐** | 不知道吃什么 | 结合预算、口味、厨具的 3–7 个选择 |
| **Plan · 计划** | 一日餐或一周备餐 | 菜单、备餐顺序、储存复热与食材复用 |
| **Shop · 买菜** | 不知道买多少 | 采购量、包规调整、店铺分区与替换方案 |
| **Scout · 攻略侦察** | 攻略互相矛盾 | 区分官方信息与个人经验的来源对比 |
| **Cook-Live · 救场** | 太咸、水多、缺材料 | 先检查安全，再给当前这一步的调整 |
| **Reuse · 再创造** | 有剩菜与边角料 | 先查储存条件，再考虑饭碗、汤面或卷饼 |

菜系不是一道固定菜单：参考库覆盖中式、日韩、东南亚、南亚、地中海、中东与北非、拉美、非洲与加勒比、美式及欧洲家常方向，并根据本地食材与工具简化。

## 三个可以独立运行的小工具

| 工具 | 做什么 | 不做什么 |
| --- | --- | --- |
| [`grocery_planner.py`](scripts/grocery_planner.py) | 按份数、餐数与模板计算采购量 | 不保证实际包规、营养充足或个人适配 |
| [`meal_check.py`](scripts/meal_check.py) | 根据输入输出规则式 PASS / WARN / FAIL 提示 | 不是过敏检测、食品检验或医疗诊断 |
| [`value_compare.py`](scripts/value_compare.py) | 对比用户给定的成本、时间、风险和复用因素 | 不提供实时价格；评分不是客观市场排名 |

<details>
<summary>展开查看安全检查与性价比演示命令</summary>

以下过敏检查使用**虚构测试输入**，刻意包含芝麻，不是食用建议：

```bash
python scripts/meal_check.py \
  --ingredients "chicken,rice,broccoli,soy sauce,sesame oil" \
  --allergies sesame --protein chicken --veg yes --starch rice \
  --risk none --goal balanced --sodium moderate --lang zh-en
```

性价比演示中的价格是**假设值**，不是商店报价。字段依次为名称、总成本、份数、分钟数、复用分（1–5）、风险分（0–5）、来源数量：

```bash
python scripts/value_compare.py \
  --option "Tofu bowls|9.50|4|35|5|1|3" \
  --option "Takeout lunch|13.00|1|10|1|1|1" --lang zh-en
```

完整选项请使用各脚本的 `--help`。
</details>

## 安全、价格和信息边界

- **先问限制，不承诺绝对安全。** 过敏、储存条件、交叉污染、熟度和复热都需要具体检查；脚本不能替代温度计或现场判断。
- **不是医疗营养治疗。** 孕期、免疫低下、肾病、糖尿病、严重过敏、进食障碍史或药物相互作用等情况，需要医生或注册营养师指导。
- **攻略是线索，不是事实保证。** 对比信息的日期、城市、单位价格、会员条件、车程、厨具与冷冻空间。无法访问小红书、知乎或其他来源时，应说明未读到，不假装核验。
- **完整规划依赖宿主能力。** 本仓库不是独立 App、在线商超或自动下单服务；社区检索不会由这三个本地计算脚本完成。

## 按需要阅读

- [Skill 核心流程](SKILL.md)
- [食品安全与官方来源](references/food-safety.md) · [健康与过敏检查](references/health-checks.md)
- [动态救场、库存和剩菜](references/dynamic-cooking.md) · [调味与预算](references/flavor-budget.md)
- [全球菜系](references/global-cuisines.md) · [基础菜品模板](references/meal-catalog.md)
- [社区攻略证据与横向比较](references/social-intel.md)

## English

Liuzi Kitchen turns pantry contents, equipment, budget, dietary restrictions, and time into practical next steps. Its six modes cover recommendations, meal planning, shopping quantities, source comparison, live cooking rescue, and safe leftover reuse.

Install the repository as a Codex Skill, or run the three standard-library Python tools independently. The reproducible example above calculates a three-serving grocery plan from fixed templates. Quantities and comparison scores are planning aids, not live prices, nutrition prescriptions, or safety guarantees. Web research and conversational reasoning come from the host Agent, not an embedded service. Medical or high-risk dietary needs require qualified professional guidance.
