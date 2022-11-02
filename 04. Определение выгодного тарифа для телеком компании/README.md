# Описание проекта
Вы аналитик компании «Мегалайн» — федерального оператора сотовой связи. Клиентам предлагают два тарифных плана: «Смарт» и «Ультра». Чтобы скорректировать рекламный бюджет, коммерческий департамент хочет понять, какой тариф приносит больше денег.
Вам предстоит сделать предварительный анализ тарифов на небольшой выборке клиентов. В вашем распоряжении данные 500 пользователей «Мегалайна»: кто они, откуда, каким тарифом пользуются, сколько звонков и сообщений каждый отправил за 2018-й год. Нужно проанализировать поведение клиентов и сделать вывод — какой тариф лучше.

### Откройте файл с данными и изучите общую информацию


```python
import pandas as pd
```

**Задание 1.** Откройте файл `/datasets/calls.csv`, сохраните датафрейм в переменную `calls`.


```python
calls = pd.read_csv('/datasets/calls.csv')
```

**Задание 2.** Выведите первые 5 строк датафрейма `calls`.


```python
calls.head()
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
      <th>id</th>
      <th>call_date</th>
      <th>duration</th>
      <th>user_id</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>1000_0</td>
      <td>2018-07-25</td>
      <td>0.00</td>
      <td>1000</td>
    </tr>
    <tr>
      <th>1</th>
      <td>1000_1</td>
      <td>2018-08-17</td>
      <td>0.00</td>
      <td>1000</td>
    </tr>
    <tr>
      <th>2</th>
      <td>1000_2</td>
      <td>2018-06-11</td>
      <td>2.85</td>
      <td>1000</td>
    </tr>
    <tr>
      <th>3</th>
      <td>1000_3</td>
      <td>2018-09-21</td>
      <td>13.80</td>
      <td>1000</td>
    </tr>
    <tr>
      <th>4</th>
      <td>1000_4</td>
      <td>2018-12-15</td>
      <td>5.18</td>
      <td>1000</td>
    </tr>
  </tbody>
</table>
</div>



**Задание 3.** Выведите основную информацию для датафрейма `calls` с помощью метода `info()`.


```python
calls.info()
```

    <class 'pandas.core.frame.DataFrame'>
    RangeIndex: 202607 entries, 0 to 202606
    Data columns (total 4 columns):
     #   Column     Non-Null Count   Dtype  
    ---  ------     --------------   -----  
     0   id         202607 non-null  object 
     1   call_date  202607 non-null  object 
     2   duration   202607 non-null  float64
     3   user_id    202607 non-null  int64  
    dtypes: float64(1), int64(1), object(2)
    memory usage: 6.2+ MB


**Задание 4.** С помощью метода `hist()` выведите гистограмму для столбца с продолжительностью звонков. Подумайте о том, как распределены данные.


```python
calls['duration'].hist()
```




    <AxesSubplot:>




    
![png](output_10_1.png)
    


**Задание 5.** Откройте файл `/datasets/internet.csv`, сохраните датафрейм в переменную `sessions`.


```python
sessions = pd.read_csv('/datasets/internet.csv')
```

**Задание 6.** Выведите первые 5 строк датафрейма `sessions`.


```python
sessions.head()
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
      <th>Unnamed: 0</th>
      <th>id</th>
      <th>mb_used</th>
      <th>session_date</th>
      <th>user_id</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>0</td>
      <td>1000_0</td>
      <td>112.95</td>
      <td>2018-11-25</td>
      <td>1000</td>
    </tr>
    <tr>
      <th>1</th>
      <td>1</td>
      <td>1000_1</td>
      <td>1052.81</td>
      <td>2018-09-07</td>
      <td>1000</td>
    </tr>
    <tr>
      <th>2</th>
      <td>2</td>
      <td>1000_2</td>
      <td>1197.26</td>
      <td>2018-06-25</td>
      <td>1000</td>
    </tr>
    <tr>
      <th>3</th>
      <td>3</td>
      <td>1000_3</td>
      <td>550.27</td>
      <td>2018-08-22</td>
      <td>1000</td>
    </tr>
    <tr>
      <th>4</th>
      <td>4</td>
      <td>1000_4</td>
      <td>302.56</td>
      <td>2018-09-24</td>
      <td>1000</td>
    </tr>
  </tbody>
</table>
</div>



**Задание 7.** Выведите основную информацию для датафрейма sessions с помощью метода `info()`. 


```python
sessions.info()
```

    <class 'pandas.core.frame.DataFrame'>
    RangeIndex: 149396 entries, 0 to 149395
    Data columns (total 5 columns):
     #   Column        Non-Null Count   Dtype  
    ---  ------        --------------   -----  
     0   Unnamed: 0    149396 non-null  int64  
     1   id            149396 non-null  object 
     2   mb_used       149396 non-null  float64
     3   session_date  149396 non-null  object 
     4   user_id       149396 non-null  int64  
    dtypes: float64(1), int64(2), object(2)
    memory usage: 5.7+ MB


**Задание 8.** С помощью метода `hist()` выведите гистограмму для столбца с количеством потраченных мегабайт.


```python
sessions['mb_used'].hist()
```




    <AxesSubplot:>




    
![png](output_18_1.png)
    


**Задание 9.** Откройте файл `/datasets/messages.csv`, сохраните датафрейм в переменную `messages`.


```python
messages = pd.read_csv('/datasets/messages.csv')
```

**Задание 10.** Выведите первые 5 строк датафрейма `messages`.


```python
messages.head()
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
      <th>id</th>
      <th>message_date</th>
      <th>user_id</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>1000_0</td>
      <td>2018-06-27</td>
      <td>1000</td>
    </tr>
    <tr>
      <th>1</th>
      <td>1000_1</td>
      <td>2018-10-08</td>
      <td>1000</td>
    </tr>
    <tr>
      <th>2</th>
      <td>1000_2</td>
      <td>2018-08-04</td>
      <td>1000</td>
    </tr>
    <tr>
      <th>3</th>
      <td>1000_3</td>
      <td>2018-06-16</td>
      <td>1000</td>
    </tr>
    <tr>
      <th>4</th>
      <td>1000_4</td>
      <td>2018-12-05</td>
      <td>1000</td>
    </tr>
  </tbody>
</table>
</div>



**Задание 11.** Выведите основную информацию для датафрейма `messages` с помощью метода `info()`. 


```python
messages.info()
```

    <class 'pandas.core.frame.DataFrame'>
    RangeIndex: 123036 entries, 0 to 123035
    Data columns (total 3 columns):
     #   Column        Non-Null Count   Dtype 
    ---  ------        --------------   ----- 
     0   id            123036 non-null  object
     1   message_date  123036 non-null  object
     2   user_id       123036 non-null  int64 
    dtypes: int64(1), object(2)
    memory usage: 2.8+ MB


**Задание 12.** Откройте файл `/datasets/tariffs.csv`, сохраните датафрейм в переменную `tariffs`.


```python
tariffs = pd.read_csv('/datasets/tariffs.csv')
```

**Задание 13.** Выведите весь датафрейм `tariffs`.


```python
tariffs
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
      <th>messages_included</th>
      <th>mb_per_month_included</th>
      <th>minutes_included</th>
      <th>rub_monthly_fee</th>
      <th>rub_per_gb</th>
      <th>rub_per_message</th>
      <th>rub_per_minute</th>
      <th>tariff_name</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>50</td>
      <td>15360</td>
      <td>500</td>
      <td>550</td>
      <td>200</td>
      <td>3</td>
      <td>3</td>
      <td>smart</td>
    </tr>
    <tr>
      <th>1</th>
      <td>1000</td>
      <td>30720</td>
      <td>3000</td>
      <td>1950</td>
      <td>150</td>
      <td>1</td>
      <td>1</td>
      <td>ultra</td>
    </tr>
  </tbody>
</table>
</div>



**Задание 14.** Выведите основную информацию для датафрейма `tariffs` с помощью метода `info()`.


```python
tariffs.info()
```

    <class 'pandas.core.frame.DataFrame'>
    RangeIndex: 2 entries, 0 to 1
    Data columns (total 8 columns):
     #   Column                 Non-Null Count  Dtype 
    ---  ------                 --------------  ----- 
     0   messages_included      2 non-null      int64 
     1   mb_per_month_included  2 non-null      int64 
     2   minutes_included       2 non-null      int64 
     3   rub_monthly_fee        2 non-null      int64 
     4   rub_per_gb             2 non-null      int64 
     5   rub_per_message        2 non-null      int64 
     6   rub_per_minute         2 non-null      int64 
     7   tariff_name            2 non-null      object
    dtypes: int64(7), object(1)
    memory usage: 256.0+ bytes


**Задание 15.** Откройте файл `/datasets/users.csv`, сохраните датафрейм в переменную `users`.


```python
users = pd.read_csv('/datasets/users.csv')
```

**Задание 16.** Выведите первые 5 строк датафрейма `users`.


```python
users.head()
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
      <th>user_id</th>
      <th>age</th>
      <th>churn_date</th>
      <th>city</th>
      <th>first_name</th>
      <th>last_name</th>
      <th>reg_date</th>
      <th>tariff</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>1000</td>
      <td>52</td>
      <td>NaN</td>
      <td>Краснодар</td>
      <td>Рафаил</td>
      <td>Верещагин</td>
      <td>2018-05-25</td>
      <td>ultra</td>
    </tr>
    <tr>
      <th>1</th>
      <td>1001</td>
      <td>41</td>
      <td>NaN</td>
      <td>Москва</td>
      <td>Иван</td>
      <td>Ежов</td>
      <td>2018-11-01</td>
      <td>smart</td>
    </tr>
    <tr>
      <th>2</th>
      <td>1002</td>
      <td>59</td>
      <td>NaN</td>
      <td>Стерлитамак</td>
      <td>Евгений</td>
      <td>Абрамович</td>
      <td>2018-06-17</td>
      <td>smart</td>
    </tr>
    <tr>
      <th>3</th>
      <td>1003</td>
      <td>23</td>
      <td>NaN</td>
      <td>Москва</td>
      <td>Белла</td>
      <td>Белякова</td>
      <td>2018-08-17</td>
      <td>ultra</td>
    </tr>
    <tr>
      <th>4</th>
      <td>1004</td>
      <td>68</td>
      <td>NaN</td>
      <td>Новокузнецк</td>
      <td>Татьяна</td>
      <td>Авдеенко</td>
      <td>2018-05-14</td>
      <td>ultra</td>
    </tr>
  </tbody>
</table>
</div>



**Задание 17.** Выведите основную информацию для датафрейма `users` с помощью метода `info()`.


```python
users.info()
```

    <class 'pandas.core.frame.DataFrame'>
    RangeIndex: 500 entries, 0 to 499
    Data columns (total 8 columns):
     #   Column      Non-Null Count  Dtype 
    ---  ------      --------------  ----- 
     0   user_id     500 non-null    int64 
     1   age         500 non-null    int64 
     2   churn_date  38 non-null     object
     3   city        500 non-null    object
     4   first_name  500 non-null    object
     5   last_name   500 non-null    object
     6   reg_date    500 non-null    object
     7   tariff      500 non-null    object
    dtypes: int64(2), object(6)
    memory usage: 31.4+ KB


### Подготовьте данные

**Задание 18.**  Приведите столбцы

- `reg_date` из таблицы `users`
- `churn_date` из таблицы `users`
- `call_date` из таблицы `calls`
- `message_date` из таблицы `messages`
- `session_date` из таблицы `sessions`

к новому типу с помощью метода `to_datetime()`.


```python
# обработка столбца reg_date
# обработка столбца churn_date
users['reg_date'] = pd.to_datetime(users['reg_date'], format='%Y-%m-%d')
users['churn_date'] = pd.to_datetime(users['churn_date'], format='%Y-%m-%d')
# обработка столбца call_date
calls['call_date'] = pd.to_datetime(calls['call_date'], format='%Y-%m-%d')
# обработка столбца message_date
# обработка столбца session_date
messages['message_date'] = pd.to_datetime(messages['message_date'], format='%Y-%m-%d')
sessions['session_date'] = pd.to_datetime(sessions['session_date'], format='%Y-%m-%d')

```

**Задание 19.** В данных вы найдёте звонки с нулевой продолжительностью. Это не ошибка: нулями обозначены пропущенные звонки, поэтому их не нужно удалять.

Однако в столбце `duration` датафрейма `calls` значения дробные. Округлите значения столбца `duration` вверх с помощью метода `numpy.ceil()` и приведите столбец `duration` к типу `int`.


```python
import numpy as np

# округление значений столбца duration с помощью np.ceil() и приведение типа к int
calls['duration'] = np.ceil(calls['duration']).astype('int')

```

**Задание 20.** Удалите столбец `Unnamed: 0` из датафрейма `sessions`. Столбец с таким названием возникает, когда данные сохраняют с указанием индекса (`df.to_csv(..., index=column)`). Он сейчас не понадобится.


```python
sessions = sessions.drop(columns='Unnamed: 0')
```

**Задание 21.** Создайте столбец `month` в датафрейме `calls` с номером месяца из столбца `call_date`.


```python
calls['month'] = calls['call_date'].dt.month
```

**Задание 22.** Создайте столбец `month` в датафрейме `messages` с номером месяца из столбца `message_date`.


```python
messages['month'] = messages['message_date'].dt.month
```

**Задание 23.** Создайте столбец `month` в датафрейме `sessions` с номером месяца из столбца `session_date`.


```python
sessions['month'] = sessions['session_date'].dt.month
```

**Задание 24.** Посчитайте количество сделанных звонков разговора для каждого пользователя по месяцам и сохраните в переменную `calls_per_month`.


```python
calls_per_month = calls.groupby(['user_id', 'month']).agg(calls=('duration', 'count'))
# подсчёт количества звонков для каждого пользователя по месяцам
```


```python
# вывод 30 первых строк на экран
calls_per_month.head(30)
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
      <th></th>
      <th>calls</th>
    </tr>
    <tr>
      <th>user_id</th>
      <th>month</th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th rowspan="8" valign="top">1000</th>
      <th>5</th>
      <td>22</td>
    </tr>
    <tr>
      <th>6</th>
      <td>43</td>
    </tr>
    <tr>
      <th>7</th>
      <td>47</td>
    </tr>
    <tr>
      <th>8</th>
      <td>52</td>
    </tr>
    <tr>
      <th>9</th>
      <td>58</td>
    </tr>
    <tr>
      <th>10</th>
      <td>57</td>
    </tr>
    <tr>
      <th>11</th>
      <td>43</td>
    </tr>
    <tr>
      <th>12</th>
      <td>46</td>
    </tr>
    <tr>
      <th rowspan="2" valign="top">1001</th>
      <th>11</th>
      <td>59</td>
    </tr>
    <tr>
      <th>12</th>
      <td>63</td>
    </tr>
    <tr>
      <th rowspan="7" valign="top">1002</th>
      <th>6</th>
      <td>15</td>
    </tr>
    <tr>
      <th>7</th>
      <td>26</td>
    </tr>
    <tr>
      <th>8</th>
      <td>42</td>
    </tr>
    <tr>
      <th>9</th>
      <td>36</td>
    </tr>
    <tr>
      <th>10</th>
      <td>33</td>
    </tr>
    <tr>
      <th>11</th>
      <td>32</td>
    </tr>
    <tr>
      <th>12</th>
      <td>33</td>
    </tr>
    <tr>
      <th rowspan="5" valign="top">1003</th>
      <th>8</th>
      <td>55</td>
    </tr>
    <tr>
      <th>9</th>
      <td>134</td>
    </tr>
    <tr>
      <th>10</th>
      <td>108</td>
    </tr>
    <tr>
      <th>11</th>
      <td>115</td>
    </tr>
    <tr>
      <th>12</th>
      <td>108</td>
    </tr>
    <tr>
      <th rowspan="8" valign="top">1004</th>
      <th>5</th>
      <td>9</td>
    </tr>
    <tr>
      <th>6</th>
      <td>31</td>
    </tr>
    <tr>
      <th>7</th>
      <td>22</td>
    </tr>
    <tr>
      <th>8</th>
      <td>19</td>
    </tr>
    <tr>
      <th>9</th>
      <td>26</td>
    </tr>
    <tr>
      <th>10</th>
      <td>29</td>
    </tr>
    <tr>
      <th>11</th>
      <td>19</td>
    </tr>
    <tr>
      <th>12</th>
      <td>21</td>
    </tr>
  </tbody>
</table>
</div>



**Задание 25.** Посчитайте количество израсходованных минут разговора для каждого пользователя по месяцам и сохраните в переменную `minutes_per_month`. Вам понадобится

- сгруппировать датафрейм с информацией о звонках по двум столбцам — с идентификаторами пользователей и номерами месяцев;
- после группировки выбрать столбец `duration`
- затем применить метод для подсчёта суммы.

Выведите первые 30 строчек `minutes_per_month`.


```python
minutes_per_month = calls.groupby(['user_id', 'month']).agg(minutes=('duration', 'sum'))
# подсчёт израсходованных минут для каждого пользователя по месяцам
```


```python
# вывод первых 30 строк на экран
minutes_per_month.head(30)
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
      <th></th>
      <th>minutes</th>
    </tr>
    <tr>
      <th>user_id</th>
      <th>month</th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th rowspan="8" valign="top">1000</th>
      <th>5</th>
      <td>159</td>
    </tr>
    <tr>
      <th>6</th>
      <td>172</td>
    </tr>
    <tr>
      <th>7</th>
      <td>340</td>
    </tr>
    <tr>
      <th>8</th>
      <td>408</td>
    </tr>
    <tr>
      <th>9</th>
      <td>466</td>
    </tr>
    <tr>
      <th>10</th>
      <td>350</td>
    </tr>
    <tr>
      <th>11</th>
      <td>338</td>
    </tr>
    <tr>
      <th>12</th>
      <td>333</td>
    </tr>
    <tr>
      <th rowspan="2" valign="top">1001</th>
      <th>11</th>
      <td>430</td>
    </tr>
    <tr>
      <th>12</th>
      <td>414</td>
    </tr>
    <tr>
      <th rowspan="7" valign="top">1002</th>
      <th>6</th>
      <td>117</td>
    </tr>
    <tr>
      <th>7</th>
      <td>214</td>
    </tr>
    <tr>
      <th>8</th>
      <td>289</td>
    </tr>
    <tr>
      <th>9</th>
      <td>206</td>
    </tr>
    <tr>
      <th>10</th>
      <td>212</td>
    </tr>
    <tr>
      <th>11</th>
      <td>243</td>
    </tr>
    <tr>
      <th>12</th>
      <td>236</td>
    </tr>
    <tr>
      <th rowspan="5" valign="top">1003</th>
      <th>8</th>
      <td>380</td>
    </tr>
    <tr>
      <th>9</th>
      <td>961</td>
    </tr>
    <tr>
      <th>10</th>
      <td>855</td>
    </tr>
    <tr>
      <th>11</th>
      <td>824</td>
    </tr>
    <tr>
      <th>12</th>
      <td>802</td>
    </tr>
    <tr>
      <th rowspan="8" valign="top">1004</th>
      <th>5</th>
      <td>35</td>
    </tr>
    <tr>
      <th>6</th>
      <td>171</td>
    </tr>
    <tr>
      <th>7</th>
      <td>135</td>
    </tr>
    <tr>
      <th>8</th>
      <td>137</td>
    </tr>
    <tr>
      <th>9</th>
      <td>117</td>
    </tr>
    <tr>
      <th>10</th>
      <td>145</td>
    </tr>
    <tr>
      <th>11</th>
      <td>117</td>
    </tr>
    <tr>
      <th>12</th>
      <td>130</td>
    </tr>
  </tbody>
</table>
</div>



**Задание 26.** Посчитайте количество отправленных сообщений по месяцам для каждого пользователя и сохраните в переменную `messages_per_month`. Вам понадобится

- сгруппировать датафрейм с информацией о сообщениях по двум столбцам — с идентификаторами пользователей и номерами месяцев;
- после группировки выбрать столбец `message_date`;
- затем применить метод для подсчёта количества.

Выведите первые 30 строчек `messages_per_month`.


```python
messages_per_month = messages.groupby(['user_id', 'month']).agg(messages=('message_date', 'count'))
# подсчёт количества отправленных сообщений для каждого пользователя по месяцам
```


```python
# вывод первых 30 строк на экран
messages_per_month.head(30)
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
      <th></th>
      <th>messages</th>
    </tr>
    <tr>
      <th>user_id</th>
      <th>month</th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th rowspan="8" valign="top">1000</th>
      <th>5</th>
      <td>22</td>
    </tr>
    <tr>
      <th>6</th>
      <td>60</td>
    </tr>
    <tr>
      <th>7</th>
      <td>75</td>
    </tr>
    <tr>
      <th>8</th>
      <td>81</td>
    </tr>
    <tr>
      <th>9</th>
      <td>57</td>
    </tr>
    <tr>
      <th>10</th>
      <td>73</td>
    </tr>
    <tr>
      <th>11</th>
      <td>58</td>
    </tr>
    <tr>
      <th>12</th>
      <td>70</td>
    </tr>
    <tr>
      <th rowspan="7" valign="top">1002</th>
      <th>6</th>
      <td>4</td>
    </tr>
    <tr>
      <th>7</th>
      <td>11</td>
    </tr>
    <tr>
      <th>8</th>
      <td>13</td>
    </tr>
    <tr>
      <th>9</th>
      <td>4</td>
    </tr>
    <tr>
      <th>10</th>
      <td>10</td>
    </tr>
    <tr>
      <th>11</th>
      <td>16</td>
    </tr>
    <tr>
      <th>12</th>
      <td>12</td>
    </tr>
    <tr>
      <th rowspan="5" valign="top">1003</th>
      <th>8</th>
      <td>37</td>
    </tr>
    <tr>
      <th>9</th>
      <td>91</td>
    </tr>
    <tr>
      <th>10</th>
      <td>83</td>
    </tr>
    <tr>
      <th>11</th>
      <td>94</td>
    </tr>
    <tr>
      <th>12</th>
      <td>75</td>
    </tr>
    <tr>
      <th rowspan="8" valign="top">1004</th>
      <th>5</th>
      <td>95</td>
    </tr>
    <tr>
      <th>6</th>
      <td>134</td>
    </tr>
    <tr>
      <th>7</th>
      <td>181</td>
    </tr>
    <tr>
      <th>8</th>
      <td>151</td>
    </tr>
    <tr>
      <th>9</th>
      <td>146</td>
    </tr>
    <tr>
      <th>10</th>
      <td>165</td>
    </tr>
    <tr>
      <th>11</th>
      <td>158</td>
    </tr>
    <tr>
      <th>12</th>
      <td>162</td>
    </tr>
    <tr>
      <th rowspan="2" valign="top">1005</th>
      <th>1</th>
      <td>7</td>
    </tr>
    <tr>
      <th>2</th>
      <td>38</td>
    </tr>
  </tbody>
</table>
</div>



**Задание 27.** Посчитайте количество потраченных мегабайт по месяцам для каждого пользователя и сохраните в переменную `sessions_per_month`. Вам понадобится

- сгруппировать датафрейм с информацией о сообщениях по двум столбцам — с идентификаторами пользователей и номерами месяцев;
- затем применить метод для подсчёта суммы: `.agg({'mb_used': 'sum'})`


```python
sessions_per_month = sessions.groupby(['user_id', 'month']).agg({'mb_used': 'sum'})
# подсчёт потраченных мегабайт для каждого пользователя по месяцам
```


```python
sessions_per_month.head(30)
# вывод первых 30 строк на экран
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
      <th></th>
      <th>mb_used</th>
    </tr>
    <tr>
      <th>user_id</th>
      <th>month</th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th rowspan="8" valign="top">1000</th>
      <th>5</th>
      <td>2253.49</td>
    </tr>
    <tr>
      <th>6</th>
      <td>23233.77</td>
    </tr>
    <tr>
      <th>7</th>
      <td>14003.64</td>
    </tr>
    <tr>
      <th>8</th>
      <td>14055.93</td>
    </tr>
    <tr>
      <th>9</th>
      <td>14568.91</td>
    </tr>
    <tr>
      <th>10</th>
      <td>14702.49</td>
    </tr>
    <tr>
      <th>11</th>
      <td>14756.47</td>
    </tr>
    <tr>
      <th>12</th>
      <td>9817.61</td>
    </tr>
    <tr>
      <th rowspan="2" valign="top">1001</th>
      <th>11</th>
      <td>18429.34</td>
    </tr>
    <tr>
      <th>12</th>
      <td>14036.66</td>
    </tr>
    <tr>
      <th rowspan="7" valign="top">1002</th>
      <th>6</th>
      <td>10856.82</td>
    </tr>
    <tr>
      <th>7</th>
      <td>17580.10</td>
    </tr>
    <tr>
      <th>8</th>
      <td>20319.26</td>
    </tr>
    <tr>
      <th>9</th>
      <td>16691.08</td>
    </tr>
    <tr>
      <th>10</th>
      <td>13888.25</td>
    </tr>
    <tr>
      <th>11</th>
      <td>18587.28</td>
    </tr>
    <tr>
      <th>12</th>
      <td>18113.73</td>
    </tr>
    <tr>
      <th rowspan="5" valign="top">1003</th>
      <th>8</th>
      <td>8565.21</td>
    </tr>
    <tr>
      <th>9</th>
      <td>12468.87</td>
    </tr>
    <tr>
      <th>10</th>
      <td>14768.14</td>
    </tr>
    <tr>
      <th>11</th>
      <td>11356.89</td>
    </tr>
    <tr>
      <th>12</th>
      <td>10121.53</td>
    </tr>
    <tr>
      <th rowspan="8" valign="top">1004</th>
      <th>5</th>
      <td>13403.98</td>
    </tr>
    <tr>
      <th>6</th>
      <td>17600.02</td>
    </tr>
    <tr>
      <th>7</th>
      <td>22229.58</td>
    </tr>
    <tr>
      <th>8</th>
      <td>28584.37</td>
    </tr>
    <tr>
      <th>9</th>
      <td>15109.03</td>
    </tr>
    <tr>
      <th>10</th>
      <td>18475.44</td>
    </tr>
    <tr>
      <th>11</th>
      <td>15616.02</td>
    </tr>
    <tr>
      <th>12</th>
      <td>18021.04</td>
    </tr>
  </tbody>
</table>
</div>



### Анализ данных и подсчёт выручки

Объединяем все посчитанные выше значения в один датафрейм `user_behavior`.
Для каждой пары "пользователь - месяц" будут доступны информация о тарифе, количестве звонков, сообщений и потраченных мегабайтах.


```python
users['churn_date'].count() / users['churn_date'].shape[0] * 100
```




    7.6



Расторгли договор 7.6% клиентов из датасета


```python
user_behavior = calls_per_month\
    .merge(messages_per_month, left_index=True, right_index=True, how='outer')\
    .merge(sessions_per_month, left_index=True, right_index=True, how='outer')\
    .merge(minutes_per_month, left_index=True, right_index=True, how='outer')\
    .reset_index()\
    .merge(users, how='left', left_on='user_id', right_on='user_id')\

user_behavior.head()
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
      <th>user_id</th>
      <th>month</th>
      <th>calls</th>
      <th>messages</th>
      <th>mb_used</th>
      <th>minutes</th>
      <th>age</th>
      <th>churn_date</th>
      <th>city</th>
      <th>first_name</th>
      <th>last_name</th>
      <th>reg_date</th>
      <th>tariff</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>1000</td>
      <td>5</td>
      <td>22.0</td>
      <td>22.0</td>
      <td>2253.49</td>
      <td>159.0</td>
      <td>52</td>
      <td>NaT</td>
      <td>Краснодар</td>
      <td>Рафаил</td>
      <td>Верещагин</td>
      <td>2018-05-25</td>
      <td>ultra</td>
    </tr>
    <tr>
      <th>1</th>
      <td>1000</td>
      <td>6</td>
      <td>43.0</td>
      <td>60.0</td>
      <td>23233.77</td>
      <td>172.0</td>
      <td>52</td>
      <td>NaT</td>
      <td>Краснодар</td>
      <td>Рафаил</td>
      <td>Верещагин</td>
      <td>2018-05-25</td>
      <td>ultra</td>
    </tr>
    <tr>
      <th>2</th>
      <td>1000</td>
      <td>7</td>
      <td>47.0</td>
      <td>75.0</td>
      <td>14003.64</td>
      <td>340.0</td>
      <td>52</td>
      <td>NaT</td>
      <td>Краснодар</td>
      <td>Рафаил</td>
      <td>Верещагин</td>
      <td>2018-05-25</td>
      <td>ultra</td>
    </tr>
    <tr>
      <th>3</th>
      <td>1000</td>
      <td>8</td>
      <td>52.0</td>
      <td>81.0</td>
      <td>14055.93</td>
      <td>408.0</td>
      <td>52</td>
      <td>NaT</td>
      <td>Краснодар</td>
      <td>Рафаил</td>
      <td>Верещагин</td>
      <td>2018-05-25</td>
      <td>ultra</td>
    </tr>
    <tr>
      <th>4</th>
      <td>1000</td>
      <td>9</td>
      <td>58.0</td>
      <td>57.0</td>
      <td>14568.91</td>
      <td>466.0</td>
      <td>52</td>
      <td>NaT</td>
      <td>Краснодар</td>
      <td>Рафаил</td>
      <td>Верещагин</td>
      <td>2018-05-25</td>
      <td>ultra</td>
    </tr>
  </tbody>
</table>
</div>



Проверим пропуски в таблице `user_behavior` после объединения:


```python
user_behavior.isna().sum()
```




    user_id          0
    month            0
    calls           40
    messages       497
    mb_used         11
    minutes         40
    age              0
    churn_date    3027
    city             0
    first_name       0
    last_name        0
    reg_date         0
    tariff           0
    dtype: int64



Заполним образовавшиеся пропуски в данных:


```python
user_behavior['calls'] = user_behavior['calls'].fillna(0)
user_behavior['minutes'] = user_behavior['minutes'].fillna(0)
user_behavior['messages'] = user_behavior['messages'].fillna(0)
user_behavior['mb_used'] = user_behavior['mb_used'].fillna(0)
```

Присоединяем информацию о тарифах


```python
# переименование столбца tariff_name на более простое tariff

tariffs = tariffs.rename(
    columns={
        'tariff_name': 'tariff'
    }
)
```


```python
user_behavior = user_behavior.merge(tariffs, on='tariff')
```

Считаем количество минут разговора, сообщений и мегабайт, превышающих включенные в тариф



```python
user_behavior['paid_minutes'] = user_behavior['minutes'] - user_behavior['minutes_included']
user_behavior['paid_messages'] = user_behavior['messages'] - user_behavior['messages_included']
user_behavior['paid_mb'] = user_behavior['mb_used'] - user_behavior['mb_per_month_included']

for col in ['paid_messages', 'paid_minutes', 'paid_mb']:
    user_behavior.loc[user_behavior[col] < 0, col] = 0
```

Переводим превышающие тариф мегабайты в гигабайты и сохраняем в столбец `paid_gb`


```python
user_behavior['paid_gb'] = np.ceil(user_behavior['paid_mb'] / 1024).astype(int)
```

Считаем выручку за минуты разговора, сообщения и интернет


```python
user_behavior['cost_minutes'] = user_behavior['paid_minutes'] * user_behavior['rub_per_minute']
user_behavior['cost_messages'] = user_behavior['paid_messages'] * user_behavior['rub_per_message']
user_behavior['cost_gb'] = user_behavior['paid_gb'] * user_behavior['rub_per_gb']
```

Считаем помесячную выручку с каждого пользователя, она будет храниться в столбец `total_cost`


```python
user_behavior['total_cost'] = \
      user_behavior['rub_monthly_fee']\
    + user_behavior['cost_minutes']\
    + user_behavior['cost_messages']\
    + user_behavior['cost_gb']
```

Датафрейм `stats_df` для каждой пары "месяц-тариф" будет хранить основные характеристики


```python
# сохранение статистических метрик для каждой пары месяц-тариф
# в одной таблице stats_df (среднее значение, стандартное отклонение, медиана)

stats_df = user_behavior.pivot_table(
            index=['month', 'tariff'],\
            values=['calls', 'minutes', 'messages', 'mb_used'],\
            aggfunc=['mean', 'std', 'median']\
).round(2).reset_index()

stats_df.columns=['month', 'tariff', 'calls_mean', 'sessions_mean', 'messages_mean', 'minutes_mean',
                                     'calls_std',  'sessions_std', 'messages_std', 'minutes_std', 
                                     'calls_median', 'sessions_median', 'messages_median',  'minutes_median']

stats_df.head(10)
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
      <th>month</th>
      <th>tariff</th>
      <th>calls_mean</th>
      <th>sessions_mean</th>
      <th>messages_mean</th>
      <th>minutes_mean</th>
      <th>calls_std</th>
      <th>sessions_std</th>
      <th>messages_std</th>
      <th>minutes_std</th>
      <th>calls_median</th>
      <th>sessions_median</th>
      <th>messages_median</th>
      <th>minutes_median</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>1</td>
      <td>smart</td>
      <td>27.68</td>
      <td>8513.72</td>
      <td>18.24</td>
      <td>203.85</td>
      <td>20.81</td>
      <td>6444.68</td>
      <td>16.20</td>
      <td>154.23</td>
      <td>20.5</td>
      <td>7096.18</td>
      <td>15.0</td>
      <td>162.5</td>
    </tr>
    <tr>
      <th>1</th>
      <td>1</td>
      <td>ultra</td>
      <td>59.44</td>
      <td>13140.68</td>
      <td>33.78</td>
      <td>428.11</td>
      <td>41.64</td>
      <td>6865.35</td>
      <td>30.67</td>
      <td>269.76</td>
      <td>51.0</td>
      <td>14791.37</td>
      <td>32.0</td>
      <td>382.0</td>
    </tr>
    <tr>
      <th>2</th>
      <td>2</td>
      <td>smart</td>
      <td>40.19</td>
      <td>11597.05</td>
      <td>24.09</td>
      <td>298.69</td>
      <td>25.39</td>
      <td>6247.35</td>
      <td>21.75</td>
      <td>190.82</td>
      <td>38.5</td>
      <td>12553.71</td>
      <td>20.0</td>
      <td>258.0</td>
    </tr>
    <tr>
      <th>3</th>
      <td>2</td>
      <td>ultra</td>
      <td>41.54</td>
      <td>11775.94</td>
      <td>21.96</td>
      <td>297.12</td>
      <td>40.97</td>
      <td>10644.64</td>
      <td>26.77</td>
      <td>296.51</td>
      <td>25.0</td>
      <td>7327.12</td>
      <td>5.5</td>
      <td>168.0</td>
    </tr>
    <tr>
      <th>4</th>
      <td>3</td>
      <td>smart</td>
      <td>54.32</td>
      <td>15104.16</td>
      <td>31.86</td>
      <td>390.05</td>
      <td>25.54</td>
      <td>5828.24</td>
      <td>26.80</td>
      <td>191.89</td>
      <td>59.0</td>
      <td>15670.25</td>
      <td>23.0</td>
      <td>409.0</td>
    </tr>
    <tr>
      <th>5</th>
      <td>3</td>
      <td>ultra</td>
      <td>67.68</td>
      <td>17535.55</td>
      <td>32.30</td>
      <td>489.65</td>
      <td>44.84</td>
      <td>10951.79</td>
      <td>41.62</td>
      <td>333.74</td>
      <td>57.0</td>
      <td>17495.18</td>
      <td>20.0</td>
      <td>403.0</td>
    </tr>
    <tr>
      <th>6</th>
      <td>4</td>
      <td>smart</td>
      <td>51.31</td>
      <td>13462.18</td>
      <td>30.74</td>
      <td>367.13</td>
      <td>25.70</td>
      <td>5698.25</td>
      <td>24.54</td>
      <td>186.49</td>
      <td>52.0</td>
      <td>14087.65</td>
      <td>28.0</td>
      <td>368.5</td>
    </tr>
    <tr>
      <th>7</th>
      <td>4</td>
      <td>ultra</td>
      <td>64.09</td>
      <td>16828.13</td>
      <td>31.56</td>
      <td>458.02</td>
      <td>36.27</td>
      <td>9718.65</td>
      <td>37.51</td>
      <td>267.68</td>
      <td>61.0</td>
      <td>16645.78</td>
      <td>17.0</td>
      <td>453.0</td>
    </tr>
    <tr>
      <th>8</th>
      <td>5</td>
      <td>smart</td>
      <td>55.24</td>
      <td>15805.18</td>
      <td>33.77</td>
      <td>387.36</td>
      <td>25.38</td>
      <td>5978.23</td>
      <td>27.04</td>
      <td>186.60</td>
      <td>59.0</td>
      <td>16323.94</td>
      <td>30.0</td>
      <td>433.0</td>
    </tr>
    <tr>
      <th>9</th>
      <td>5</td>
      <td>ultra</td>
      <td>72.51</td>
      <td>19363.15</td>
      <td>37.85</td>
      <td>510.33</td>
      <td>41.08</td>
      <td>10046.11</td>
      <td>40.31</td>
      <td>289.60</td>
      <td>75.0</td>
      <td>18696.43</td>
      <td>25.0</td>
      <td>519.0</td>
    </tr>
  </tbody>
</table>
</div>



Распределение среднего количества звонков по видам тарифов и месяцам


```python
import seaborn as sns

ax = sns.barplot(x='month',
            y='calls_mean',
            hue="tariff",
            data=stats_df,
            palette=['lightblue', 'blue'])

ax.set_title('Распределение количества звонков по видам тарифов и месяцам')
ax.set(xlabel='Номер месяца', ylabel='Среднее количество звонков');
```


    
![png](output_85_0.png)
    



```python
import matplotlib.pyplot as plt

user_behavior.groupby('tariff')['calls'].plot(kind='hist', bins=35, alpha=0.5)
plt.legend(['Smart', 'Ultra'])
plt.xlabel('Количество звонков')
plt.ylabel('Количество клиентов')
plt.show()
```


    
![png](output_86_0.png)
    


Распределение средней продолжительности звонков по видам тарифов и месяцам


```python
ax = sns.barplot(x='month',
            y='minutes_mean',
            hue="tariff",
            data=stats_df,
            palette=['lightblue', 'blue'])

ax.set_title('Распределение продолжительности звонков по видам тарифов и месяцам')
ax.set(xlabel='Номер месяца', ylabel='Средняя продолжительность звонков');
```


    
![png](output_88_0.png)
    



```python
user_behavior[user_behavior['tariff'] =='smart']['minutes'].hist(bins=35, alpha=0.5, color='green')
user_behavior[user_behavior['tariff'] =='ultra']['minutes'].hist(bins=35, alpha=0.5, color='blue');
```


    
![png](output_89_0.png)
    


Средняя длительность разговоров у абонентов тарифа Ultra больше, чем у абонентов тарифа Smart. В течение года пользователи обоих тарифов увеличивают среднюю продолжительность своих разговоров. Рост средней длительности разговоров у абонентов тарифа Smart равномерный в течение года. Пользователи тарифа Ultra не проявляют подобной линейной стабильности. Стоит отметить, что феврале у абонентов обоих тарифных планов наблюдались самые низкие показатели.

Распределение среднего количества сообщений по видам тарифов и месяцам


```python
ax = sns.barplot(x='month',
            y='messages_mean',
            hue="tariff",
            data=stats_df,
            palette=['lightblue', 'blue']
)

ax.set_title('Распределение количества сообщений по видам тарифов и месяцам')
ax.set(xlabel='Номер месяца', ylabel='Среднее количество звонков');
```


    
![png](output_92_0.png)
    



```python
user_behavior[user_behavior['tariff'] =='smart']['messages'].hist(bins=35, alpha=0.5, color='green')
user_behavior[user_behavior['tariff'] =='ultra']['messages'].hist(bins=35, alpha=0.5, color='blue');
```


    
![png](output_93_0.png)
    


В среднем количество сообщений пользователи тарифа Ultra отправляют больше - почти на 20 сообщений больше, чем пользователи тарифа Smart. Количество сообщений в течение года на обоих тарифак растет. Динамика по отправке сообщений схожа с тенденциями по длительности разговоров: в феврале отмечено наименьшее количество сообщений за год и пользователи тарифа Ultra также проявляют нелинейную полодительную динамику.


```python
ax = sns.barplot(x='month',
            y='sessions_mean',
            hue="tariff",
            data=stats_df,
            palette=['lightblue', 'blue']
)

ax.set_title('Распределение количества потраченного трафика (Мб) по видам тарифов и месяцам')
ax.set(xlabel='Номер месяца', ylabel='Среднее количество мегабайт');
```


    
![png](output_95_0.png)
    


Сравнение потраченных мегабайт среди пользователей тарифов Smart и Ultra


```python
user_behavior[user_behavior['tariff'] =='smart']['mb_used'].hist(bins=35, alpha=0.5, color='green')
user_behavior[user_behavior['tariff'] =='ultra']['mb_used'].hist(bins=35, alpha=0.5, color='blue');
```


    
![png](output_97_0.png)
    


Меньше всего пользователи использовали интернет в январе, феврале и апреле. Чаще всего абоненты тарифа Smart тратят 15-17 Гб, а абоненты тарифного плана Ultra - 19-21 ГБ.

### Проверка гипотез

**Задание 28.** Проверка гипотезы: средняя выручка пользователей тарифов «Ультра» и «Смарт» различаются;

```
H_0: Выручка (total_cost) пользователей "Ультра" = выручка (total_cost) пользователей "Смарт"`
H_a: Выручка (total_cost) пользователей "Ультра" ≠ выручка (total_cost) пользователей "Смарт"`
alpha = 0.05
```


```python
from scipy import stats as st
```


```python
ultra = user_behavior.loc[user_behavior.tariff == 'ultra', 'total_cost']
smart = user_behavior.loc[user_behavior.tariff == 'smart', 'total_cost']
# results = вызов метода для проверки гипотезы
results = st.ttest_ind(ultra, smart, equal_var=False)
# alpha = задайте значение уровня значимости
alpha = .05
# вывод значения p-value на экран 
print(results.pvalue)
# условный оператор с выводом строки с ответом
if results.pvalue < alpha:
    print('Отвергаем нулевую гипотезу')
else:
    print('Не получилось отвергнуть нулевую гипотезу')
```

    4.2606313931076085e-250
    Отвергаем нулевую гипотезу


**Задание 29.** Проверка гипотезы: пользователи из Москвы приносят больше выручки, чем пользователи из других городов;

```
H_0: Выручка (total_cost) пользователей из Москвы = выручка (total_cost) пользователей не из Москвы`
H_1: Выручка (total_cost) пользователей из Москвы ≠ выручка (total_cost) пользователей не из Москвы`
alpha = 0.05
```


```python
ultra = user_behavior.loc[user_behavior.city == 'Москва', 'total_cost']
smart = user_behavior.loc[user_behavior.city != 'Москва', 'total_cost']
# results = вызов метода для проверки гипотезы
results = st.ttest_ind(ultra, smart, equal_var=False)
# alpha = задайте значение уровня значимости
alpha = .05
# вывод значения p-value на экран 
print(results.pvalue)
# условный оператор с выводом строки с ответом
if results.pvalue < alpha:
    print('Отвергаем нулевую гипотезу')
else:
    print('Не получилось отвергнуть нулевую гипотезу')
```

    0.5257376663729298
    Не получилось отвергнуть нулевую гипотезу

