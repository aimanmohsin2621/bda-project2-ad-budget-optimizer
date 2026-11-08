# Digital Advertising Budget Optimizer (Big Data Analytics Project 2)


> **All data in this project is simulated.** Platform behaviour (cost per thousand impressions, click-through and
> conversion rates, order value, diminishing returns) comes from illustrative assumptions, not measured industry
> benchmarks and not from any real company. The results demonstrate the method, not real campaign performance.

## What it does

1. Generates a 12-column advertising dataset (`date, campaign, platform, impressions, clicks, spend, conversions,
   revenue, age, gender, city, device`) across Facebook, Instagram, Google, TikTok and YouTube.
2. Calculates CTR, CPC, CPA, conversion rate, ROAS, revenue and campaign performance (ratios from summed totals).
3. Runs a Big Data simulation from 10 thousand to 10 million records and compares **Pandas**, a **MapReduce-style**
   job and **Spark** (local mode). All engines return identical totals.
4. Fits diminishing-returns curves per platform (ROAS falls as daily spend rises).
5. Builds a **Marketing Budget Recommendation Engine** for the next Rs 1,000,000, then checks it with a held-out
   forecast and a simulated A/B test.

## Results (simulated data)

**Recommended split of Rs 1,000,000:** Facebook 24%, Instagram 21%, Google 45%, TikTok 5%, YouTube 5%
(5% to 45% limits per platform). Predicted revenue about Rs 4.90M. In the simulated A/B test the engine
earns 16.4% more than the current mix.

| Rows | Pandas | MapReduce-style | Spark (local) |
|---|---|---|---|
| 10,000 | 0.06 s | 0.08 s | 11.72 s |
| 100,000 | 0.22 s | 0.24 s | 12.87 s |
| 1,000,000 | 2.12 s | 2.10 s | 16.62 s |
| 10,000,000 | 20.11 s | 25.00 s | 39.91 s |

Peak memory at 10 million rows: Pandas 2,434 MB, MapReduce-style 46 MB, Spark 1,145 MB.
Measured on Google Colab (2 virtual CPUs), median of 3 runs. Spark times exclude session start-up.

**Reading the benchmark:** on one small machine Pandas is fastest up to 10 million rows. Spark has a large fixed
cost but grows much more slowly (2.4x from 1M to 10M rows against about 10x to 12x for the others), and Pandas
memory grows with the data. The MapReduce engine is a pure-Python model of the programming approach, not Hadoop.

## Limitations

- Simulated data: platform differences are assumptions, not discoveries.
- The engine's budget for TikTok and YouTube is below the range seen in the history (extrapolated).
- The A/B check is circular (the simulator uses the same kind of diminishing-returns effect).
- ROAS is revenue per spend, not profit; sale periods and creative fatigue are not modelled in the engine.
- The 5% minimum forces spend into YouTube, where the marginal ROAS is below 1: it is a business choice.
- Part of the click-through assumptions is scaled from a relative engagement level in my Project 1 dataset, which
  is itself unverified; this only sets the ordering of rates.

## How to run

1. Open `Project2_Ad_Budget_Optimizer.ipynb` in [Google Colab](https://colab.research.google.com).
2. Click **Runtime > Run all**. The 10-million-row benchmark takes several minutes. To skip it, set `RUN_10M = False`
   in the benchmark cell.

The notebook writes its own helper files (`adsim.py` for data generation, `bench.py` for the benchmark), so no
other files are needed.

## Files

| File | Description |
|---|---|
| `Project2_Ad_Budget_Optimizer.ipynb` | Full analysis notebook with outputs |
| `Project2_Report.docx` | Written report |

## Next step

Replace the simulated table with an authorised campaign export that has the same 12 columns and re-run the notebook
unchanged. Add profit margin, sale periods and creative fatigue, and test the recommendation on a small real budget
with a control group.
