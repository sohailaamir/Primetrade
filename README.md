# Trader Performance vs Market Sentiment 📊

## 🎯 Objective
Analyze how market sentiment (Fear/Greed) relates to trader behavior and performance on Hyperliquid. The goal of this analysis is to uncover data-driven patterns that can inform smarter, automated trading strategies.

---

## 📂 Repository Structure
```text
Primetrade_Intern_Assignment/
│
├── data/
│   ├── fear_greed_index.csv       # Daily sentiment classifications
│   └── historical_data.csv        # Hyperliquid trader execution data
│
├── analysis.ipynb                 # Main Jupyter Notebook (Code & Visuals)
├── README.md                      # Project documentation (This file)
│
└── Visualizations/
    ├── avg_pnl.png                # Chart: Average PnL by Sentiment
    ├── pnl_segment.png            # Chart: PnL by Size Segment & Sentiment
    └── win_rate.png               # Chart: Win Rate by Sentiment
```

---

## ⚙️ Setup & How to Run (Reproducibility)
To reproduce the findings and run the predictive model locally:

1. **Clone the repository**:
   ```bash
   git clone <your-github-repo-url>
   cd Primetrade_Intern_Assignment
   ```
2. **Install dependencies**:
   Ensure you have Python 3.8+ installed. Install the required packages via pip:
   ```bash
   pip install pandas numpy matplotlib seaborn scikit-learn
   ```
3. **Execute the Notebook**:
   Open `analysis.ipynb` in Jupyter Notebook, Jupyter Lab, or VS Code, and run all cells sequentially. The code will automatically clean the data, generate the visualizations, and train the Bonus Random Forest model.

---

## 🔬 Methodology
1. **Data Alignment**: Loaded historical Hyperliquid trader data and the Fear/Greed index. Mismatched timestamp strings (IST) were converted to standardized `YYYY-MM-DD` formats to allow for clean, daily-level merging.
2. **Feature Engineering**: Normalized "Extreme" sentiments into baseline `Fear` / `Greed` buckets to maintain statistical significance. Created a proxy for "Leverage" by segmenting traders by `Size USD` (Large Size vs Small Size), as explicit leverage metrics were absent.
3. **Segmentation**: Categorized users into **Frequent** and **Infrequent** segments dynamically, split by the median overall trade count per account.
4. **Performance Metrics**: Calculated Max Drawdown strictly chronologically per account and defined "Wins" exclusively on closed/settled trades where `Closed PnL != 0`.

---

## 💡 Data-Backed Insights
1. **Volatility Drives Size**: Average trade size (USD) jumps significantly from Greed to Fear periods, indicating aggressive positioning during market dips.
2. **Improved Win Rates in Fear**: Contrary to intuition, traders maintain a higher aggregate win rate (~84%) during Fear periods compared to Greed (~82%).
3. **Drawdown Discrepancy**: 'Large Size' traders experience massive max drawdowns during rapid sentiment shifts compared to small-size traders.
4. **Segment Divergence**: Frequent traders thrive during Fear (high volatility), while Infrequent traders perform worse and overexpose themselves to shorts during Greed.

---

## 🚀 Actionable Strategy Recommendations
Based on the empirical evidence, I propose the following rules of thumb:

1. **Volatility Scaling (For Frequent/Algorithmic Traders)**: 
   *Rule:* Scale up position sizing and trade frequency during `Fear` days. 
   *Reasoning:* Win rates and absolute profitability naturally peak for this segment during high-volatility dips.
2. **Retail Protection (For Infrequent Traders)**: 
   *Rule:* Enforce strict discretionary sizing limits and algorithmic trailing stops during `Greed` days. 
   *Reasoning:* Infrequent/retail traders skew heavily into counter-trend shorting during Greed, which yields poor returns and substantially higher drawdowns.

---

## 🤖 Bonus: Predictive Modeling
Included at the end of the notebook is a lightweight Machine Learning model (`RandomForestClassifier`) designed to predict the next day's aggregate profitability. 
* **Features Used**: Sentiment Score, T-1 Average Trade Size, T-1 Trade Volume (Account Count), and T-1 Total PnL.
* **Results**: The model achieved an overall out-of-sample accuracy of **83%**, demonstrating that sentiment combined with lagged behavioral metrics carries strong predictive signals for short-term market profitability.
```
