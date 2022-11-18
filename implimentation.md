## 0. Install and Import Dependencies
> Install anaconda distributer pack from anaconda [website](https://www.anaconda.com/products/distribution/)
> 
> After installed, search anaconda powershell prompt and open it and enter **"jupyter notebook"**
> 
> After jupyter notebook launched in our browser.
> 
> Use the package manager [pip](https://pip.pypa.io/en/stable/) to install neuralprophet.

```bash
pip install neuralprophet 
```
> importing pandas,neuralprophet,pyplot and pickle.
```python
import pandas as pd 
from neuralprophet import NeuralProphet 
from matplotlib import pyplot as plt 
import pickle 
```
> loading the csv into pandas.
```python
df = pd.read_csv('weatherAUS.csv')
```
## 1. Read in Data and Process Dates

> perprocesing the data.
```python
melb = df[df['Location']=='Melbourne'] 
melb['Date'] = pd.to_datetime(melb['Date']) 
plt.plot(melb['Date'], melb['Temp3pm']) 
plt.show() 
 ```
 > deleting the empty spaces.
 ```python
 melb['Year'] = melb['Date'].apply(lambda x: x.year) 
melb = melb[melb['Year']<=2015] 
plt.plot(melb['Date'], melb['Temp3pm']) 
plt.show() 
```
 >creating the data has two value x and y for forcasting .
 ```python
data = melb[['Date', 'Temp3pm']] 
data.dropna(inplace=True) 
data.columns = ['ds', 'y'] 
data.head() 
```
## 2. Train Model

```python
m = NeuralProphet(epochs=1000) 
model = m.fit(data, freq='D') 

 ```
## 3. Forecast Away
```python
future = m.make_future_dataframe(data, periods=100) 
forecast = m.predict(future) 
 plot1 = m.plot(forecast)
 ```

## 4. Save Model
```python
with open('saved_model.pkl', "wb") as f: 
pickle.dump(m, f) 
 ```
