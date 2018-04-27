
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
# Since its not a csv open as text

f = open('/Volumes/GoogleDrive/My Drive/Project Saussure/whatsapp_chat_o2_v2/_chat_v2.txt')
d = f.read()
```


```python
# Transform the text to a dataframe 

d1 = re.compile("(\d{2}/\d{2}/\d{4})").split(d) # don't forget the extra parenthesis to keep the delimiter
d1.pop(0) # delete element 1
d2 = pd.DataFrame(np.array(d1).reshape(-1,2)) # -1 means however long that array is
d2.columns = ['Date', 'Message_log'] 
```


```python
# Divide the one column one string dataframe into Timestamp/User/Message dataframe

d2['Time'] = d2.Message_log.str[2:10]
d2['User_msg'] = d2.Message_log.str[12:]
d2['User'] = d2.User_msg.str.split(':').str.get(0) # keep element 1 before sep of split inside User_msg
d2['Message'] = d2.User_msg.str.split(':').str.get(1) 
d2['Timestamp'] = d2.Date.str.cat(d2.Time, sep =' ') # combine Time and Date
```


```python
# Keep only Timestamp, User, Message 

df = pd.concat([d2['Timestamp'],d2['User'],d2['Message']], axis=1)
```


```python
# CLEANING 

# Drop rows where database was shared with dates therefore messes with our dataframe
df = df.drop([10101,10102,10103,10104,10105,10106,10107])
```


```python
# Convert timestamp to datetime
df['Timestamp'] = pd.to_datetime(df['Timestamp'], format="%d/%m/%Y %H:%M:%S")
df.info()

# Strip \n
df['Message'] = df['Message'].astype(str).str.rstrip('\n')

# Replace the unsaved number with Maria's name
df.User[df.User == ' \u202a+44\xa07477\xa0122897\u202c'] = 'Maria' 
```

    <class 'pandas.core.frame.DataFrame'>
    Int64Index: 11186 entries, 0 to 11192
    Data columns (total 3 columns):
    Timestamp    11186 non-null datetime64[ns]
    User         11186 non-null object
    Message      11159 non-null object
    dtypes: datetime64[ns](1), object(2)
    memory usage: 349.6+ KB


    /anaconda3/lib/python3.6/site-packages/ipykernel_launcher.py:9: SettingWithCopyWarning: 
    A value is trying to be set on a copy of a slice from a DataFrame
    
    See the caveats in the documentation: http://pandas.pydata.org/pandas-docs/stable/indexing.html#indexing-view-versus-copy
      if __name__ == '__main__':



```python
# Removing all whitespace before the name

df.User.replace('^\s+', '', regex=True, inplace=True)
```


```python
# index to drop (message by whatsapp when user come in out of the platform)

idx = df.loc[df['Message'] == 'nan'].index
df.drop(df.index[idx], inplace=True)
```


```python
# Handle exception 
idx2 = df.loc[df['User'] == " \u200eAdolfo changed this group's icon\n"].index
df.drop(df.index[idx2], inplace=True)
```


```python
df
```




<div>
<style>
    .dataframe thead tr:only-child th {
        text-align: right;
    }

    .dataframe thead th {
        text-align: left;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Timestamp</th>
      <th>User</th>
      <th>Message</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>3</th>
      <td>2017-10-07 06:25:35</td>
      <td>Javier</td>
      <td>&lt;‎video omitted&gt;</td>
    </tr>
    <tr>
      <th>4</th>
      <td>2017-10-08 19:37:52</td>
      <td>Kike</td>
      <td>Hey everyone. We had the same in the MBA so I...</td>
    </tr>
    <tr>
      <th>5</th>
      <td>2017-10-08 19:39:05</td>
      <td>Karim</td>
      <td>Good idea 👌</td>
    </tr>
    <tr>
      <th>6</th>
      <td>2017-10-08 19:39:47</td>
      <td>Tobias</td>
      <td>👍</td>
    </tr>
    <tr>
      <th>7</th>
      <td>2017-10-08 19:41:45</td>
      <td>Tobias</td>
      <td>Should we include our linkedIn/Facebook link?</td>
    </tr>
    <tr>
      <th>8</th>
      <td>2017-10-08 19:42:21</td>
      <td>Kike</td>
      <td>Up to you</td>
    </tr>
    <tr>
      <th>9</th>
      <td>2017-10-08 19:52:23</td>
      <td>José C.</td>
      <td>Its share on view only</td>
    </tr>
    <tr>
      <th>10</th>
      <td>2017-10-08 19:52:52</td>
      <td>Kike</td>
      <td>Yeah my bad. Just changed it</td>
    </tr>
    <tr>
      <th>11</th>
      <td>2017-10-08 19:54:24</td>
      <td>José C.</td>
      <td>&lt;‎image omitted&gt;</td>
    </tr>
    <tr>
      <th>12</th>
      <td>2017-10-08 19:54:34</td>
      <td>Pau</td>
      <td>yep</td>
    </tr>
    <tr>
      <th>13</th>
      <td>2017-10-08 19:54:35</td>
      <td>‪+44 7477 122897‬</td>
      <td>&lt;‎image omitted&gt;</td>
    </tr>
    <tr>
      <th>14</th>
      <td>2017-10-08 20:10:33</td>
      <td>Kike</td>
      <td>Good to go now</td>
    </tr>
    <tr>
      <th>15</th>
      <td>2017-10-08 20:12:05</td>
      <td>Javier</td>
      <td>yep</td>
    </tr>
    <tr>
      <th>16</th>
      <td>2017-10-08 20:36:36</td>
      <td>Javier</td>
      <td>el próximo de estos no hay que perdeselo https</td>
    </tr>
    <tr>
      <th>17</th>
      <td>2017-10-08 20:36:39</td>
      <td>Javier</td>
      <td>we should go to the next after brunch</td>
    </tr>
    <tr>
      <th>18</th>
      <td>2017-10-08 20:36:47</td>
      <td>Javier</td>
      <td>looks amazing</td>
    </tr>
    <tr>
      <th>19</th>
      <td>2017-10-08 21:26:16</td>
      <td>Camillo</td>
      <td>Alcohol - always!</td>
    </tr>
    <tr>
      <th>20</th>
      <td>2017-10-08 21:26:27</td>
      <td>Camillo</td>
      <td>You organise it?</td>
    </tr>
    <tr>
      <th>21</th>
      <td>2017-10-08 21:28:53</td>
      <td>Camillo</td>
      <td>I'm just starting my 2nd bottle of wine in pr...</td>
    </tr>
    <tr>
      <th>22</th>
      <td>2017-10-08 21:29:15</td>
      <td>Marina</td>
      <td>Superrr in, tell us when another one is comin...</td>
    </tr>
    <tr>
      <th>23</th>
      <td>2017-10-08 21:30:07</td>
      <td>Javier</td>
      <td>Nah man, I wish!! But on 22nd will be the nex...</td>
    </tr>
    <tr>
      <th>24</th>
      <td>2017-10-08 21:30:23</td>
      <td>Javier</td>
      <td>That’s the spirit! ☝🏼</td>
    </tr>
    <tr>
      <th>25</th>
      <td>2017-10-08 21:30:29</td>
      <td>Camillo</td>
      <td>I mean you let us know when the next one is ;)</td>
    </tr>
    <tr>
      <th>26</th>
      <td>2017-10-08 21:30:46</td>
      <td>Youssef</td>
      <td>Yes it's nice... been doing it for the last 2...</td>
    </tr>
    <tr>
      <th>27</th>
      <td>2017-10-08 21:31:02</td>
      <td>Marina</td>
      <td>Thanks for the invite</td>
    </tr>
    <tr>
      <th>28</th>
      <td>2017-10-08 21:31:03</td>
      <td>Marina</td>
      <td>Hahah</td>
    </tr>
    <tr>
      <th>29</th>
      <td>2017-10-08 21:31:11</td>
      <td>Camillo</td>
      <td>Jajajaja</td>
    </tr>
    <tr>
      <th>30</th>
      <td>2017-10-08 21:31:33</td>
      <td>Javier</td>
      <td>Sure!!</td>
    </tr>
    <tr>
      <th>31</th>
      <td>2017-10-08 21:31:37</td>
      <td>Karim</td>
      <td>&lt;‎image omitted&gt;</td>
    </tr>
    <tr>
      <th>32</th>
      <td>2017-10-08 21:32:41</td>
      <td>Marina</td>
      <td>I'm just starting my first guys, apparently I...</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>11163</th>
      <td>2018-04-10 22:38:33</td>
      <td>Vlada</td>
      <td>FYI</td>
    </tr>
    <tr>
      <th>11164</th>
      <td>2018-04-10 22:40:01</td>
      <td>Maria</td>
      <td>Yes</td>
    </tr>
    <tr>
      <th>11165</th>
      <td>2018-04-10 22:45:32</td>
      <td>Pau</td>
      <td>Auch</td>
    </tr>
    <tr>
      <th>11166</th>
      <td>2018-04-10 22:45:50</td>
      <td>Camillo</td>
      <td>Maria be careful not to hit me with your car ...</td>
    </tr>
    <tr>
      <th>11167</th>
      <td>2018-04-10 22:47:00</td>
      <td>Sharon</td>
      <td>I am marias backup in case she needs it 💪</td>
    </tr>
    <tr>
      <th>11168</th>
      <td>2018-04-10 22:47:50</td>
      <td>Adolfo</td>
      <td>Dont do it maria, let him be kn pain for a wh...</td>
    </tr>
    <tr>
      <th>11169</th>
      <td>2018-04-10 22:47:56</td>
      <td>Camillo</td>
      <td>&lt;‎GIF omitted&gt;</td>
    </tr>
    <tr>
      <th>11170</th>
      <td>2018-04-10 22:49:07</td>
      <td>Sharon</td>
      <td>Afraid much?? Hahaha</td>
    </tr>
    <tr>
      <th>11171</th>
      <td>2018-04-10 22:50:53</td>
      <td>Camillo</td>
      <td>&lt;‎GIF omitted&gt;</td>
    </tr>
    <tr>
      <th>11172</th>
      <td>2018-04-10 22:51:47</td>
      <td>Sharon</td>
      <td>Jajaja so you are so afraid you would rather ...</td>
    </tr>
    <tr>
      <th>11173</th>
      <td>2018-04-10 22:56:35</td>
      <td>Maria</td>
      <td>You’ve got enough for a while, not this week</td>
    </tr>
    <tr>
      <th>11174</th>
      <td>2018-04-10 22:56:48</td>
      <td>Maria</td>
      <td>Up for next week?</td>
    </tr>
    <tr>
      <th>11175</th>
      <td>2018-04-10 22:57:02</td>
      <td>Cecilia</td>
      <td>Hahahahahahahaha</td>
    </tr>
    <tr>
      <th>11176</th>
      <td>2018-04-10 22:57:31</td>
      <td>Sharon</td>
      <td>Always baby!! Hahahahahahahahaha don’t even h...</td>
    </tr>
    <tr>
      <th>11177</th>
      <td>2018-04-10 22:58:25</td>
      <td>Camillo</td>
      <td>&lt;‎GIF omitted&gt;</td>
    </tr>
    <tr>
      <th>11178</th>
      <td>2018-04-10 22:58:43</td>
      <td>Maria</td>
      <td>❤</td>
    </tr>
    <tr>
      <th>11179</th>
      <td>2018-04-10 22:59:33</td>
      <td>Sharon</td>
      <td>&lt;‎GIF omitted&gt;</td>
    </tr>
    <tr>
      <th>11180</th>
      <td>2018-04-10 23:00:47</td>
      <td>Camillo</td>
      <td>&lt;‎video omitted&gt;</td>
    </tr>
    <tr>
      <th>11181</th>
      <td>2018-04-10 23:02:23</td>
      <td>Sharon</td>
      <td>&lt;‎GIF omitted&gt;</td>
    </tr>
    <tr>
      <th>11182</th>
      <td>2018-04-10 23:02:38</td>
      <td>Camillo</td>
      <td>&lt;‎GIF omitted&gt;</td>
    </tr>
    <tr>
      <th>11183</th>
      <td>2018-04-10 23:03:01</td>
      <td>Camillo</td>
      <td>Or drop you?</td>
    </tr>
    <tr>
      <th>11184</th>
      <td>2018-04-10 23:03:21</td>
      <td>Sharon</td>
      <td>I’m too valuable for you ... you would never ...</td>
    </tr>
    <tr>
      <th>11185</th>
      <td>2018-04-10 23:03:40</td>
      <td>Camillo</td>
      <td>💝</td>
    </tr>
    <tr>
      <th>11186</th>
      <td>2018-04-10 23:05:53</td>
      <td>Angela</td>
      <td>Please keep your couple fight private 😪</td>
    </tr>
    <tr>
      <th>11187</th>
      <td>2018-04-10 23:06:10</td>
      <td>Maria</td>
      <td>😂😂😂😂</td>
    </tr>
    <tr>
      <th>11188</th>
      <td>2018-04-10 23:06:33</td>
      <td>Karim</td>
      <td>🙋🏻‍♂🙋🏻‍♂</td>
    </tr>
    <tr>
      <th>11189</th>
      <td>2018-04-10 23:07:26</td>
      <td>Sharon</td>
      <td>Come on! You enjoy watching us fight 🙄 haha</td>
    </tr>
    <tr>
      <th>11190</th>
      <td>2018-04-10 23:07:35</td>
      <td>Camillo</td>
      <td>Yeah you’re right. Sorry.</td>
    </tr>
    <tr>
      <th>11191</th>
      <td>2018-04-10 23:08:04</td>
      <td>Camillo</td>
      <td>See! She never apologises 😒</td>
    </tr>
    <tr>
      <th>11192</th>
      <td>2018-04-10 23:08:26</td>
      <td>Sharon</td>
      <td>You learnt the words man</td>
    </tr>
  </tbody>
</table>
<p>11159 rows × 3 columns</p>
</div>




```python
df.User.unique()
```




    array(['Javier', 'Kike', 'Karim', 'Tobias', 'José C.', 'Pau',
           '\u202a+44\xa07477\xa0122897\u202c', 'Camillo', 'Marina', 'Youssef',
           'Hani', 'Rui', 'Mustafa', 'Adolfo', 'VJ', 'Max', 'Théo', 'Ahsan',
           'Cecilia', 'Angel', 'Hernando', 'Ilianna', 'Tania', 'Sharon',
           'Michellé', 'Angela', 'Dan', 'Vlada', 'Amartya', 'Tommy',
           'Charlotte', 'Oscar', 'Faisal', 'Mateusz', 'Nacho', 'Theresa',
           'Mike', 'Aninda', 'Deodatta', 'Tomasso', 'Mohamed', 'Esteban',
           'Rudy', 'Jose S.', 'Filippo', 'Resham', 'Jorge A.', 'Sina',
           'Shelly', 'Andrea', 'Hlieb', 'Nikita', 'Roland', 'Sanjay', 'Maria',
           'Milo', "\u200eAdolfo changed this group's icon\n"], dtype=object)





