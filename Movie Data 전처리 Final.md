

```python
import pandas as pd
import numpy as np
import json
from tqdm import tqdm_notebook
```


```python
df = pd.read_csv('movies_metadata.csv',engine='python', thousands=',')
df =df[df['vote_count']>200]
df =df[df['vote_average']<11]
df = df[df['vote_average']>5.5]
df = df[df['runtime']<300]
df = df[df['runtime']>10]
df = df[df['budget']!='0']
df = df[df['revenue']>100]
```


```python
df.to_csv("newoutput.csv")
df = pd.read_csv('newoutput.csv',engine='python', thousands=',')
```


```python
for i in tqdm_notebook(range(len(df['genres']))):
    x = df['genres'][i]
    json_acceptable_string = x.replace("'", "\"")
    df['genres'][i] = json.loads(json_acceptable_string)
```


    HBox(children=(IntProgress(value=0, max=2704), HTML(value='')))


    C:\ProgramData\Anaconda3\lib\site-packages\ipykernel_launcher.py:4: SettingWithCopyWarning: 
    A value is trying to be set on a copy of a slice from a DataFrame
    
    See the caveats in the documentation: http://pandas.pydata.org/pandas-docs/stable/indexing.html#indexing-view-versus-copy
      after removing the cwd from sys.path.
    

    
    


```python
new_cols = ['genre1','genre2', 'genre3', 'genre4', 'genre5', 'genre6', 'genre7']
df2 = df.reindex(df.columns.union(new_cols), axis=1)
```


```python
for i in tqdm_notebook(range(len(df))):
    for j in range(len(df['genres'][i])):
        df2['genre'+str(j+1)][i]=df['genres'][i][j]['name']
```


    HBox(children=(IntProgress(value=0, max=2704), HTML(value='')))


    C:\ProgramData\Anaconda3\lib\site-packages\ipykernel_launcher.py:3: SettingWithCopyWarning: 
    A value is trying to be set on a copy of a slice from a DataFrame
    
    See the caveats in the documentation: http://pandas.pydata.org/pandas-docs/stable/indexing.html#indexing-view-versus-copy
      This is separate from the ipykernel package so we can avoid doing imports until
    C:\ProgramData\Anaconda3\lib\site-packages\pandas\core\indexing.py:189: SettingWithCopyWarning: 
    A value is trying to be set on a copy of a slice from a DataFrame
    
    See the caveats in the documentation: http://pandas.pydata.org/pandas-docs/stable/indexing.html#indexing-view-versus-copy
      self._setitem_with_indexer(indexer, value)
    

    
    


```python
df2 = df2[['title', 'id', 'revenue', 'vote_average', 'vote_count', 'genre1','genre2', 'genre3', 'genre4', 'genre5', 'genre6', 'genre7']]
```


```python
df2.to_csv("output0604.csv")
```
