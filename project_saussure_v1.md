
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
      <th>0</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>29/09/2017, 19:21:48: ‎Messages to this group ...</td>
    </tr>
    <tr>
      <th>1</th>
      <td>29/09/2017, 19:21:48: ‎Kike created this group</td>
    </tr>
    <tr>
      <th>2</th>
      <td>07/10/2017, 00:41:05: ‎Kike added you</td>
    </tr>
    <tr>
      <th>3</th>
      <td>07/10/2017, 06:25:35: Javier: &lt;‎video omitted&gt;</td>
    </tr>
    <tr>
      <th>4</th>
      <td>08/10/2017, 19:37:52: Kike: Hey everyone. We h...</td>
    </tr>
    <tr>
      <th>5</th>
      <td></td>
    </tr>
    <tr>
      <th>6</th>
      <td>https://docs.google.com/a/student.ie.edu/sprea...</td>
    </tr>
    <tr>
      <th>7</th>
      <td>08/10/2017, 19:39:05: Karim: Good idea 👌</td>
    </tr>
    <tr>
      <th>8</th>
      <td>08/10/2017, 19:39:47: Tobias: 👍</td>
    </tr>
    <tr>
      <th>9</th>
      <td>08/10/2017, 19:41:45: Tobias: Should we includ...</td>
    </tr>
    <tr>
      <th>10</th>
      <td>08/10/2017, 19:42:21: Kike: Up to you</td>
    </tr>
    <tr>
      <th>11</th>
      <td>08/10/2017, 19:52:23: José C.: Its share on vi...</td>
    </tr>
    <tr>
      <th>12</th>
      <td>08/10/2017, 19:52:52: Kike: Yeah my bad. Just ...</td>
    </tr>
    <tr>
      <th>13</th>
      <td>08/10/2017, 19:54:24: José C.: &lt;‎image omitted&gt;</td>
    </tr>
    <tr>
      <th>14</th>
      <td>08/10/2017, 19:54:34: Pau: yep</td>
    </tr>
    <tr>
      <th>15</th>
      <td>08/10/2017, 19:54:35: ‪+44 7477 122897‬: &lt;‎ima...</td>
    </tr>
    <tr>
      <th>16</th>
      <td>08/10/2017, 20:10:33: Kike: Good to go now</td>
    </tr>
    <tr>
      <th>17</th>
      <td>08/10/2017, 20:12:05: Javier: yep</td>
    </tr>
    <tr>
      <th>18</th>
      <td>08/10/2017, 20:36:36: Javier: el próximo de es...</td>
    </tr>
    <tr>
      <th>19</th>
      <td>08/10/2017, 20:36:39: Javier: we should go to ...</td>
    </tr>
    <tr>
      <th>20</th>
      <td>08/10/2017, 20:36:47: Javier: looks amazing</td>
    </tr>
    <tr>
      <th>21</th>
      <td>08/10/2017, 21:26:16: Camillo: Alcohol - always!</td>
    </tr>
    <tr>
      <th>22</th>
      <td>08/10/2017, 21:26:27: Camillo: You organise it?</td>
    </tr>
    <tr>
      <th>23</th>
      <td>08/10/2017, 21:28:53: Camillo: I'm just starti...</td>
    </tr>
    <tr>
      <th>24</th>
      <td>08/10/2017, 21:29:15: Marina: Superrr in, tell...</td>
    </tr>
    <tr>
      <th>25</th>
      <td>08/10/2017, 21:30:07: Javier: Nah man, I wish!...</td>
    </tr>
    <tr>
      <th>26</th>
      <td>08/10/2017, 21:30:23: Javier: That’s the spiri...</td>
    </tr>
    <tr>
      <th>27</th>
      <td>08/10/2017, 21:30:29: Camillo: I mean you let ...</td>
    </tr>
    <tr>
      <th>28</th>
      <td>08/10/2017, 21:30:46: Youssef: Yes it's nice.....</td>
    </tr>
    <tr>
      <th>29</th>
      <td>08/10/2017, 21:31:02: Marina: Thanks for the i...</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
    </tr>
    <tr>
      <th>15742</th>
      <td>10/04/2018, 22:38:33: Vlada: FYI: Spark grades...</td>
    </tr>
    <tr>
      <th>15743</th>
      <td>10/04/2018, 22:40:01: Maria: Yes</td>
    </tr>
    <tr>
      <th>15744</th>
      <td>10/04/2018, 22:45:32: Pau: Auch</td>
    </tr>
    <tr>
      <th>15745</th>
      <td>10/04/2018, 22:45:50: Camillo: Maria be carefu...</td>
    </tr>
    <tr>
      <th>15746</th>
      <td>10/04/2018, 22:47:00: Sharon: I am marias back...</td>
    </tr>
    <tr>
      <th>15747</th>
      <td>10/04/2018, 22:47:50: Adolfo: Dont do it maria...</td>
    </tr>
    <tr>
      <th>15748</th>
      <td>10/04/2018, 22:47:56: Camillo: &lt;‎GIF omitted&gt;</td>
    </tr>
    <tr>
      <th>15749</th>
      <td>10/04/2018, 22:49:07: Sharon: Afraid much?? Ha...</td>
    </tr>
    <tr>
      <th>15750</th>
      <td>10/04/2018, 22:50:53: Camillo: &lt;‎GIF omitted&gt;</td>
    </tr>
    <tr>
      <th>15751</th>
      <td>10/04/2018, 22:51:47: Sharon: Jajaja so you ar...</td>
    </tr>
    <tr>
      <th>15752</th>
      <td>10/04/2018, 22:56:35: Maria: You’ve got enough...</td>
    </tr>
    <tr>
      <th>15753</th>
      <td>10/04/2018, 22:56:48: Maria: Up for next week?</td>
    </tr>
    <tr>
      <th>15754</th>
      <td>10/04/2018, 22:57:02: Cecilia: Hahahahahahahaha</td>
    </tr>
    <tr>
      <th>15755</th>
      <td>10/04/2018, 22:57:31: Sharon: Always baby!! Ha...</td>
    </tr>
    <tr>
      <th>15756</th>
      <td>10/04/2018, 22:58:25: Camillo: &lt;‎GIF omitted&gt;</td>
    </tr>
    <tr>
      <th>15757</th>
      <td>10/04/2018, 22:58:43: Maria: ❤</td>
    </tr>
    <tr>
      <th>15758</th>
      <td>10/04/2018, 22:59:33: Sharon: &lt;‎GIF omitted&gt;</td>
    </tr>
    <tr>
      <th>15759</th>
      <td>10/04/2018, 23:00:47: Camillo: &lt;‎video omitted&gt;</td>
    </tr>
    <tr>
      <th>15760</th>
      <td>10/04/2018, 23:02:23: Sharon: &lt;‎GIF omitted&gt;</td>
    </tr>
    <tr>
      <th>15761</th>
      <td>10/04/2018, 23:02:38: Camillo: &lt;‎GIF omitted&gt;</td>
    </tr>
    <tr>
      <th>15762</th>
      <td>10/04/2018, 23:03:01: Camillo: Or drop you?</td>
    </tr>
    <tr>
      <th>15763</th>
      <td>10/04/2018, 23:03:21: Sharon: I’m too valuable...</td>
    </tr>
    <tr>
      <th>15764</th>
      <td>10/04/2018, 23:03:40: Camillo: 💝</td>
    </tr>
    <tr>
      <th>15765</th>
      <td>10/04/2018, 23:05:53: Angela: Please keep your...</td>
    </tr>
    <tr>
      <th>15766</th>
      <td>10/04/2018, 23:06:10: Maria: 😂😂😂😂</td>
    </tr>
    <tr>
      <th>15767</th>
      <td>10/04/2018, 23:06:33: Karim: 🙋🏻‍♂🙋🏻‍♂</td>
    </tr>
    <tr>
      <th>15768</th>
      <td>10/04/2018, 23:07:26: Sharon: Come on! You enj...</td>
    </tr>
    <tr>
      <th>15769</th>
      <td>10/04/2018, 23:07:35: Camillo: Yeah you’re rig...</td>
    </tr>
    <tr>
      <th>15770</th>
      <td>10/04/2018, 23:08:04: Camillo: See! She never ...</td>
    </tr>
    <tr>
      <th>15771</th>
      <td>10/04/2018, 23:08:26: Sharon: You learnt the w...</td>
    </tr>
  </tbody>
</table>
<p>15772 rows × 1 columns</p>
</div>


