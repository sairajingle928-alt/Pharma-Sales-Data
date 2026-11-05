# Pharma-Sales-Data
patient data with medicines ,there ids ,rep ids,hcps,dates,times,days
<br>
import pandas as pd
<br>
import numpy as np
<br>
import matplotlib.pyplot as plt
<br>
import seaborn as sns   
<br>
import plotly.express as px
<br>
import plotly.io as pio
<br>
#Option # Option A: Renders the chart inside a local inline iframe (highly reliable)
<br>
pio.renderers.default = "notebook_connected"
<br
pd.set_option('display.float_format', lambda x: '%.4f' % x)
<br>
### 1.Loading the dataset
<br>
from pathlib import Path
<br>
import pandas as pd
<br>
Pharmacy_sales_data = pd.read_excel(r"C:\Users\saira\OneDrive\Desktop\PYTHON\Artificial intelligence\Deep learning\Pharmacy_sales_data.xlsx")
<br>
#Define the file path if it is missing
<br>
file_path = Path(r"C:\Users\saira\OneDrive\Desktop\PYTHON\Artificial intelligence\Deep learning\Pharmacy_Sales_Rep_Data.csv")
<br>

#Read the CSV only if it exists; otherwise use the already loaded Excel data
<br>
if file_path.exists():
<br>
    sales_rep_territory_data = pd.read_csv(file_path)
    <br>
else:
<br>
    sales_rep_territory_data = Pharmacy_sales_data.copy()
    <br>
    print(f"CSV file not found at {file_path}. Using the existing Excel data instead.")
    <br>
    sales_rep_territory_data = pd.read_csv('C:\\Users\\saira\\OneDrive\\Desktop\\PYTHON\\Artificial intelligence\\Deep learning\\Sales_Rep_Territory_Data.csv')
    <br>
    <br>
    CSV file not found at C:\Users\saira\OneDrive\Desktop\PYTHON\Artificial intelligence\Deep learning\Pharmacy_Sales_Rep_Data.csv. Using the existing Excel data instead.
    <br>
    <br>
    #Best fix: use the dataframe that already exists
    <br>
Pharmacy_sales_data.info()
<br>
<br>
<class 'pandas.DataFrame'>
RangeIndex: 500 entries, 0 to 499
Data columns (total 8 columns):
 #  Column                Non-Null Count  Dtype
---  ------  
<br>--------------  -----
 0   Transaction ID   500 non-null    str  
 1   Date            -      500 non-null    str  
 2   Year            -      500 non-null    int64
 3   Month           -      500 non-null    str  
 4   Patient ID       -     500 non-null    str  
 5   Medicine Name    -     500 non-null    str  
 6   Daily Sales Quantity - 500 non-null    int64
 7   Patients Count     -   500 non-null    int64
dtypes: int64(3), str(5)
<br>
memory usage: 31.4 KB
<br>
<br>
Pharmacy_sales_data['Date'] = pd.to_datetime(Pharmacy_sales_data['Date'])
<br>
sales_rep_territory_data.info()
<br>
<class 'pandas.DataFrame'>
<br>
RangeIndex: 500 entries, 0 to 499
<br>
Data columns (total 21 columns):
<br>
 #   Column                  Non-Null Count  Dtype  
---  ------                  --------------  -----  
 0   transaction_id    -      500 non-null    str    
 1   date              -      500 non-null    str    
 2   year              -     500 non-null    int64  
 3   month             -    500 non-null    str    
 4   patient_id        -      500 non-null    str    
 5   medicine_name      -     500 non-null    str    
 6   daily_sales_quantity  -  500 non-null    int64  
 7   patients_count     -     500 non-null    int64  
 8   sales_rep_id       -     500 non-null    str    
 9   sales_rep_name      -    500 non-null    str    
 10  territory_id        -    500 non-null    str    
 11  territory_name      -   500 non-null    str    
 12  state               -    500 non-null    str    
 13  city               -     500 non-null    str    
 14  doctor_visits      -     500 non-null    int64  
 15  pharmacy_visits    -     500 non-null    int64  
 16  monthly_sales_inr    -   500 non-null    int64  
 17  target_sales_inr    -    500 non-null    int64  
 18  report_month        -    500 non-null    str    
 19  total_customer_visits -  500 non-null    int64  
 20  target_achievement_pct - 500 non-null    float64
 <br>
dtypes: float64(1), int64(8), str(12)
<br>
memory usage: 82.2 KB
<br>
<br>
<br>
sales_rep_territory_data['date'] = pd.to_datetime(
<br>
    sales_rep_territory_data['date'],
    <br>
    errors='coerce'
    <br>
)
<br>
<br>
### 2).Analysis
<br>
### 2.1).Pharmacy Sales data
<br>
<br>
Pharmacy_sales_data.head()
<br>
<br>
Transaction ID	Date	Year	Month	Patient ID	Medicine Name	Daily Sales Quantity	Patients Count
<br>
0	TXN00001	2025-04-13	2025	April	PID0043	Atorvastatin 20mg	12	3
<br>
1	TXN00002	2026-03-12	2026	March	PID0065	Paracetamol 500mg	40	2
<br>
2	TXN00003	2025-09-28	2025	September	PID0035	Amlodipine 5mg	17	8
<br>
3	TXN00004	2025-04-17	2025	April	PID0093	Losartan 50mg	10	2
<br>
4	TXN00005	2025-03-13	2025	March	PID0007	Metformin 850mg	50	8
<br>
<br>
# Use the dataframe that already exists in the notebook
<br>
sales_data = Pharmacy_sales_data.copy()
<br>
sales_data.columns = [col.lower() for col in sales_data.columns
<br>
<br>
print(sales_data['date'].min(), sales_data['date'].max())
<br>
<br>
2025-01-02 2026-12-29
<br>
<br>
### Q1 What are the total number of Medicines in the market?
<br>
<br>
print(f"Total number of medicines: {sales_data['medicine name'].nunique()}.")
<br>
<br>
Total number of medicines: 10.
<br>
<br>
### How many Pateints are getting the medicine from the pharmacy?
<br>
<br>
# Use the dataframe already created in this notebook
<br>
df = sales_data.copy()
<br>
<br>
<br>
# Group total sales by month and patient
<br>
<br>
df['date'] = pd.to_datetime(df['date'], errors='coerce')
<br>
<br>
grouped_sales_data_month = (
    df.dropna(subset=['date'])
    <br>
    <br>
      .groupby([df['date'].dt.to_period('M'), df['patient id']])['daily sales quantity']
      <br>
      <br>
      .sum()
      <br>
      .reset_index()
      <br>
      .rename(columns={
          'date': 'month',
          'patient id': 'patient_id',
          'daily sales quantity': 'total_sales'
      })
)
<br>
<br>
print(grouped_sales_data_month.head(), end='\n\n')
<br>
# Patients with zero sales in at least one month
<br>
patients_with_no_sales = grouped_sales_data_month[
    grouped_sales_data_month['total_sales'] == 0
]['patient_id'].unique()
<br>
<br>
# Patients with sales in at least one month
<br>
<br>
patients_with_sales = grouped_sales_data_month[
    ~grouped_sales_data_month['patient_id'].isin(patients_with_no_sales)
]['patient_id'].nunique()
<br>
<br>
print(f"Total number of patients not purchasing any medicines in either month: {len(patients_with_no_sales)}")
<br>
print(f"Total number of patients purchasing medicines in at least one month: {patients_with_sales}")
<br>
<br>
month patient_id  total_sales
<br>
0  2025-01    PID0001           43
<br>
1  2025-01    PID0006            5
<br>
2  2025-01    PID0012           12
<br>
3  2025-01    PID0023           44
<br>
4  2025-01    PID0029           39
<br>
Total number of patients not purchasing any medicines in either month: 0
<br>
Total number of patients purchasing medicines in at least one month: 100
<br>
<br>
### How many patients prescribing medicines in both months?
<br>
<br>
# -- can be interpreted similar to above question
<br>
no_Medicines = sales_data[sales_data['daily sales quantity'] > 0]['patient id'].nunique()
<br>
print('Total number of Patients prescribing any medicines anytime in past 4 months: {}'.format(no_Medicines))
<br>
<br>
Total number of Patients prescribing any medicines anytime in past 4 months: 100
<br>
<br>
### What are the total sales and shares of Medicines of each state?
<br>
<br>
# Total sales per state
<br>
# Use the real state column if it exists; otherwise fall back to patient_id
<br>
state_col = next(
    (c for c in ['Patients_state', 'Patients State', 'patients_state', 'state', 'State', 'patient id'] if c in sales_data.columns),
    'patient id'
)
<br>
<br>
Pharmacy_sales_per_state = (
    sales_data.assign(
    <br>
        Paracetamol_500mg_sales_usd=sales_data['daily sales quantity'].where(
            sales_data['medicine name'].eq('Paracetamol 500mg'), 0
            <br>
            <br>
        ),
        other_Medicines_sales_usd=sales_data['daily sales quantity'].where(
            ~sales_data['medicine name'].eq('Paracetamol 500mg'), 0
        )
        <br>
        <br>
    )
    .groupby(state_col, as_index=False)
    <br>
    <br>
    .agg(
        Paracetamol_500mg_sales_usd=('Paracetamol_500mg_sales_usd', 'sum'),
        <br>
        <br>
        other_Medicines_sales_usd=('other_Medicines_sales_usd', 'sum')
        <br>
        <br>
    )
    .rename(columns={state_col: 'Patients_state'})
    <br>
    <br>
)
print(Pharmacy_sales_per_state.head(), end = '\n\n')
<br>
<br>
# Create share column using the exact names from Pharmacy_sales_per_state
<br>
<br>
denom = (
    Pharmacy_sales_per_state['Paracetamol_500mg_sales_usd']
    + Pharmacy_sales_per_state['other_Medicines_sales_usd']
    <br>
    <br>
)
<br>
<br>
<br>
Pharmacy_sales_per_state['Paracetamol_500mg_share'] 
<br>= (
    Pharmacy_sales_per_state['Paracetamol_500mg_sales_usd'] / denom * 100
).replace([float('inf'), float('-inf')], 0).fillna(0)
<br>
<br>
print(Pharmacy_sales_per_state.head(), end = '\n\n')
<br>
<br>
<br>
plt.figure(figsize=(15,9))
<br>
<br>
sns.barplot(
    data=Pharmacy_sales_per_state,
    <br>
    x='Patients_state',
    <br>
    y='Paracetamol_500mg_sales_usd',
    <br>
    order=Pharmacy_sales_per_state.sort_values(
    <br>
        by='Paracetamol_500mg_sales_usd',
        <br>
        ascending=False
        <br>
    ).Patients_state
    <br>
)
<br>
<br>
plt.show()
<br>
<br>
plt.figure(figsize=(15,9))
<br>
<br>
sns.barplot(
<br>
<br>
    data=Pharmacy_sales_per_state,
    <br>
    x='Patients_state',
    <br>
    y='Paracetamol_500mg_share',
    <br>
    order=Pharmacy_sales_per_state.sort_values(
    <br>
        by='Paracetamol_500mg_share',
        <br>
        ascending=False
        <br>
    ).Patients_state
    <br>
)
<br>
plt.show()
<br>
Patients_state  Paracetamol_500mg_sales_usd  other_Medicines_sales_usd
<br>
0    .    PID0001                    -        0          -              212
<br>
1     .   PID0002                    -       57           -              26
<br>
2     .   PID0003                     -       0          -              231
<br>
3     .   PID0004                    -        0            -             44
<br>
4     .   PID0005                    -       70            -            272
<br>
<br>
  Patients_state  Paracetamol_500mg_sales_usd  other_Medicines_sales_usd  \
  <br>
0   .     PID0001                    -        0              -          212   
<br>
1    .    PID0002                    -       57               -          26   
<br>
2   .     PID0003                    -        0               -         231   
<br>
3   .     PID0004                    -        0               -          44  
<br>
4   .     PID0005                     -      70               -         272   
<br>
<br>
   Paracetamol_500mg_share  
   <br>
0 .                0.000000  
<br>
1  .              68.674699  
<br>
2  .               0.000000  
<br>
3  .               0.000000  
<br>
4  .             20.467836
<br>

