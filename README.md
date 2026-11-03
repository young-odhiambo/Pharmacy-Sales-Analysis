# Pharmacy Sales Data Cleaning & Analysis

A mini data-analysis project in Python that takes a messy, real-world-style pharmacy sales dataset from Kenya and turns it into a clean, analysis-ready table.

## Project Overview

Retail pharmacy data is rarely tidy. Orders come from different branches, channels, and staff, so the same value is often typed many different ways (`Nairobi`, `NAIROBI`, `NRB`, `nairobi `). This project walks through a full **exploratory data analysis (EDA) and cleaning workflow** to fix those problems so the data can be trusted for reporting and decision-making.

The cleaned data can then be used to answer business questions such as:

- Which branches, cities, and channels (in-store, app, online) bring in the most revenue?
- Which product categories sell best, and how much do prescription items contribute?
- When do sales spike, so staffing and stock levels can be planned around them?
- How do promo codes and payment methods (M-Pesa, card, cash, insurance) affect sales?

## Dataset

**File:** `pharmacy_sales_raw.csv`

| Item | Detail |
|---|---|
| Rows | 6,150 orders |
| Columns | 26 |
| Period covered | January – December 2026 |
| Cities | Nairobi, Mombasa, Kisumu, Nakuru, Eldoret |
| Channels | In-store, App, Online |
| Products | 338 unique products across 24 raw category labels |
| Customers | 1,728 unique customer IDs |

**Main columns:** `order_id`, `order_date`, `delivery_date`, `customer_id`, `customer_name`, `customer_phone`, `customer_email`, `customer_gender`, `customer_age`, `city`, `branch`, `channel`, `product_id`, `product_name`, `category`, `requires_prescription`, `expiry_date`, `unit_price_kes`, `quantity`, `discount_pct`, `promo_code`, `total_amount_kes`, `delivery_fee_kes`, `payment_method`, `customer_rating`, `customer_review`

### Data quality issues found in the raw file

- **Duplicates:** 120 fully duplicated rows and 150 repeated `order_id` values
- **Inconsistent text:** mixed casing, stray spaces, and abbreviations in city, category, payment method, and promo code
- **Inconsistent flags:** `requires_prescription` stored as `Y`, `N`, `yes`, `no`, `1`, `0`, `TRUE`, `FALSE`
- **Mixed date formats** in `order_date`
- **Messy phone numbers:** `+254…`, `0799 297 864`, `0746-061-372`, `730127413`
- **Currency text in prices:** `unit_price_kes` stored as text with `KES`/`KSH` prefixes
- **Outliers:** impossible customer ages (e.g. 999)
- **Missing values** in several columns, including phone, email, age, branch, rating, and review

## Cleaning Steps

All work is in `sales_analysis.ipynb`.

1. **Pre-cleaning EDA:** a reusable `basic_checks()` function reports shape, columns, duplicates, data types, missing values, and summary statistics. Boxplots are used to spot outliers in numeric columns.
2. **Remove duplicates:** drop exact duplicate rows, then drop repeated `order_id` values (keeping the first).
3. **Standardise categorical columns:**
   - `city`: unified to Nairobi, Mombasa, Kisumu, etc.
   - `category`: merged variants (e.g. `Cold and Flu` → `Cold & Flu`, `Mum and Baby` → `Mum & Baby`)
   - `requires_prescription`: converted to `Yes` / `No`
   - `payment_method`: unified to `M-Pesa`, `Card`, `Cash`
   - `promo_code`: trimmed, upper-cased, spaces and dashes removed
4. **Clean text fields:** `product_name`, `customer_name`, and `customer_review` are trimmed and case-normalised; empty or `-` reviews are set to `None`.
5. **Parse dates:** `order_date` converted to a proper datetime (mixed formats, day-first). Timestamps are kept because they help with planning staff, stock, and inventory around peak periods.
6. **Format phone numbers:** a `format_phone()` function converts all numbers to the local `07XXXXXXXX` format.
7. **Clean emails:** trimmed and lower-cased.
8. **Handle age outliers:** IQR method used to inspect outliers, and unrealistic ages (100 and above) removed.
9. **Fix prices:** strip `KES`/`KSH` text from `unit_price_kes` and convert it to a numeric column.

## Tools & Libraries

- Python 3
- [pandas](https://pandas.pydata.org/) for data manipulation
- [NumPy](https://numpy.org/) for numerical operations
- [Matplotlib](https://matplotlib.org/) and [Seaborn](https://seaborn.pydata.org/) for visualisation
- Jupyter Notebook

## How to Run

1. Clone or download this repository.
2. Install the dependencies:
   ```bash
   pip install pandas numpy seaborn matplotlib jupyter
   ```
3. Put `pharmacy_sales_raw.csv` in the same folder as the notebook, and update the file path in the notebook's load cell to a relative one:
   ```python
   sales_data = pd.read_csv('pharmacy_sales_raw.csv')
   ```
4. Launch Jupyter and run the notebook from top to bottom:
   ```bash
   jupyter notebook sales_analysis.ipynb
   ```

## Project Structure

```
.
├── sales_analysis.ipynb      # EDA and data cleaning notebook
├── pharmacy_sales_raw.csv    # Raw dataset
└── README.md
```

## Next Steps

- Handle remaining missing values (branch, payment method, quantity, discount)
- Validate `total_amount_kes` against `unit_price_kes × quantity` and the discount
- Save the cleaned data to a new file (e.g. `pharmacy_sales_clean.csv`)
- Analyse sales by branch, category, channel, and time of day / month
- Build visualisations and summarise key insights

## Author

Your Name · [GitHub](https://github.com/your-username) · [LinkedIn](https://www.linkedin.com/in/your-profile)