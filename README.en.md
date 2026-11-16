<div align="center">

<img src="assets/icon.svg" alt="Liuzi Kitchen icon" width="144" />

# 留子厨房 · Liuzi Kitchen

<a id="english"></a>
**Start with your pantry, budget, and equipment, then decide what to cook.**

A bilingual Codex skill for planning and cooking student meals.

**English** · [简体中文](README.md)

[![Codex Skill](https://img.shields.io/badge/Agent-Codex_Skill-2563eb)](SKILL.md)
[![Python tools](https://img.shields.io/badge/3_个工具-Python_标准库-3776AB)](#三个可以独立运行的小工具)

[Start here](#三步开始) · [Sample output](#一分钟看懂采购量助手) · [Six modes](#六种模式一套厨房) · [Standalone tools](#三个可以独立运行的小工具) · [Safety boundaries](#安全价格和信息边界)

</div>

One pan, a rice cooker, a shared fridge, and half a cabbage you would rather not waste. Liuzi Kitchen combines a **Codex Skill and three Python helpers** to work from servings, pantry contents, allergies, time, and budget. It helps plan shopping and cooking, then adjust when conditions change.

> Choose a meal → estimate quantities → check safety and restrictions → adjust while cooking → reuse leftovers safely

<a id="一分钟看懂采购量助手"></a>
## A grocery-planning example

**Inputs: 1 person, 3 meals, tofu + rice + vegetables.** These quantities come from running the repository script. They are not live supermarket quotes or an individual nutrition prescription.

| Item | Estimated quantity |
| --- | ---: |
| Tofu | 450 g |
| Rice | 225 g dry rice, or an equivalent staple portion |
| Fresh + frozen vegetables | 750 g |
| Sauce / seasoning | About 4.5 tbsp |
| Cooking oil | About 2.2 tbsp |

```bash
python scripts/grocery_planner.py \
  --servings 1 --meals 3 --protein tofu --lang zh-en
```

The script also suggests meal directions, storage and reheating reminders, health-adaptation notes, and ways to reduce waste. **It calculates from fixed portion templates; it does not query pantry stock or prices, or automatically assess your allergies and health needs.** Adjust personal restrictions before using the plan for shopping.

<a id="三步开始"></a>
## Start in three steps

<a id="1-安装到-codex"></a>
### 1. Install in Codex

Clone into your local Skills directory:

```bash
git clone https://github.com/yuzhang-zhong/liuzi-kitchen.git ~/.codex/skills/liuzi-kitchen
```

If that directory exists, update the existing installation instead of cloning over it. Reload Codex Skills afterward. A compatible Skills installer can also install [`SKILL.md`](SKILL.md) from the repository root.

To try only the three helper scripts, use a regular clone:

```bash
git clone https://github.com/yuzhang-zhong/liuzi-kitchen.git
cd liuzi-kitchen
python scripts/grocery_planner.py --help
```

The scripts need Python 3, use only its standard library, and require no API key. Full conversation and web research depend on the host agent's capabilities.

<a id="2-用你真实的条件开场"></a>
### 2. Describe your actual conditions

```text
Use $liuzi-kitchen. I cook for one person and have a rice cooker and a frying pan.
My fridge has tofu, eggs, cabbage, and rice. Ask about allergies and dietary restrictions
first, then plan tonight's dinner and tomorrow's lunch. Keep each cooking session
under 25 minutes and answer in Chinese and English.
```

For a weekly plan, add your city, accessible shops, budget, freezer space, and number of people per meal. Without that context, a seemingly cheaper bulk purchase may become waste.

<a id="3-情况变了直接继续"></a>
### 3. Continue when something changes

```text
I'm halfway through cooking. The food is too salty and there is a lot of water
in the pan. Help me check whether it is cooked through before suggesting a fix.
```

```text
Can I still use yesterday's rice? Ask about storage time and temperature first;
do not jump straight to a fried-rice recipe.
```

<a id="六种模式一套厨房"></a>
## Six cooking modes

| Mode | When it helps | What to expect |
| --- | --- | --- |
| **Recommend** | You do not know what to eat | 3–7 options based on budget, taste, and equipment |
| **Plan** | Daily meals or weekly prep | Meals, prep order, storage, reheating, and ingredient reuse |
| **Shop** | You do not know how much to buy | Quantities, pack-size adjustments, store sections, and substitutions |
| **Scout** | Guides contradict each other | Source comparisons distinguishing official information from personal experience |
| **Cook-Live** | Too salty, too watery, or missing ingredients | Safety checks followed by adjustments for the current cooking step |
| **Reuse** | Leftovers or unused ingredients | Storage checks before considering bowls, noodle soups, or wraps |

The reference library covers Chinese, Japanese/Korean, Southeast Asian, South Asian, Mediterranean, Middle Eastern/North African, Latin American, African/Caribbean, American, and European home-cooking directions. Suggestions are simplified around local ingredients and equipment, not fixed to one menu.

<a id="三个可以独立运行的小工具"></a>
## Three standalone helpers

| Tool | What it does | What it does not do |
| --- | --- | --- |
| [`grocery_planner.py`](scripts/grocery_planner.py) | Calculates quantities from servings, meals, and templates | Does not guarantee pack sizes, nutritional adequacy, or personal suitability |
| [`meal_check.py`](scripts/meal_check.py) | Produces rule-based PASS / WARN / FAIL notes from input | Is not an allergy test, food inspection, or medical diagnosis |
| [`value_compare.py`](scripts/value_compare.py) | Compares supplied cost, time, risk, and reuse factors | Does not fetch live prices; its score is not an objective market ranking |

<details>
<summary>Safety-check and value-comparison commands</summary>

The allergy-check example deliberately includes sesame in **fictional test input**. It is not a meal recommendation:

```bash
python scripts/meal_check.py \
  --ingredients "chicken,rice,broccoli,soy sauce,sesame oil" \
  --allergies sesame --protein chicken --veg yes --starch rice \
  --risk none --goal balanced --sodium moderate --lang zh-en
```

The following prices are hypothetical, not store quotes. Fields are name, total cost, servings, minutes, reuse score (1–5), risk score (0–5), and source count:

```bash
python scripts/value_compare.py \
  --option "Tofu bowls|9.50|4|35|5|1|3" \
  --option "Takeout lunch|13.00|1|10|1|1|1" --lang zh-en
```

Use each script's `--help` for complete options.

</details>

<a id="安全价格和信息边界"></a>
## Safety, price, and information boundaries

- Ask about restrictions first; do not promise absolute safety. Allergies, storage, cross-contamination, doneness, and reheating need specific checks. Scripts do not replace a thermometer or on-site judgment.
- This is not medical nutrition therapy. Pregnancy, immunocompromise, kidney disease, diabetes, severe allergies, a history of eating disorders, or medication interactions require a clinician or registered dietitian.
- Community guides are leads, not guarantees. Compare date, city, unit price, membership conditions, travel, equipment, and freezer space. If a source such as RedNote or Zhihu cannot be accessed, state that it was not read.
- Full planning depends on the host agent. This repository is not a standalone app, online supermarket, or automatic ordering service. The three local scripts do not conduct community web research.

<a id="按需要阅读"></a>
## Further reading

- [Core Skill workflow](SKILL.md)
- [Food safety and official sources](references/food-safety.md) · [Health and allergy checks](references/health-checks.md)
- [Live adjustments, inventory, and leftovers](references/dynamic-cooking.md) · [Flavor and budget](references/flavor-budget.md)
- [Global cuisines](references/global-cuisines.md) · [Basic meal templates](references/meal-catalog.md)
- [Community evidence and source comparison](references/social-intel.md)
