# Power Outage Analysis
by Tony Kuo and Tony Zheng
https://tzheng846.github.io/Power_Outage_Analysis/
# Why So Serious? - Outage Duration Level Classifier

**Name(s):** Yi-Hsuan Kuo, Tony Zheng  
**Website Link:** [Power Outage Analysis](https://tzheng846.github.io/Power_Outage_Analysis/)  

---

## 1.1 Step 1: Introduction
We aim to answer: **What are the characteristics of different levels of outage duration?**

---

## 1.2 Step 2: Data Cleaning and EDA

### 1.2.1 Data Cleaning
```python
# Load data and select relevant columns
original = pd.read_csv('data/power_outage.csv')
outage = original[['YEAR', 'MONTH', 'U.S._STATE', 'NERC.REGION', 'CLIMATE.REGION', 
                   'ANOMALY.LEVEL', 'CLIMATE.CATEGORY', 'OUTAGE.START.DATE', 
                   'OUTAGE.START.TIME', 'CAUSE.CATEGORY', 'CAUSE.CATEGORY.DETAIL', 
                   'HURRICANE.NAMES', 'OUTAGE.DURATION', 'DEMAND.LOSS.MW', 
                   'CUSTOMERS.AFFECTED', 'RES.PRICE', 'COM.PRICE', 'IND.PRICE', 
                   'TOTAL.PRICE', 'TOTAL.CUSTOMERS', 'POPULATION', 'AREAPCT_URBAN', 
                   'PCT_LAND']]

# Clean data
outage.dropna(subset=['OUTAGE.DURATION'], inplace=True)
outage['CAUSE.CATEGORY.DETAIL'] = outage['CAUSE.CATEGORY.DETAIL'].str.strip()
outage['DEMAND.LOSS.MW'] = outage['DEMAND.LOSS.MW'].replace(0, np.nan)
outage['CUSTOMERS.AFFECTED'] = outage['CUSTOMERS.AFFECTED'].replace(0, np.nan)
outage['OUTAGE.START.DATE'] = pd.to_datetime(outage['OUTAGE.START.DATE'])
outage['OUTAGE.START.TIME'] = outage['OUTAGE.START.TIME'].apply(
    lambda x: datetime.strptime(x, '%I:%M:%S %p').time()
)
outage['DAY.OR.NIGHT'] = outage['OUTAGE.START.TIME'].apply(
    lambda x: 'DAY' if (x > time(9)) and (x < time(17)) else 'NIGHT'
)
outage['HAS.HURRICANE'] = outage['HURRICANE.NAMES'].notna()
outage = outage.drop('HURRICANE.NAMES', axis=1)
px.histogram(outage, x='POPULATION', title='Population Distribution')
