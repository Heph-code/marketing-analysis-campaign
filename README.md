# Marketing Campaign Analysis

**English** | [Русский](README.ru.md)

Analysis of a one-month marketing campaign (January 2018): channel performance, the effect of ad personalization (A/B test), and the investigation of an unexpected drop in House Ads conversion.

## Table of Contents
- [Business Questions](#business-questions)
- [Key Findings](#key-findings)
- [Visuals](#visuals)
- [Dataset](#dataset)
- [Methodology](#methodology)
- [Repository Structure](#repository-structure)
- [Getting Started](#getting-started)
- [Limitations](#limitations)

## Business Questions
1. How did the campaign perform across marketing channels, age groups and test groups (control vs. personalization)?
2. Why did the **House Ads** conversion rate drop, and how many subscribers were lost?
3. Does personalization significantly improve conversion?

## Key Findings

| # | Finding | Evidence |
|---|---------|----------|
| 1 | **Personalization works** | 705 conversions vs. 371 in control on a similar ad volume (4,946 vs. 5,076 ads). Gains on Email, Facebook, Instagram and Push; none on House Ads. |
| 2 | **Ad allocation is inefficient** | House Ads receive ~47% of all ads but convert at 6.3%; Email converts at 34.2%. |
| 3 | **Younger users convert best** | Users up to 30 convert far above older groups on every channel; 19-24 is the strongest segment. |
| 4 | **A localization bug hurt House Ads** | From 11 Jan the share of ads shown in the user's preferred language fell from ~100% to 80-90%, and conversion fell from ~8-14% to ~3-4%. |
| 5 | **Estimated loss from the bug** | ~280 subscribers (578 expected vs. 298 actual). Upper-bound estimate: baseline rates for Arabic and German rest on very small samples. |
| 6 | **Email A/B test is significant** | 28.1% to 39.1% conversion (lift 39%, p = 0.006). |
| 7 | **Instagram personalization is a big win for under-30s** | Overall lift 380% (p < 0.001); significant only for age groups 0-30. |
| 8 | **Retention** | About 60% of subscribers are retained (rounded to the nearest 10%). |

### Recommendations
- Fix the House Ads localization bug and monitor the match between displayed and preferred language.
- Roll out personalization to all channels, prioritizing users under 30.
- Re-allocate budget from House Ads towards higher-converting channels.
- Test different personalization messaging for users aged 30+, where no significant effect was found.
- Run formal significance tests for Facebook and Push (differences are visible but not tested).

## Visuals

**Conversion rate by age group and channel**
![Conversion rate by age group](images/conversion_by_age_group.png)

**Control vs. personalization conversion rate**
![Conversion rate by variant](images/conversion_by_variant.png)

**House Ads conversion rate over time (drop from 11 Jan)**
![House Ads conversion drop](images/house_ads_conversion_drop.png)

**Daily ads served by channel (email blast on 15 Jan)**
![Daily ads trend](images/daily_ads_trend.png)

## Dataset
The notebook expects a file named `marketing.csv` in the repository root (the file is **not included** in this repository). It contains 10,037 records, one per ad served.

| Column | Description |
|--------|-------------|
| `user_id` | Unique user identifier |
| `date_served` | Date the ad was shown |
| `marketing_channel` | Channel: House Ads, Instagram, Facebook, Push, Email |
| `variant` | Test group: `control` or `personalization` |
| `converted` | Whether the user subscribed after seeing the ad |
| `language_displayed` | Language of the ad |
| `language_preferred` | Language preferred by the user |
| `age_group` | Age bracket of the user |
| `date_subscribed` | Subscription date (subscribers only) |
| `date_canceled` | Cancellation date (cancelled subscribers only) |
| `subscribing_channel` | Channel through which the user subscribed |
| `is_retained` | Whether the subscriber is still subscribed |

## Methodology
1. **Data cleaning**: fix data types, parse dates, map `True`/`False` flags to 1/0, create day-of-week and language-match features.
2. **Exploratory analysis**: ad volume, conversion rate by channel, age group, test variant and day of week.
3. **Root-cause analysis**: rule out weekday/weekend effects, then identify the language mismatch behind the House Ads drop and estimate its impact.
4. **A/B testing**: lift and two-sample t-tests for Email and Instagram, overall and by segment (language, age group).

## Repository Structure
```
marketing-campaign-analysis/
├── Marketing_Campaign_Analysis.ipynb   # Full annotated analysis
├── images/                             # Charts used in this README
├── requirements.txt                    # Python dependencies
├── README.md                           # English documentation
├── README.ru.md                        # Russian documentation
└── .gitignore
```

## Getting Started
```bash
# 1. Clone the repository
git clone https://github.com/<your-username>/marketing-campaign-analysis.git
cd marketing-campaign-analysis

# 2. Create and activate a virtual environment (Python 3.8-3.10 recommended)
python -m venv .venv
source .venv/bin/activate        # Windows: .venv\Scripts\activate

# 3. Install dependencies
pip install -r requirements.txt

# 4. Place marketing.csv in the repository root, then start Jupyter
jupyter notebook Marketing_Campaign_Analysis.ipynb
```

## Limitations
- Only one month of data; no long-term effects can be assessed.
- Very small samples for some segments (e.g. Email in Arabic, Spanish and German), so those results should be interpreted with caution.
- Facebook and Push personalization effects were not formally tested.
- The lost-subscriber estimate is an upper bound (see Key Findings).
