# Kazakhstan Apartment Prices: Scraping → Analysis → Prediction

An end-to-end data science project on the Kazakhstan housing market. I collected my own dataset by scraping **~3,600 apartment listings from [krisha.kz](https://krisha.kz)**, then cleaned it, explored it and trained models to predict apartment prices.

![Average area by number of rooms](images/avg_area_by_rooms.png)

## What's inside

| Step | What I did |
|---|---|
| **1. Data collection** | Wrote a web scraper (`requests` + `BeautifulSoup`) that went through 180 pages of listings and extracted the price, address, city, publication date, number of rooms and area for each one |
| **2. Cleaning** | Handled missing values, dropped an empty column, parsed rooms and area from listing titles with regex, removed duplicates |
| **3. Feature engineering** | Grouped 40+ cities into 5 regions of Kazakhstan (Central, South, West, North, East) |
| **4. EDA** | Distributions of price, area and rooms, average area by room count, correlation analysis |
| **5. Modeling** | Compared Linear Regression, Decision Tree and Random Forest in a scikit-learn `Pipeline` (scaling + one-hot encoding) |

## Results

| Model | R² on test set |
|---|---|
| **Linear Regression** | **0.54** |
| Random Forest | 0.47 |
| Decision Tree | 0.44 |

The simplest model performed best and explains about half of the variation in price. That makes sense: listings only give rooms, area, region and a short title. A big part of an apartment's price comes from things that weren't in the data, like the exact district, building age, renovation and floor.

**Some findings from the data:**
- 2-room apartments are the most common listing type, followed by 3-room ones
- Average area grows steadily with the number of rooms, from ~30 m² for a 1-room to ~200 m² for a 7-room apartment
- The median asking price in the dataset is **31 million ₸**

<p align="center">
  <img src="images/distributions.png" width="48%">
  <img src="images/correlation_heatmap.png" width="40%">
</p>

## What I'd improve next

Looking back at this project, here's what I'd change:
- **Area parsing bug:** decimal areas like `56.8 м²` were read as `8`, which explains the strange spike near 0 m² and the weak price–area correlation. The fix is a better regex: `(\d+(?:\.\d+)?)\s*м²`
- **Outliers:** one listing is priced at ~26 billion ₸, almost certainly a typo. Removing such outliers would make the charts and models much cleaner
- **Missing cities:** about half of the listings had no city, and they were all defaulted to the Central region
- **Metrics:** report the error in real tenge instead of scaled units, and try gradient boosting (XGBoost / LightGBM)

## Tech stack

`Python` · `requests` · `BeautifulSoup` · `pandas` · `NumPy` · `scikit-learn` · `matplotlib` · `seaborn` · `Plotly`

## How to run

```bash
pip install -r requirements.txt
jupyter notebook kazakhstan-apartment-prices.ipynb
```

Run the first cell to scrape fresh data. The dataset itself is not included in this repo.

---

**Author:** Nuraiym Aidaraly, Data Science student at SDU (Suleyman Demirel University)
