
# Project Saussure

## 1. Libraries


```python
import matplotlib 
import pandas as pd 
import numpy as np
import re
```

## 2. Data Importation and Cleaning


```python
f = open('/Users/theotortorici/Google Drive/Project Saussure/whatsapp_chat_o2_v2/_chat_v2.txt')
d = f.read()
```


```python
d1 = d.split('\n')
type(d1)
d2 = pd.DataFrame(np.array(d1))
d2
```




