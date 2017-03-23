---
title: pandas summary statistics
date: 2017-03-23 11:22:28
header-img: /img/python-bg.jpg
tags:
  - statistics
  - python
  - pandas
categories:
  - python
---

# Pandas Summary Statistics

```python
import numpy as np
import pandas as pd
from pandas import Series,DataFrame
```

```python
arr = np.array([[1,2,np.nan],[np.nan,3,4]])
```

```python
dframe1 = DataFrame(arr,index=['A','B'],columns=['One','Two','Three'])
dframe1
```

|     |One|Two|Three|
|-----|---|---|-----|
|**A**|1.0|2.0|NaN  |
|**B**|NaN|3.0|4.0  |

```python
dframe1.sum()
```

    One      1.0
    Two      5.0
    Three    4.0
    dtype: float64

```python
dframe1.sum(axis=1)
```

    A    3.0
    B    7.0
    dtype: float64

```python
dframe1.min()
```

    One      1.0
    Two      2.0
    Three    4.0
    dtype: float64

```python
dframe1.idxmin() # Get index of minimun value
```

    One      A
    Two      A
    Three    B
    dtype: object

```python
dframe1
```

||One|Two|Three|
|-|--- |--- |--- |
|**A**|1.0|2.0|NaN|
|**B**|NaN|3.0|4.0|


```python
dframe1.cumsum() # Acumulation sum
```

||One|Two|Three|
|--- |--- |--- |--- |
|**A**|1.0|2.0|NaN|
|**B**|NaN|5.0|4.0|

```python
dframe1.describe()
```

||One|Two|Three|
|--- |--- |--- |--- |
|**count**|1.0|2.000000|1.0|
|**mean**|1.0|2.500000|4.0|
|**std**|NaN|0.707107|NaN|
|**min**|1.0|2.000000|4.0|
|**25%**|1.0|2.250000|4.0|
|**50%**|1.0|2.500000|4.0|
|**75%**|1.0|2.750000|4.0|
|**max**|1.0|3.000000|4.0|

```python
import pandas_datareader.data as pdweb
import datetime
```

```python
prices = pdweb.get_data_yahoo(['CVX','XOM','BP'],start=datetime.datetime(2010,1,1),
                              end=datetime.datetime(2013,1,1))['Adj Close']
prices.head()
```

|Date|BP|CVX|XOM|
|--- |--- |--- |--- |
|2010-01-04|41.524677|60.612343|56.187171|
|2010-01-05|41.819526|61.041678|56.406554|
|2010-01-06|42.037154|61.049341|56.894077|
|2010-01-07|42.023113|60.819345|56.715323|
|2010-01-08|42.121396|60.926678|56.487807|


```python
volume = pdweb.get_data_yahoo(['CVX','XOM','BP'],start=datetime.datetime(2010,1,1),
                              end=datetime.datetime(2013,1,1))['Volume']
volume.head()
```

|Date|BP|CVX|XOM|
|--- |--- |--- |--- |
|2010-01-04|3956100.0|10173800.0|27809100.0|
|2010-01-05|4109600.0|10593700.0|30174700.0|
|2010-01-06|6227900.0|11014600.0|35044700.0|
|2010-01-07|4431300.0|9626900.0|27192100.0|
|2010-01-08|3786100.0|5624300.0|24891800.0|

```python
rets = prices.pct_change()
```

```python
# Correlation of the stocks
corr = rets.corr()
```

```python
%matplotlib inline
prices.plot()
```

![png](output_19_1.png)


```python
import seaborn as sns
import seaborn.linearmodels as sns_ols
import matplotlib.pyplot as plt
```


```python
sns_ols.corrplot(rets,annot=False, diag_names=False)
```

![png](output_21_1.png)


```python
rets.corr()
```

| |BP|CVX|XOM|
|--- |--- |--- |--- |
|**BP**|1.000000|0.589024|0.617390|
|**CVX**|0.589024|1.000000|0.854781|
|**XOM**|0.617390|0.854781|1.000000|


```python
mask = np.zeros_like(rets.corr())
mask[np.triu_indices_from(mask)] = True
cmap = sns.diverging_palette(250, 15, as_cmap=True)
sns.set(style="white")

sns.heatmap(corr, cmap=cmap, vmin=-1, vmax=1, mask=mask, square=True)
```

![png](output_23_1.png)


```python
ser1 = Series(['w','w','x','y','z','w','y','a','x','w'])
ser1
```

    0    w
    1    w
    2    x
    3    y
    4    z
    5    w
    6    y
    7    a
    8    x
    9    w
    dtype: object


```python
ser1.unique()
```

    array(['w', 'x', 'y', 'z', 'a'], dtype=object)

```python
ser1.value_counts()
```

    w    4
    x    2
    y    2
    a    1
    z    1
    dtype: int64
