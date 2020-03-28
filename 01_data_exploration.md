# Electric car charging points in Germany


```python
import pandas as pd
import numpy as np

import matplotlib.pyplot as plt
import seaborn as sns
```


```python
data = pd.read_excel('data/Ladesaeulenkarte_Datenbankauszug30.xlsx', skiprows=5)
```


```python
data.head()
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Betreiber</th>
      <th>Adresse</th>
      <th>Postleitzahl Ort</th>
      <th>Bundesland</th>
      <th>Längengrad [DG]</th>
      <th>Breitengrad [DG]</th>
      <th>Inbetriebnahmedatum</th>
      <th>Anschlussleistung [kW]</th>
      <th>Art der Ladeeinrichtung</th>
      <th>Anzahl Ladepunkte</th>
      <th>...</th>
      <th>Public Key1</th>
      <th>Steckertypen2</th>
      <th>P2 [kW]</th>
      <th>Public Key2</th>
      <th>Steckertypen3</th>
      <th>P3 [kW]</th>
      <th>Public Key3</th>
      <th>Steckertypen4</th>
      <th>P4 [kW]</th>
      <th>Public Key4</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>MVV Energie AG</td>
      <td>Luisenring 49</td>
      <td>68159 Mannheim</td>
      <td>Baden-Württemberg</td>
      <td>8.46764</td>
      <td>49.4947</td>
      <td>2019-08-30</td>
      <td>44</td>
      <td>Normalladeeinrichtung</td>
      <td>2</td>
      <td>...</td>
      <td>NaN</td>
      <td>AC Steckdose Typ 2</td>
      <td>22.0</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>1</th>
      <td>MVV Energie AG</td>
      <td>L 13 15</td>
      <td>68161 Mannheim</td>
      <td>Baden-Württemberg</td>
      <td>8.46887</td>
      <td>49.4808</td>
      <td>2018-12-20</td>
      <td>44</td>
      <td>Normalladeeinrichtung</td>
      <td>2</td>
      <td>...</td>
      <td>NaN</td>
      <td>AC Steckdose Typ 2</td>
      <td>22.0</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>2</th>
      <td>MVV Energie AG</td>
      <td>Q6</td>
      <td>68161 Mannheim</td>
      <td>Baden-Württemberg</td>
      <td>8.47209</td>
      <td>49.4864</td>
      <td>2016-11-01</td>
      <td>22</td>
      <td>Normalladeeinrichtung</td>
      <td>1</td>
      <td>...</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>3</th>
      <td>MVV Energie AG</td>
      <td>Q6</td>
      <td>68161 Mannheim</td>
      <td>Baden-Württemberg</td>
      <td>8.47209</td>
      <td>49.4864</td>
      <td>2016-11-01</td>
      <td>22</td>
      <td>Normalladeeinrichtung</td>
      <td>1</td>
      <td>...</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>4</th>
      <td>MVV Energie AG</td>
      <td>Q6</td>
      <td>68161 Mannheim</td>
      <td>Baden-Württemberg</td>
      <td>8.47209</td>
      <td>49.4864</td>
      <td>2016-11-01</td>
      <td>11</td>
      <td>Normalladeeinrichtung</td>
      <td>1</td>
      <td>...</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
  </tbody>
</table>
<p>5 rows × 22 columns</p>
</div>




```python
data.columns = data.columns.str.replace(' ', '')
```


```python
data.info()
```

    <class 'pandas.core.frame.DataFrame'>
    RangeIndex: 13006 entries, 0 to 13005
    Data columns (total 22 columns):
     #   Column                 Non-Null Count  Dtype         
    ---  ------                 --------------  -----         
     0   Betreiber              13006 non-null  object        
     1   Adresse                13006 non-null  object        
     2   PostleitzahlOrt        13006 non-null  object        
     3   Bundesland             13006 non-null  object        
     4   Längengrad[DG]         13006 non-null  object        
     5   Breitengrad[DG]        13006 non-null  object        
     6   Inbetriebnahmedatum    13006 non-null  datetime64[ns]
     7   Anschlussleistung[kW]  13006 non-null  object        
     8   ArtderLadeeinrichtung  13006 non-null  object        
     9   AnzahlLadepunkte       13006 non-null  int64         
     10  Steckertypen1          13006 non-null  object        
     11  P1[kW]                 13006 non-null  float64       
     12  PublicKey1             448 non-null    object        
     13  Steckertypen2          11460 non-null  object        
     14  P2[kW]                 11461 non-null  float64       
     15  PublicKey2             385 non-null    object        
     16  Steckertypen3          568 non-null    object        
     17  P3[kW]                 570 non-null    float64       
     18  PublicKey3             9 non-null      object        
     19  Steckertypen4          400 non-null    object        
     20  P4[kW]                 401 non-null    float64       
     21  PublicKey4             8 non-null      object        
    dtypes: datetime64[ns](1), float64(4), int64(1), object(16)
    memory usage: 2.2+ MB



```python
data.nunique()
```




    Betreiber                 1408
    Adresse                  10585
    PostleitzahlOrt           4938
    Bundesland                  16
    Längengrad[DG]           11470
    Breitengrad[DG]          11460
    Inbetriebnahmedatum       1912
    Anschlussleistung[kW]      109
    ArtderLadeeinrichtung        2
    AnzahlLadepunkte             4
    Steckertypen1               31
    P1[kW]                      47
    PublicKey1                 447
    Steckertypen2               24
    P2[kW]                      39
    PublicKey2                 384
    Steckertypen3               12
    P3[kW]                      25
    PublicKey3                   9
    Steckertypen4               11
    P4[kW]                      16
    PublicKey4                   8
    dtype: int64




```python
bundesland_summary = pd.pivot_table(data = data, index = 'Bundesland', values = 'AnzahlLadepunkte', aggfunc = ['count', 'sum'])
bundesland_summary.reset_index(inplace=True)
bundesland_summary.columns = ['Bundesland', 'counts', 'sum']
```


```python
bundesland_summary.sort_values(by='counts', ascending=False)
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Bundesland</th>
      <th>counts</th>
      <th>sum</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>1</th>
      <td>Bayern</td>
      <td>3002</td>
      <td>5995</td>
    </tr>
    <tr>
      <th>9</th>
      <td>Nordrhein-Westfalen</td>
      <td>2285</td>
      <td>4442</td>
    </tr>
    <tr>
      <th>0</th>
      <td>Baden-Württemberg</td>
      <td>1863</td>
      <td>3569</td>
    </tr>
    <tr>
      <th>8</th>
      <td>Niedersachsen</td>
      <td>1262</td>
      <td>2372</td>
    </tr>
    <tr>
      <th>6</th>
      <td>Hessen</td>
      <td>802</td>
      <td>1577</td>
    </tr>
    <tr>
      <th>10</th>
      <td>Rheinland-Pfalz</td>
      <td>565</td>
      <td>1094</td>
    </tr>
    <tr>
      <th>14</th>
      <td>Schleswig-Holstein</td>
      <td>536</td>
      <td>1043</td>
    </tr>
    <tr>
      <th>5</th>
      <td>Hamburg</td>
      <td>521</td>
      <td>1054</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Berlin</td>
      <td>516</td>
      <td>938</td>
    </tr>
    <tr>
      <th>12</th>
      <td>Sachsen</td>
      <td>486</td>
      <td>1058</td>
    </tr>
    <tr>
      <th>15</th>
      <td>Thüringen</td>
      <td>343</td>
      <td>681</td>
    </tr>
    <tr>
      <th>3</th>
      <td>Brandenburg</td>
      <td>243</td>
      <td>478</td>
    </tr>
    <tr>
      <th>13</th>
      <td>Sachsen-Anhalt</td>
      <td>240</td>
      <td>469</td>
    </tr>
    <tr>
      <th>7</th>
      <td>Mecklenburg-Vorpommern</td>
      <td>142</td>
      <td>261</td>
    </tr>
    <tr>
      <th>4</th>
      <td>Bremen</td>
      <td>129</td>
      <td>254</td>
    </tr>
    <tr>
      <th>11</th>
      <td>Saarland</td>
      <td>71</td>
      <td>149</td>
    </tr>
  </tbody>
</table>
</div>




```python
fig, ax = plt.subplots(figsize = (10,7))
sns.barplot(data = bundesland_summary, x = 'counts', y = 'Bundesland', ax=ax)
ax.set_xlabel('Number of charging stations')
ax.set_ylabel('')
ax.set_title('Number of electric vehicle charging points per Bundesland in Germany')
ax.grid(alpha=0.3);
```


![png](01_data_exploration_files/01_data_exploration_9_0.png)



```python

```
