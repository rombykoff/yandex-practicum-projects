# Анализ бизнес-показателей приложения Procrastinate Pro+

Несмотря на огромные вложения в рекламу, последние несколько месяцев компания терпит убытки. Наша задача — разобраться в причинах и помочь компании выйти в плюс.

**Необходимо рассмотреть следующие показатели:**
- откуда приходят пользователи и какими устройствами они пользуются,
- сколько стоит привлечение пользователей из различных рекламных каналов;
- сколько денег приносит каждый клиент,
- когда расходы на привлечение клиента окупаются,
- какие факторы мешают привлечению клиентов.

**В нашем распоряжении три датасета:**

`visits_info_short.csv` - хранит лог сервера с информацией о посещениях сайта  
- User Id — уникальный идентификатор пользователя,
- Region — страна пользователя,
- Device — тип устройства пользователя,
- Channel — идентификатор источника перехода,
- Session Start — дата и время начала сессии,
- Session End — дата и время окончания сессии.

`orders_info_short.csv` - информацию о покупках
- User Id — уникальный идентификатор пользователя,
- Event Dt — дата и время покупки,
- Revenue — сумма заказа.

`costs_info_short.csv` - информацию о расходах на рекламу
- Channel — идентификатор рекламного источника,
- Dt — дата проведения рекламной кампании,
- Costs — расходы на эту кампанию.

<a id='intro'></a>

<h1>Содержание<span class="tocSkip"></span></h1>
<div class="toc"><ul class="toc-item"><li><span><a href="#Анализ-бизнес-показателей-приложения-Procrastinate-Pro+" data-toc-modified-id="Анализ-бизнес-показателей-приложения-Procrastinate-Pro+-1">Анализ бизнес-показателей приложения Procrastinate Pro+</a></span><ul class="toc-item"><li><span><a href="#Шаг-1.-Загрузка-данных-и-подготовка-к-анализу" data-toc-modified-id="Шаг-1.-Загрузка-данных-и-подготовка-к-анализу-1.1">Шаг 1. Загрузка данных и подготовка к анализу</a></span></li><li><span><a href="#Шаг-2.-Функции-для-расчета-и-анализа-LTV,-ROI,-удержания-и-конверсии" data-toc-modified-id="Шаг-2.-Функции-для-расчета-и-анализа-LTV,-ROI,-удержания-и-конверсии-1.2">Шаг 2. Функции для расчета и анализа LTV, ROI, удержания и конверсии</a></span></li><li><span><a href="#Шаг-3.-Исследовательский-анализ-данных" data-toc-modified-id="Шаг-3.-Исследовательский-анализ-данных-1.3">Шаг 3. Исследовательский анализ данных</a></span></li><li><span><a href="#Шаг-4.-Маркетинг" data-toc-modified-id="Шаг-4.-Маркетинг-1.4">Шаг 4. Маркетинг</a></span><ul class="toc-item"><li><span><a href="#Затраты-на-рекламу" data-toc-modified-id="Затраты-на-рекламу-1.4.1">Затраты на рекламу</a></span></li><li><span><a href="#Стоимость-привлечения-клиента" data-toc-modified-id="Стоимость-привлечения-клиента-1.4.2">Стоимость привлечения клиента</a></span></li></ul></li><li><span><a href="#Шаг-5.-Оценка-окупаемости-рекламы-для-привлечения-пользователей" data-toc-modified-id="Шаг-5.-Оценка-окупаемости-рекламы-для-привлечения-пользователей-1.5">Шаг 5. Оценка окупаемости рекламы для привлечения пользователей</a></span><ul class="toc-item"><li><span><a href="#Анализ-общей-окупаемости-рекламы" data-toc-modified-id="Анализ-общей-окупаемости-рекламы-1.5.1">Анализ общей окупаемости рекламы</a></span></li><li><span><a href="#Анализ-окупаемости-рекламы-с-разбивкой-по-устройствам" data-toc-modified-id="Анализ-окупаемости-рекламы-с-разбивкой-по-устройствам-1.5.2">Анализ окупаемости рекламы с разбивкой по устройствам</a></span></li><li><span><a href="#Анализ-окупаемости-рекламы-с-разбивкой-по-странам" data-toc-modified-id="Анализ-окупаемости-рекламы-с-разбивкой-по-странам-1.5.3">Анализ окупаемости рекламы с разбивкой по странам</a></span></li><li><span><a href="#Анализ-окупаемости-рекламы-с-разбивкой-по-рекламным-каналам" data-toc-modified-id="Анализ-окупаемости-рекламы-с-разбивкой-по-рекламным-каналам-1.5.4">Анализ окупаемости рекламы с разбивкой по рекламным каналам</a></span></li></ul></li></ul></li><li><span><a href="#Шаг-6.-Общий-вывод" data-toc-modified-id="Шаг-6.-Общий-вывод-2">Шаг 6. Общий вывод</a></span></li></ul></div>

## Шаг 1. Загрузка данных и подготовка к анализу
Загрузим данные о визитах, заказах и расходах в переменные. Оптимизируем данные для анализа. Убедимся, что тип данных в каждой колонке — правильный.

[Содержание](#intro)


```python
import pandas as pd
import numpy as np
from datetime import datetime, timedelta
from matplotlib import pyplot as plt
import seaborn as sns
```


```python
try:
    visits, orders, costs = (
        pd.read_csv('/datasets/visits_info_short.csv'),
        pd.read_csv('/datasets/orders_info_short.csv'),
        pd.read_csv('/datasets/costs_info_short.csv')
        )
except FileNotFoundError:
    print('Некорректно задан путь к файлу')
```

**Данные загружены. Посмотрим таблицы, общую информацию и проверим дубликаты:**

**`visits`**


```python
visits.info()
print()
display(visits.head(3))
print()
print('Количество явных строк-дубликатов:', visits.duplicated().sum())
```

    <class 'pandas.core.frame.DataFrame'>
    RangeIndex: 309901 entries, 0 to 309900
    Data columns (total 6 columns):
     #   Column         Non-Null Count   Dtype 
    ---  ------         --------------   ----- 
     0   User Id        309901 non-null  int64 
     1   Region         309901 non-null  object
     2   Device         309901 non-null  object
     3   Channel        309901 non-null  object
     4   Session Start  309901 non-null  object
     5   Session End    309901 non-null  object
    dtypes: int64(1), object(5)
    memory usage: 14.2+ MB
    



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
      <th>User Id</th>
      <th>Region</th>
      <th>Device</th>
      <th>Channel</th>
      <th>Session Start</th>
      <th>Session End</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>981449118918</td>
      <td>United States</td>
      <td>iPhone</td>
      <td>organic</td>
      <td>2019-05-01 02:36:01</td>
      <td>2019-05-01 02:45:01</td>
    </tr>
    <tr>
      <th>1</th>
      <td>278965908054</td>
      <td>United States</td>
      <td>iPhone</td>
      <td>organic</td>
      <td>2019-05-01 04:46:31</td>
      <td>2019-05-01 04:47:35</td>
    </tr>
    <tr>
      <th>2</th>
      <td>590706206550</td>
      <td>United States</td>
      <td>Mac</td>
      <td>organic</td>
      <td>2019-05-01 14:09:25</td>
      <td>2019-05-01 15:32:08</td>
    </tr>
  </tbody>
</table>
</div>


    
    Количество явных строк-дубликатов: 0


**`orders`**


```python
orders.info()
print()
display(orders.head(3))
print()
print('Количество явных строк-дубликатов:', orders.duplicated().sum())
```

    <class 'pandas.core.frame.DataFrame'>
    RangeIndex: 40212 entries, 0 to 40211
    Data columns (total 3 columns):
     #   Column    Non-Null Count  Dtype  
    ---  ------    --------------  -----  
     0   User Id   40212 non-null  int64  
     1   Event Dt  40212 non-null  object 
     2   Revenue   40212 non-null  float64
    dtypes: float64(1), int64(1), object(1)
    memory usage: 942.6+ KB
    



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
      <th>User Id</th>
      <th>Event Dt</th>
      <th>Revenue</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>188246423999</td>
      <td>2019-05-01 23:09:52</td>
      <td>4.99</td>
    </tr>
    <tr>
      <th>1</th>
      <td>174361394180</td>
      <td>2019-05-01 12:24:04</td>
      <td>4.99</td>
    </tr>
    <tr>
      <th>2</th>
      <td>529610067795</td>
      <td>2019-05-01 11:34:04</td>
      <td>4.99</td>
    </tr>
  </tbody>
</table>
</div>


    
    Количество явных строк-дубликатов: 0


**`costs`**


```python
costs.info()
print()
display(costs.head(3))
print()
print('Количество явных строк-дубликатов:', costs.duplicated().sum())
```

    <class 'pandas.core.frame.DataFrame'>
    RangeIndex: 1800 entries, 0 to 1799
    Data columns (total 3 columns):
     #   Column   Non-Null Count  Dtype  
    ---  ------   --------------  -----  
     0   dt       1800 non-null   object 
     1   Channel  1800 non-null   object 
     2   costs    1800 non-null   float64
    dtypes: float64(1), object(2)
    memory usage: 42.3+ KB
    



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
      <th>dt</th>
      <th>Channel</th>
      <th>costs</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>2019-05-01</td>
      <td>FaceBoom</td>
      <td>113.3</td>
    </tr>
    <tr>
      <th>1</th>
      <td>2019-05-02</td>
      <td>FaceBoom</td>
      <td>78.1</td>
    </tr>
    <tr>
      <th>2</th>
      <td>2019-05-03</td>
      <td>FaceBoom</td>
      <td>85.8</td>
    </tr>
  </tbody>
</table>
</div>


    
    Количество явных строк-дубликатов: 0


**По итогу:**
- Пропусков и дубликатов нет
- Названия столбцов нужно привести к общему стилю
- Изменить тип данных в столбцах с датами


```python
#приведем столбцы к змеиному регистру
visits.columns = [x.lower().replace(' ', '_') for x in visits.columns.values]
orders.columns = [x.lower().replace(' ', '_') for x in orders.columns.values]
costs.columns = costs.columns.str.lower()

#изменим тип данных
visits['session_start'] = pd.to_datetime(visits['session_start'])
visits['session_end'] = pd.to_datetime(visits['session_end'])
orders['event_dt'] = pd.to_datetime(orders['event_dt'])
costs['dt'] = pd.to_datetime(costs['dt']).dt.date

visits.info()
display(visits.head(1))
print()
orders.info()
display(orders.head(1))
print()
costs.info()
display(costs.head(1))
print()
```

    <class 'pandas.core.frame.DataFrame'>
    RangeIndex: 309901 entries, 0 to 309900
    Data columns (total 6 columns):
     #   Column         Non-Null Count   Dtype         
    ---  ------         --------------   -----         
     0   user_id        309901 non-null  int64         
     1   region         309901 non-null  object        
     2   device         309901 non-null  object        
     3   channel        309901 non-null  object        
     4   session_start  309901 non-null  datetime64[ns]
     5   session_end    309901 non-null  datetime64[ns]
    dtypes: datetime64[ns](2), int64(1), object(3)
    memory usage: 14.2+ MB



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
      <th>region</th>
      <th>device</th>
      <th>channel</th>
      <th>session_start</th>
      <th>session_end</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>981449118918</td>
      <td>United States</td>
      <td>iPhone</td>
      <td>organic</td>
      <td>2019-05-01 02:36:01</td>
      <td>2019-05-01 02:45:01</td>
    </tr>
  </tbody>
</table>
</div>


    
    <class 'pandas.core.frame.DataFrame'>
    RangeIndex: 40212 entries, 0 to 40211
    Data columns (total 3 columns):
     #   Column    Non-Null Count  Dtype         
    ---  ------    --------------  -----         
     0   user_id   40212 non-null  int64         
     1   event_dt  40212 non-null  datetime64[ns]
     2   revenue   40212 non-null  float64       
    dtypes: datetime64[ns](1), float64(1), int64(1)
    memory usage: 942.6 KB



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
      <th>event_dt</th>
      <th>revenue</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>188246423999</td>
      <td>2019-05-01 23:09:52</td>
      <td>4.99</td>
    </tr>
  </tbody>
</table>
</div>


    
    <class 'pandas.core.frame.DataFrame'>
    RangeIndex: 1800 entries, 0 to 1799
    Data columns (total 3 columns):
     #   Column   Non-Null Count  Dtype  
    ---  ------   --------------  -----  
     0   dt       1800 non-null   object 
     1   channel  1800 non-null   object 
     2   costs    1800 non-null   float64
    dtypes: float64(1), object(2)
    memory usage: 42.3+ KB



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
      <th>dt</th>
      <th>channel</th>
      <th>costs</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>2019-05-01</td>
      <td>FaceBoom</td>
      <td>113.3</td>
    </tr>
  </tbody>
</table>
</div>


    


**Вывод:** Пропусков и явных строк-дубликатов нет. Данные преобразованы для дальнейшего анализа.

## Шаг 2. Функции для расчета и анализа LTV, ROI, удержания и конверсии

[Содержание](#intro)


```python
# функция для создания пользовательских профилей

def get_profiles(visits, orders, ad_costs, event_names=[]):

    # находим параметры первых посещений
    profiles = (
        visits.sort_values(by=['user_id', 'session_start'])
        .groupby('user_id')
        .agg(
            {
                'session_start': 'first',
                'channel': 'first',
                'device': 'first',
                'region': 'first',
            }
        )
        .rename(columns={'session_start': 'first_ts'})
        .reset_index()
    )

    # для когортного анализа определяем дату первого посещения
    # и первый день месяца, в который это посещение произошло
    profiles['dt'] = profiles['first_ts'].dt.date
    profiles['month'] = profiles['first_ts'].astype('datetime64[M]')

    # добавляем признак платящих пользователей
    profiles['payer'] = profiles['user_id'].isin(orders['user_id'].unique())


    # считаем количество уникальных пользователей
    # с одинаковыми источником и датой привлечения
    new_users = (
        profiles.groupby(['dt', 'channel'])
        .agg({'user_id': 'nunique'})
        .rename(columns={'user_id': 'unique_users'})
        .reset_index()
    )

    # объединяем траты на рекламу и число привлечённых пользователей
    ad_costs = ad_costs.merge(new_users, on=['dt', 'channel'], how='left')

    # делим рекламные расходы на число привлечённых пользователей
    ad_costs['acquisition_cost'] = ad_costs['costs'] / ad_costs['unique_users']

    # добавляем стоимость привлечения в профили
    profiles = profiles.merge(
        ad_costs[['dt', 'channel', 'acquisition_cost']],
        on=['dt', 'channel'],
        how='left',
    )

    # стоимость привлечения органических пользователей равна нулю
    profiles['acquisition_cost'] = profiles['acquisition_cost'].fillna(0)

    return profiles
```


```python
# функция для расчёта удержания

def get_retention(
    profiles,
    visits,
    observation_date,
    horizon_days,
    dimensions=[],
    ignore_horizon=False,
):

    # добавляем столбец payer в передаваемый dimensions список
    dimensions = ['payer'] + dimensions

    # исключаем пользователей, не «доживших» до горизонта анализа
    last_suitable_acquisition_date = observation_date
    if not ignore_horizon:
        last_suitable_acquisition_date = observation_date - timedelta(
            days=horizon_days - 1
        )
    result_raw = profiles.query('dt <= @last_suitable_acquisition_date')

    # собираем «сырые» данные для расчёта удержания
    result_raw = result_raw.merge(
        visits[['user_id', 'session_start']], on='user_id', how='left'
    )
    result_raw['lifetime'] = (
        result_raw['session_start'] - result_raw['first_ts']
    ).dt.days

    # функция для группировки таблицы по желаемым признакам
    def group_by_dimensions(df, dims, horizon_days):
        result = df.pivot_table(
            index=dims, columns='lifetime', values='user_id', aggfunc='nunique'
        )
        cohort_sizes = (
            df.groupby(dims)
            .agg({'user_id': 'nunique'})
            .rename(columns={'user_id': 'cohort_size'})
        )
        result = cohort_sizes.merge(result, on=dims, how='left').fillna(0)
        result = result.div(result['cohort_size'], axis=0)
        result = result[['cohort_size'] + list(range(horizon_days))]
        result['cohort_size'] = cohort_sizes
        return result

    # получаем таблицу удержания
    result_grouped = group_by_dimensions(result_raw, dimensions, horizon_days)

    # получаем таблицу динамики удержания
    result_in_time = group_by_dimensions(
        result_raw, dimensions + ['dt'], horizon_days
    )

    # возвращаем обе таблицы и сырые данные
    return result_raw, result_grouped, result_in_time
```


```python
# функция для расчёта конверсии

def get_conversion(
    profiles,
    orders,
    observation_date,
    horizon_days,
    dimensions=[],
    ignore_horizon=False,
):

    # исключаем пользователей, не «доживших» до горизонта анализа
    last_suitable_acquisition_date = observation_date
    if not ignore_horizon:
        last_suitable_acquisition_date = observation_date - timedelta(
            days=horizon_days - 1
        )
    result_raw = profiles.query('dt <= @last_suitable_acquisition_date')

    # определяем дату и время первой покупки для каждого пользователя
    first_purchases = (
        orders.sort_values(by=['user_id', 'event_dt'])
        .groupby('user_id')
        .agg({'event_dt': 'first'})
        .reset_index()
    )

    # добавляем данные о покупках в профили
    result_raw = result_raw.merge(
        first_purchases[['user_id', 'event_dt']], on='user_id', how='left'
    )

    # рассчитываем лайфтайм для каждой покупки
    result_raw['lifetime'] = (
        result_raw['event_dt'] - result_raw['first_ts']
    ).dt.days

    # группируем по cohort, если в dimensions ничего нет
    if len(dimensions) == 0:
        result_raw['cohort'] = 'All users' 
        dimensions = dimensions + ['cohort']

    # функция для группировки таблицы по желаемым признакам
    def group_by_dimensions(df, dims, horizon_days):
        result = df.pivot_table(
            index=dims, columns='lifetime', values='user_id', aggfunc='nunique'
        )
        result = result.fillna(0).cumsum(axis = 1)
        cohort_sizes = (
            df.groupby(dims)
            .agg({'user_id': 'nunique'})
            .rename(columns={'user_id': 'cohort_size'})
        )
        result = cohort_sizes.merge(result, on=dims, how='left').fillna(0)
        # делим каждую «ячейку» в строке на размер когорты
        # и получаем conversion rate
        result = result.div(result['cohort_size'], axis=0)
        result = result[['cohort_size'] + list(range(horizon_days))]
        result['cohort_size'] = cohort_sizes
        return result

    # получаем таблицу конверсии
    result_grouped = group_by_dimensions(result_raw, dimensions, horizon_days)

    # для таблицы динамики конверсии убираем 'cohort' из dimensions
    if 'cohort' in dimensions: 
        dimensions = []

    # получаем таблицу динамики конверсии
    result_in_time = group_by_dimensions(
        result_raw, dimensions + ['dt'], horizon_days
    )

    # возвращаем обе таблицы и сырые данные
    return result_raw, result_grouped, result_in_time
```


```python
# функция для расчёта LTV и ROI

def get_ltv(
    profiles,
    orders,
    observation_date,
    horizon_days,
    dimensions=[],
    ignore_horizon=False,
):

    # исключаем пользователей, не «доживших» до горизонта анализа
    last_suitable_acquisition_date = observation_date
    if not ignore_horizon:
        last_suitable_acquisition_date = observation_date - timedelta(
            days=horizon_days - 1
        )
    result_raw = profiles.query('dt <= @last_suitable_acquisition_date')
    # добавляем данные о покупках в профили
    result_raw = result_raw.merge(
        orders[['user_id', 'event_dt', 'revenue']], on='user_id', how='left'
    )
    # рассчитываем лайфтайм пользователя для каждой покупки
    result_raw['lifetime'] = (
        result_raw['event_dt'] - result_raw['first_ts']
    ).dt.days
    # группируем по cohort, если в dimensions ничего нет
    if len(dimensions) == 0:
        result_raw['cohort'] = 'All users'
        dimensions = dimensions + ['cohort']

    # функция группировки по желаемым признакам
    def group_by_dimensions(df, dims, horizon_days):
        # строим «треугольную» таблицу выручки
        result = df.pivot_table(
            index=dims, columns='lifetime', values='revenue', aggfunc='sum'
        )
        # находим сумму выручки с накоплением
        result = result.fillna(0).cumsum(axis=1)
        # вычисляем размеры когорт
        cohort_sizes = (
            df.groupby(dims)
            .agg({'user_id': 'nunique'})
            .rename(columns={'user_id': 'cohort_size'})
        )
        # объединяем размеры когорт и таблицу выручки
        result = cohort_sizes.merge(result, on=dims, how='left').fillna(0)
        # считаем LTV: делим каждую «ячейку» в строке на размер когорты
        result = result.div(result['cohort_size'], axis=0)
        # исключаем все лайфтаймы, превышающие горизонт анализа
        result = result[['cohort_size'] + list(range(horizon_days))]
        # восстанавливаем размеры когорт
        result['cohort_size'] = cohort_sizes

        # собираем датафрейм с данными пользователей и значениями CAC, 
        # добавляя параметры из dimensions
        cac = df[['user_id', 'acquisition_cost'] + dims].drop_duplicates()

        # считаем средний CAC по параметрам из dimensions
        cac = (
            cac.groupby(dims)
            .agg({'acquisition_cost': 'mean'})
            .rename(columns={'acquisition_cost': 'cac'})
        )

        # считаем ROI: делим LTV на CAC
        roi = result.div(cac['cac'], axis=0)

        # удаляем строки с бесконечным ROI
        roi = roi[~roi['cohort_size'].isin([np.inf])]

        # восстанавливаем размеры когорт в таблице ROI
        roi['cohort_size'] = cohort_sizes

        # добавляем CAC в таблицу ROI
        roi['cac'] = cac['cac']

        # в финальной таблице оставляем размеры когорт, CAC
        # и ROI в лайфтаймы, не превышающие горизонт анализа
        roi = roi[['cohort_size', 'cac'] + list(range(horizon_days))]

        # возвращаем таблицы LTV и ROI
        return result, roi

    # получаем таблицы LTV и ROI
    result_grouped, roi_grouped = group_by_dimensions(
        result_raw, dimensions, horizon_days
    )

    # для таблиц динамики убираем 'cohort' из dimensions
    if 'cohort' in dimensions:
        dimensions = []

    # получаем таблицы динамики LTV и ROI
    result_in_time, roi_in_time = group_by_dimensions(
        result_raw, dimensions + ['dt'], horizon_days
    )

    return (
        result_raw,  # сырые данные
        result_grouped,  # таблица LTV
        result_in_time,  # таблица динамики LTV
        roi_grouped,  # таблица ROI
        roi_in_time,  # таблица динамики ROI
    )
```


```python
# функция для сглаживания фрейма

def filter_data(df, window):
    # для каждого столбца применяем скользящее среднее
    for column in df.columns.values:
        df[column] = df[column].rolling(window).mean() 
    return df
```


```python
# функция для визуализации удержания

def plot_retention(retention, retention_history, horizon, window=7):

    # задаём размер сетки для графиков
    plt.figure(figsize=(15, 10))

    # исключаем размеры когорт и удержание первого дня
    retention = retention.drop(columns=['cohort_size', 0])
    # в таблице динамики оставляем только нужный лайфтайм
    retention_history = retention_history.drop(columns=['cohort_size'])[
        [horizon - 1]
    ]

    # если в индексах таблицы удержания только payer,
    # добавляем второй признак — cohort
    if retention.index.nlevels == 1:
        retention['cohort'] = 'All users'
        retention = retention.reset_index().set_index(['cohort', 'payer'])

    # в таблице графиков — два столбца и две строки, четыре ячейки
    # в первой строим кривые удержания платящих пользователей
    ax1 = plt.subplot(2, 2, 1)
    retention.query('payer == True').droplevel('payer').T.plot(
        grid=True, ax=ax1
    )
    plt.legend()
    plt.xlabel('Лайфтайм')
    plt.title('Удержание платящих пользователей')

    # во второй ячейке строим кривые удержания неплатящих
    # вертикальная ось — от графика из первой ячейки
    ax2 = plt.subplot(2, 2, 2, sharey=ax1)
    retention.query('payer == False').droplevel('payer').T.plot(
        grid=True, ax=ax2
    )
    plt.legend()
    plt.xlabel('Лайфтайм')
    plt.title('Удержание неплатящих пользователей')

    # в третьей ячейке — динамика удержания платящих
    ax3 = plt.subplot(2, 2, 3)
    # получаем названия столбцов для сводной таблицы
    columns = [
        name
        for name in retention_history.index.names
        if name not in ['dt', 'payer']
    ]
    # фильтруем данные и строим график
    filtered_data = retention_history.query('payer == True').pivot_table(
        index='dt', columns=columns, values=horizon - 1, aggfunc='mean'
    )
    filter_data(filtered_data, window).plot(grid=True, ax=ax3)
    plt.xlabel('Дата привлечения')
    plt.title(
        'Динамика удержания платящих пользователей на {}-й день'.format(
            horizon
        )
    )

    # в чётвертой ячейке — динамика удержания неплатящих
    ax4 = plt.subplot(2, 2, 4, sharey=ax3)
    # фильтруем данные и строим график
    filtered_data = retention_history.query('payer == False').pivot_table(
        index='dt', columns=columns, values=horizon - 1, aggfunc='mean'
    )
    filter_data(filtered_data, window).plot(grid=True, ax=ax4)
    plt.xlabel('Дата привлечения')
    plt.title(
        'Динамика удержания неплатящих пользователей на {}-й день'.format(
            horizon
        )
    )
    
    plt.tight_layout()
    plt.show()
```


```python
# функция для визуализации конверсии

def plot_conversion(conversion, conversion_history, horizon, window=7):

    # задаём размер сетки для графиков
    plt.figure(figsize=(15, 5))

    # исключаем размеры когорт
    conversion = conversion.drop(columns=['cohort_size'])
    # в таблице динамики оставляем только нужный лайфтайм
    conversion_history = conversion_history.drop(columns=['cohort_size'])[
        [horizon - 1]
    ]

    # первый график — кривые конверсии
    ax1 = plt.subplot(1, 2, 1)
    conversion.T.plot(grid=True, ax=ax1)
    plt.legend()
    plt.xlabel('Лайфтайм')
    plt.title('Конверсия пользователей')

    # второй график — динамика конверсии
    ax2 = plt.subplot(1, 2, 2, sharey=ax1)
    columns = [
        # столбцами сводной таблицы станут все столбцы индекса, кроме даты
        name for name in conversion_history.index.names if name not in ['dt']
    ]
    filtered_data = conversion_history.pivot_table(
        index='dt', columns=columns, values=horizon - 1, aggfunc='mean'
    )
    filter_data(filtered_data, window).plot(grid=True, ax=ax2)
    plt.xlabel('Дата привлечения')
    plt.title('Динамика конверсии пользователей на {}-й день'.format(horizon))

    plt.tight_layout()
    plt.show()
```


```python
# функция для визуализации LTV и ROI

def plot_ltv_roi(ltv, ltv_history, roi, roi_history, horizon, window=7):

    # задаём сетку отрисовки графиков
    plt.figure(figsize=(20, 10))

    # из таблицы ltv исключаем размеры когорт
    ltv = ltv.drop(columns=['cohort_size'])
    # в таблице динамики ltv оставляем только нужный лайфтайм
    ltv_history = ltv_history.drop(columns=['cohort_size'])[[horizon - 1]]

    # стоимость привлечения запишем в отдельный фрейм
    cac_history = roi_history[['cac']]

    # из таблицы roi исключаем размеры когорт и cac
    roi = roi.drop(columns=['cohort_size', 'cac'])
    # в таблице динамики roi оставляем только нужный лайфтайм
    roi_history = roi_history.drop(columns=['cohort_size', 'cac'])[
        [horizon - 1]
    ]

    # первый график — кривые ltv
    ax1 = plt.subplot(2, 3, 1)
    ltv.T.plot(grid=True, ax=ax1)
    plt.legend()
    plt.xlabel('Лайфтайм')
    plt.title('LTV')

    # второй график — динамика ltv
    ax2 = plt.subplot(2, 3, 2, sharey=ax1)
    # столбцами сводной таблицы станут все столбцы индекса, кроме даты
    columns = [name for name in ltv_history.index.names if name not in ['dt']]
    filtered_data = ltv_history.pivot_table(
        index='dt', columns=columns, values=horizon - 1, aggfunc='mean'
    )
    filter_data(filtered_data, window).plot(grid=True, ax=ax2)
    plt.xlabel('Дата привлечения')
    plt.title('Динамика LTV пользователей на {}-й день'.format(horizon))

    # третий график — динамика cac
    ax3 = plt.subplot(2, 3, 3, sharey=ax1)
    # столбцами сводной таблицы станут все столбцы индекса, кроме даты
    columns = [name for name in cac_history.index.names if name not in ['dt']]
    filtered_data = cac_history.pivot_table(
        index='dt', columns=columns, values='cac', aggfunc='mean'
    )
    filter_data(filtered_data, window).plot(grid=True, ax=ax3)
    plt.xlabel('Дата привлечения')
    plt.title('Динамика стоимости привлечения пользователей')

    # четвёртый график — кривые roi
    ax4 = plt.subplot(2, 3, 4)
    roi.T.plot(grid=True, ax=ax4)
    plt.axhline(y=1, color='red', linestyle='--', label='Уровень окупаемости')
    plt.legend()
    plt.xlabel('Лайфтайм')
    plt.title('ROI')

    # пятый график — динамика roi
    ax5 = plt.subplot(2, 3, 5, sharey=ax4)
    # столбцами сводной таблицы станут все столбцы индекса, кроме даты
    columns = [name for name in roi_history.index.names if name not in ['dt']]
    filtered_data = roi_history.pivot_table(
        index='dt', columns=columns, values=horizon - 1, aggfunc='mean'
    )
    filter_data(filtered_data, window).plot(grid=True, ax=ax5)
    plt.axhline(y=1, color='red', linestyle='--', label='Уровень окупаемости')
    plt.xlabel('Дата привлечения')
    plt.title('Динамика ROI пользователей на {}-й день'.format(horizon))

    plt.tight_layout()
    plt.show()
```


```python
# становим момент и горизонт анализа данных.
observation_date = datetime(2019, 11, 1).date()  # момент анализа
horizon_days = 14  # горизонт анализа 
```

## Шаг 3. Исследовательский анализ данных
[Содержание](#intro)

Построим профили пользователей. Определим минимальную и максимальную дату привлечения пользователей.

Выясним:
- Из каких стран приходят посетители? Какие страны дают больше всего платящих пользователей?
- Какими устройствами они пользуются? С каких устройств чаще всего заходят платящие пользователи?
- По каким рекламным каналам шло привлечение пользователей? Какие каналы приносят больше всего платящих пользователей?

**Построим профили пользователей**


```python
profiles = get_profiles(visits, orders, costs)
profiles.head(5)
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
      <th>first_ts</th>
      <th>channel</th>
      <th>device</th>
      <th>region</th>
      <th>dt</th>
      <th>month</th>
      <th>payer</th>
      <th>acquisition_cost</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>599326</td>
      <td>2019-05-07 20:58:57</td>
      <td>FaceBoom</td>
      <td>Mac</td>
      <td>United States</td>
      <td>2019-05-07</td>
      <td>2019-05-01</td>
      <td>True</td>
      <td>1.088172</td>
    </tr>
    <tr>
      <th>1</th>
      <td>4919697</td>
      <td>2019-07-09 12:46:07</td>
      <td>FaceBoom</td>
      <td>iPhone</td>
      <td>United States</td>
      <td>2019-07-09</td>
      <td>2019-07-01</td>
      <td>False</td>
      <td>1.107237</td>
    </tr>
    <tr>
      <th>2</th>
      <td>6085896</td>
      <td>2019-10-01 09:58:33</td>
      <td>organic</td>
      <td>iPhone</td>
      <td>France</td>
      <td>2019-10-01</td>
      <td>2019-10-01</td>
      <td>False</td>
      <td>0.000000</td>
    </tr>
    <tr>
      <th>3</th>
      <td>22593348</td>
      <td>2019-08-22 21:35:48</td>
      <td>AdNonSense</td>
      <td>PC</td>
      <td>Germany</td>
      <td>2019-08-22</td>
      <td>2019-08-01</td>
      <td>False</td>
      <td>0.988235</td>
    </tr>
    <tr>
      <th>4</th>
      <td>31989216</td>
      <td>2019-10-02 00:07:44</td>
      <td>YRabbit</td>
      <td>iPhone</td>
      <td>United States</td>
      <td>2019-10-02</td>
      <td>2019-10-01</td>
      <td>False</td>
      <td>0.230769</td>
    </tr>
  </tbody>
</table>
</div>



**Определим минимальную и максимальную дату привлечения пользователей**


```python
min_date = profiles['dt'].min()
max_date = profiles['dt'].max() 
print('Даты привлечения пользователей: {} - {}'.format(min_date, max_date))
```

    Даты привлечения пользователей: 2019-05-01 - 2019-10-27


**Выясним из каких стран приходят посетители и какие страны дают больше всего платящих пользователей.**


```python
# сгрупируем по странам, вычислим долю и отсортируем пользователей
region = (
    profiles
    .pivot_table(index='region', values='payer', aggfunc=['count', 'sum', 'mean'])
    .reset_index()
    .droplevel(1, axis=1)
    .rename(columns={'count': 'all_users', 'sum': 'paying_users', 'mean': 'ratio'})
    .sort_values(by='ratio', ascending=False)
    .style.format({'ratio': '{:.2%}'})
)
region
```




<style  type="text/css" >
</style><table id="T_0b791_" ><thead>    <tr>        <th class="blank level0" ></th>        <th class="col_heading level0 col0" >region</th>        <th class="col_heading level0 col1" >all_users</th>        <th class="col_heading level0 col2" >paying_users</th>        <th class="col_heading level0 col3" >ratio</th>    </tr></thead><tbody>
                <tr>
                        <th id="T_0b791_level0_row0" class="row_heading level0 row0" >3</th>
                        <td id="T_0b791_row0_col0" class="data row0 col0" >United States</td>
                        <td id="T_0b791_row0_col1" class="data row0 col1" >100002</td>
                        <td id="T_0b791_row0_col2" class="data row0 col2" >6902</td>
                        <td id="T_0b791_row0_col3" class="data row0 col3" >6.90%</td>
            </tr>
            <tr>
                        <th id="T_0b791_level0_row1" class="row_heading level0 row1" >1</th>
                        <td id="T_0b791_row1_col0" class="data row1 col0" >Germany</td>
                        <td id="T_0b791_row1_col1" class="data row1 col1" >14981</td>
                        <td id="T_0b791_row1_col2" class="data row1 col2" >616</td>
                        <td id="T_0b791_row1_col3" class="data row1 col3" >4.11%</td>
            </tr>
            <tr>
                        <th id="T_0b791_level0_row2" class="row_heading level0 row2" >2</th>
                        <td id="T_0b791_row2_col0" class="data row2 col0" >UK</td>
                        <td id="T_0b791_row2_col1" class="data row2 col1" >17575</td>
                        <td id="T_0b791_row2_col2" class="data row2 col2" >700</td>
                        <td id="T_0b791_row2_col3" class="data row2 col3" >3.98%</td>
            </tr>
            <tr>
                        <th id="T_0b791_level0_row3" class="row_heading level0 row3" >0</th>
                        <td id="T_0b791_row3_col0" class="data row3 col0" >France</td>
                        <td id="T_0b791_row3_col1" class="data row3 col1" >17450</td>
                        <td id="T_0b791_row3_col2" class="data row3 col2" >663</td>
                        <td id="T_0b791_row3_col3" class="data row3 col3" >3.80%</td>
            </tr>
    </tbody></table>



По таблице видно, что пользователи приходят из United States, Germany, UK, France.  
Больше всего платящих пользователей в United States.

**Узнаем какими устройствами они пользуются и с каких устройств чаще всего заходят платящие пользователи**


```python
device = (
    profiles
    .pivot_table(index='device', values='payer', aggfunc=['count', 'sum', 'mean'])
    .reset_index()
    .droplevel(1, axis=1)
    .rename(columns={'count': 'all_users', 'sum': 'paying_users', 'mean': 'ratio'})
    .sort_values(by='ratio', ascending=False)
    .style.format({'ratio': '{:.2%}'})
)
device
```




<style  type="text/css" >
</style><table id="T_9d157_" ><thead>    <tr>        <th class="blank level0" ></th>        <th class="col_heading level0 col0" >device</th>        <th class="col_heading level0 col1" >all_users</th>        <th class="col_heading level0 col2" >paying_users</th>        <th class="col_heading level0 col3" >ratio</th>    </tr></thead><tbody>
                <tr>
                        <th id="T_9d157_level0_row0" class="row_heading level0 row0" >1</th>
                        <td id="T_9d157_row0_col0" class="data row0 col0" >Mac</td>
                        <td id="T_9d157_row0_col1" class="data row0 col1" >30042</td>
                        <td id="T_9d157_row0_col2" class="data row0 col2" >1912</td>
                        <td id="T_9d157_row0_col3" class="data row0 col3" >6.36%</td>
            </tr>
            <tr>
                        <th id="T_9d157_level0_row1" class="row_heading level0 row1" >3</th>
                        <td id="T_9d157_row1_col0" class="data row1 col0" >iPhone</td>
                        <td id="T_9d157_row1_col1" class="data row1 col1" >54479</td>
                        <td id="T_9d157_row1_col2" class="data row1 col2" >3382</td>
                        <td id="T_9d157_row1_col3" class="data row1 col3" >6.21%</td>
            </tr>
            <tr>
                        <th id="T_9d157_level0_row2" class="row_heading level0 row2" >0</th>
                        <td id="T_9d157_row2_col0" class="data row2 col0" >Android</td>
                        <td id="T_9d157_row2_col1" class="data row2 col1" >35032</td>
                        <td id="T_9d157_row2_col2" class="data row2 col2" >2050</td>
                        <td id="T_9d157_row2_col3" class="data row2 col3" >5.85%</td>
            </tr>
            <tr>
                        <th id="T_9d157_level0_row3" class="row_heading level0 row3" >2</th>
                        <td id="T_9d157_row3_col0" class="data row3 col0" >PC</td>
                        <td id="T_9d157_row3_col1" class="data row3 col1" >30455</td>
                        <td id="T_9d157_row3_col2" class="data row3 col2" >1537</td>
                        <td id="T_9d157_row3_col3" class="data row3 col3" >5.05%</td>
            </tr>
    </tbody></table>



Используемые устройства: Mac, iPhone, Android, PC.
Чаще всего платящие пользователи используют Mac.

**Посмотрим по каким рекламным каналам шло привлечение пользователей. Какие каналы приносят больше всего платящих пользователей.**


```python
channel = (
    profiles
    .pivot_table(index='channel', values='payer', aggfunc=['count', 'sum', 'mean'])
    .reset_index()
    .droplevel(1, axis=1)
    .rename(columns={'count': 'all_users', 'sum': 'paying_users', 'mean': 'ratio'})
    .sort_values(by='ratio', ascending=False)
    .style.format({'ratio': '{:.2%}'})
)
channel
```




<style  type="text/css" >
</style><table id="T_03d76_" ><thead>    <tr>        <th class="blank level0" ></th>        <th class="col_heading level0 col0" >channel</th>        <th class="col_heading level0 col1" >all_users</th>        <th class="col_heading level0 col2" >paying_users</th>        <th class="col_heading level0 col3" >ratio</th>    </tr></thead><tbody>
                <tr>
                        <th id="T_03d76_level0_row0" class="row_heading level0 row0" >1</th>
                        <td id="T_03d76_row0_col0" class="data row0 col0" >FaceBoom</td>
                        <td id="T_03d76_row0_col1" class="data row0 col1" >29144</td>
                        <td id="T_03d76_row0_col2" class="data row0 col2" >3557</td>
                        <td id="T_03d76_row0_col3" class="data row0 col3" >12.20%</td>
            </tr>
            <tr>
                        <th id="T_03d76_level0_row1" class="row_heading level0 row1" >0</th>
                        <td id="T_03d76_row1_col0" class="data row1 col0" >AdNonSense</td>
                        <td id="T_03d76_row1_col1" class="data row1 col1" >3880</td>
                        <td id="T_03d76_row1_col2" class="data row1 col2" >440</td>
                        <td id="T_03d76_row1_col3" class="data row1 col3" >11.34%</td>
            </tr>
            <tr>
                        <th id="T_03d76_level0_row2" class="row_heading level0 row2" >9</th>
                        <td id="T_03d76_row2_col0" class="data row2 col0" >lambdaMediaAds</td>
                        <td id="T_03d76_row2_col1" class="data row2 col1" >2149</td>
                        <td id="T_03d76_row2_col2" class="data row2 col2" >225</td>
                        <td id="T_03d76_row2_col3" class="data row2 col3" >10.47%</td>
            </tr>
            <tr>
                        <th id="T_03d76_level0_row3" class="row_heading level0 row3" >6</th>
                        <td id="T_03d76_row3_col0" class="data row3 col0" >TipTop</td>
                        <td id="T_03d76_row3_col1" class="data row3 col1" >19561</td>
                        <td id="T_03d76_row3_col2" class="data row3 col2" >1878</td>
                        <td id="T_03d76_row3_col3" class="data row3 col3" >9.60%</td>
            </tr>
            <tr>
                        <th id="T_03d76_level0_row4" class="row_heading level0 row4" >5</th>
                        <td id="T_03d76_row4_col0" class="data row4 col0" >RocketSuperAds</td>
                        <td id="T_03d76_row4_col1" class="data row4 col1" >4448</td>
                        <td id="T_03d76_row4_col2" class="data row4 col2" >352</td>
                        <td id="T_03d76_row4_col3" class="data row4 col3" >7.91%</td>
            </tr>
            <tr>
                        <th id="T_03d76_level0_row5" class="row_heading level0 row5" >7</th>
                        <td id="T_03d76_row5_col0" class="data row5 col0" >WahooNetBanner</td>
                        <td id="T_03d76_row5_col1" class="data row5 col1" >8553</td>
                        <td id="T_03d76_row5_col2" class="data row5 col2" >453</td>
                        <td id="T_03d76_row5_col3" class="data row5 col3" >5.30%</td>
            </tr>
            <tr>
                        <th id="T_03d76_level0_row6" class="row_heading level0 row6" >8</th>
                        <td id="T_03d76_row6_col0" class="data row6 col0" >YRabbit</td>
                        <td id="T_03d76_row6_col1" class="data row6 col1" >4312</td>
                        <td id="T_03d76_row6_col2" class="data row6 col2" >165</td>
                        <td id="T_03d76_row6_col3" class="data row6 col3" >3.83%</td>
            </tr>
            <tr>
                        <th id="T_03d76_level0_row7" class="row_heading level0 row7" >3</th>
                        <td id="T_03d76_row7_col0" class="data row7 col0" >MediaTornado</td>
                        <td id="T_03d76_row7_col1" class="data row7 col1" >4364</td>
                        <td id="T_03d76_row7_col2" class="data row7 col2" >156</td>
                        <td id="T_03d76_row7_col3" class="data row7 col3" >3.57%</td>
            </tr>
            <tr>
                        <th id="T_03d76_level0_row8" class="row_heading level0 row8" >2</th>
                        <td id="T_03d76_row8_col0" class="data row8 col0" >LeapBob</td>
                        <td id="T_03d76_row8_col1" class="data row8 col1" >8553</td>
                        <td id="T_03d76_row8_col2" class="data row8 col2" >262</td>
                        <td id="T_03d76_row8_col3" class="data row8 col3" >3.06%</td>
            </tr>
            <tr>
                        <th id="T_03d76_level0_row9" class="row_heading level0 row9" >4</th>
                        <td id="T_03d76_row9_col0" class="data row9 col0" >OppleCreativeMedia</td>
                        <td id="T_03d76_row9_col1" class="data row9 col1" >8605</td>
                        <td id="T_03d76_row9_col2" class="data row9 col2" >233</td>
                        <td id="T_03d76_row9_col3" class="data row9 col3" >2.71%</td>
            </tr>
            <tr>
                        <th id="T_03d76_level0_row10" class="row_heading level0 row10" >10</th>
                        <td id="T_03d76_row10_col0" class="data row10 col0" >organic</td>
                        <td id="T_03d76_row10_col1" class="data row10 col1" >56439</td>
                        <td id="T_03d76_row10_col2" class="data row10 col2" >1160</td>
                        <td id="T_03d76_row10_col3" class="data row10 col3" >2.06%</td>
            </tr>
    </tbody></table>



В таблице видим весь перечень каналов, по которым приходили пользователи.  
Самый большое значение платящих пользователей у FaceBoom - 12,2%, на втором месте - AdNonSense 11,34%, на третьем месте - lambdaMediaAds 10.47%.  
Так же видим болшое количество "органических" пользователей.

**Вывод:**
- Диапазон привлечения клиентов 2019-05-01 - 2019-10-27
- По таблице видно, что пользователи приходят из United States, Germany, UK, France. Больше всего платящих пользователей в United States - 6,9%.
- Используемые устройства: Mac, iPhone, Android, PC. Чаще всего платящие пользователи используют Mac - 6,36%.
- Самый большое значение платящих пользователей у канала FaceBoom - 12,2%, на втором месте - AdNonSense 11,34%, на третьем месте - lambdaMediaAds 10.47%. Так же видим болшое количество "органических" пользователей.

## Шаг 4. Маркетинг
[Содержание](#intro)

### Затраты на рекламу

**Выясним сколько денег потратили? Всего / на каждый источник / по времени.**


```python
# всего
print('Затрачено денег всего:', profiles['acquisition_cost'].sum().round(1))
```

    Затрачено денег всего: 105497.3



```python
# на каждый источник
channel_costs = (
    profiles.pivot_table(index='channel', values='acquisition_cost', aggfunc='sum')
)
channel_costs['ratio'] = channel_costs['acquisition_cost'] / profiles['acquisition_cost'].sum() * 100
channel_costs = channel_costs.sort_values(by='ratio', ascending=False).style.format({'ratio': '{:.2f}%', 'acquisition_cost': '{:.2f}'})
print('Затрачено денег на каждый источник:')
display(channel_costs)
```

    Затрачено денег на каждый источник:



<style  type="text/css" >
</style><table id="T_b60f3_" ><thead>    <tr>        <th class="blank level0" ></th>        <th class="col_heading level0 col0" >acquisition_cost</th>        <th class="col_heading level0 col1" >ratio</th>    </tr>    <tr>        <th class="index_name level0" >channel</th>        <th class="blank" ></th>        <th class="blank" ></th>    </tr></thead><tbody>
                <tr>
                        <th id="T_b60f3_level0_row0" class="row_heading level0 row0" >TipTop</th>
                        <td id="T_b60f3_row0_col0" class="data row0 col0" >54751.30</td>
                        <td id="T_b60f3_row0_col1" class="data row0 col1" >51.90%</td>
            </tr>
            <tr>
                        <th id="T_b60f3_level0_row1" class="row_heading level0 row1" >FaceBoom</th>
                        <td id="T_b60f3_row1_col0" class="data row1 col0" >32445.60</td>
                        <td id="T_b60f3_row1_col1" class="data row1 col1" >30.75%</td>
            </tr>
            <tr>
                        <th id="T_b60f3_level0_row2" class="row_heading level0 row2" >WahooNetBanner</th>
                        <td id="T_b60f3_row2_col0" class="data row2 col0" >5151.00</td>
                        <td id="T_b60f3_row2_col1" class="data row2 col1" >4.88%</td>
            </tr>
            <tr>
                        <th id="T_b60f3_level0_row3" class="row_heading level0 row3" >AdNonSense</th>
                        <td id="T_b60f3_row3_col0" class="data row3 col0" >3911.25</td>
                        <td id="T_b60f3_row3_col1" class="data row3 col1" >3.71%</td>
            </tr>
            <tr>
                        <th id="T_b60f3_level0_row4" class="row_heading level0 row4" >OppleCreativeMedia</th>
                        <td id="T_b60f3_row4_col0" class="data row4 col0" >2151.25</td>
                        <td id="T_b60f3_row4_col1" class="data row4 col1" >2.04%</td>
            </tr>
            <tr>
                        <th id="T_b60f3_level0_row5" class="row_heading level0 row5" >RocketSuperAds</th>
                        <td id="T_b60f3_row5_col0" class="data row5 col0" >1833.00</td>
                        <td id="T_b60f3_row5_col1" class="data row5 col1" >1.74%</td>
            </tr>
            <tr>
                        <th id="T_b60f3_level0_row6" class="row_heading level0 row6" >LeapBob</th>
                        <td id="T_b60f3_row6_col0" class="data row6 col0" >1797.60</td>
                        <td id="T_b60f3_row6_col1" class="data row6 col1" >1.70%</td>
            </tr>
            <tr>
                        <th id="T_b60f3_level0_row7" class="row_heading level0 row7" >lambdaMediaAds</th>
                        <td id="T_b60f3_row7_col0" class="data row7 col0" >1557.60</td>
                        <td id="T_b60f3_row7_col1" class="data row7 col1" >1.48%</td>
            </tr>
            <tr>
                        <th id="T_b60f3_level0_row8" class="row_heading level0 row8" >MediaTornado</th>
                        <td id="T_b60f3_row8_col0" class="data row8 col0" >954.48</td>
                        <td id="T_b60f3_row8_col1" class="data row8 col1" >0.90%</td>
            </tr>
            <tr>
                        <th id="T_b60f3_level0_row9" class="row_heading level0 row9" >YRabbit</th>
                        <td id="T_b60f3_row9_col0" class="data row9 col0" >944.22</td>
                        <td id="T_b60f3_row9_col1" class="data row9 col1" >0.90%</td>
            </tr>
            <tr>
                        <th id="T_b60f3_level0_row10" class="row_heading level0 row10" >organic</th>
                        <td id="T_b60f3_row10_col0" class="data row10 col0" >0.00</td>
                        <td id="T_b60f3_row10_col1" class="data row10 col1" >0.00%</td>
            </tr>
    </tbody></table>



```python
# по времени
profiles = profiles.query('channel != "organic"')

channel_time = profiles.pivot_table(index='month', columns='channel', values='acquisition_cost', aggfunc='sum')

channel_time.plot(figsize=(15, 10), grid=True, style='o-') 

plt.yticks(np.arange(0, 14000, 1000)) # зададим шаг по оси y
plt.title('График затрат на маркетинг по каналам')
plt.xlabel('Дата')
plt.ylabel('Затраты')
plt.show()
```


    
![png](output_45_0.png)
    



```python
# посмотрим на график без выделяющихся значений
channel_time = (
    profiles[~profiles['channel'].isin(['TipTop', 'FaceBoom'])]
    .pivot_table(index='month', columns='channel', values='acquisition_cost', aggfunc='sum')
)
channel_time.plot(figsize=(15, 10), grid=True, style='o-') 

plt.yticks(np.arange(0, 1500, 100)) 
plt.title('График затрат на маркетинг по каналам')
plt.xlabel('Дата')
plt.ylabel('Затраты')
plt.show()
```


    
![png](output_46_0.png)
    


Общая сумма на рекламу - 105497.3  
Половина этой суммы потрачено на TipTop - 51,9%, следом  FaceBoom - 30,75%, третье место - WahooNetBanner - 4,88%. Organic не имеет расходов, поэтому его убрали из анализа.  

Как отметили ранее, основные траты на рекламу идут по каналам TipTop и FaceBoom. По каналу TipTop сумма в мае была примерно 3000, а в июне уже больше 6500. В сентябре было достигнуто максимальное значение - около 13000.  
Так же затраты на каналы в июне выросли у WahooNetBanner, LeapBob, OppleCreativeMedia. По остальным источникам траты снижались. Почти в 2 раза упало финансирование AdNonSense.

### Стоимость привлечения клиента
[Содержание](#intro)

Посмотрим сколько в среднем стоило привлечение одного покупателя из каждого источника и средний CAC на одного пользователя для всего проекта.


```python
cac_channel = (
    profiles
    .groupby('channel')
    .agg({'user_id': 'count', 'acquisition_cost': 'sum'})
    )
cac_channel['cac'] = (cac_channel['acquisition_cost'] / cac_channel['user_id']).round(2)
cac_channel.sort_values(by='cac', ascending=False)
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
      <th>acquisition_cost</th>
      <th>cac</th>
    </tr>
    <tr>
      <th>channel</th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>TipTop</th>
      <td>19561</td>
      <td>54751.30</td>
      <td>2.80</td>
    </tr>
    <tr>
      <th>FaceBoom</th>
      <td>29144</td>
      <td>32445.60</td>
      <td>1.11</td>
    </tr>
    <tr>
      <th>AdNonSense</th>
      <td>3880</td>
      <td>3911.25</td>
      <td>1.01</td>
    </tr>
    <tr>
      <th>lambdaMediaAds</th>
      <td>2149</td>
      <td>1557.60</td>
      <td>0.72</td>
    </tr>
    <tr>
      <th>WahooNetBanner</th>
      <td>8553</td>
      <td>5151.00</td>
      <td>0.60</td>
    </tr>
    <tr>
      <th>RocketSuperAds</th>
      <td>4448</td>
      <td>1833.00</td>
      <td>0.41</td>
    </tr>
    <tr>
      <th>OppleCreativeMedia</th>
      <td>8605</td>
      <td>2151.25</td>
      <td>0.25</td>
    </tr>
    <tr>
      <th>MediaTornado</th>
      <td>4364</td>
      <td>954.48</td>
      <td>0.22</td>
    </tr>
    <tr>
      <th>YRabbit</th>
      <td>4312</td>
      <td>944.22</td>
      <td>0.22</td>
    </tr>
    <tr>
      <th>LeapBob</th>
      <td>8553</td>
      <td>1797.60</td>
      <td>0.21</td>
    </tr>
  </tbody>
</table>
</div>



Самое дорогое привлечение пользователей у TipTop - 2,8  
Дешевле всего обходятся пользователи у LeapBob	- 0,21


```python
cac_all = (profiles['acquisition_cost'].sum() / profiles['user_id'].count()).round(2)
print('Cредний CAC на одного пользователя для всего проекта:', cac_all)
```

    Cредний CAC на одного пользователя для всего проекта: 1.13


**Вывод:**

Общая сумма на рекламу - 105497.3  
Половина этой суммы потрачено на TipTop - 51,9%, следом FaceBoom - 30,75%, третье место - WahooNetBanner - 4,88%. Organic не имеет расходов, поэтому его убрали из анализа.

Как отметили ранее, основные траты на рекламу идут по каналам TipTop и FaceBoom. По каналу TipTop сумма в мае была примерно 3000, а в июне уже больше 6500. В сентябре было достигнуто максимальное значение - около 13000.  
Так же затраты на каналы в июне выросли у WahooNetBanner, LeapBob, OppleCreativeMedia. По остальным источникам траты снижались. Почти в 2 раза упало финансирование AdNonSense.

Самое дорогое привлечение пользователей у TipTop - 2,8  
Дешевле всего обходятся пользователи у LeapBob - 0,21  
Cредний CAC на одного пользователя для всего проекта: 1.13

## Шаг 5. Оценка окупаемости рекламы для привлечения пользователей
[Содержание](#intro)

С помощью LTV и ROI:
- Проанализируем общую окупаемость рекламы;
- Проанализируем окупаемость рекламы с разбивкой по устройствам;
- Проанализируем окупаемость рекламы с разбивкой по странам;
- Проанализируем окупаемость рекламы с разбивкой по рекламным каналам.

### Анализ общей окупаемости рекламы
[Содержание](#intro)


```python
# проанализируем общую окупаемость рекламы, считаем LTV и ROI
ltv_raw, ltv_grouped, ltv_history, roi_grouped, roi_history = get_ltv(
    profiles, orders, observation_date, horizon_days
)
# строим графики
plot_ltv_roi(ltv_grouped, ltv_history, roi_grouped, roi_history, horizon_days) 
```


    
![png](output_56_0.png)
    


По графикам можно сделать такие вывод:
- Реклама не окупается. ROI в конце недели — чуть выше 80%.
- CAC растет. Возможно, дело в увеличении рекламного бюджета. С 2019-06 при резком увеличении трат на рекламу, ROI пошло ниже уровня окупаемости.
- Показатель LTV достаточно стабилен. Значит, дело не в ухудшении качества пользователей.

Чтобы разобраться в причинах, пройдём по всем доступным характеристикам пользователей — стране, источнику и устройству первого посещения.

### Анализ окупаемости рекламы с разбивкой по устройствам
[Содержание](#intro)


```python
# проанализируем окупаемость рекламы с разбивкой по устройствам
dimensions = ['device']

ltv_raw, ltv_grouped, ltv_history, roi_grouped, roi_history = get_ltv(
    profiles, orders, observation_date, horizon_days, dimensions=dimensions
)
plot_ltv_roi(
    ltv_grouped, ltv_history, roi_grouped, roi_history, horizon_days, window=14
) 
```


    
![png](output_59_0.png)
    


Пользователи пользуются следующими устройствами:
- Android,
- Mac,
- PC,
- iPhone. 

По графику LTV видно, что платящиеся пользователи чаще всего заходят из iPhone и Mac, Android и меньше всего PC. Но разница у них очень маленькая. В отличии от трат на рекламу.  
По CAC и ROI видны различия - чем меньше стоимость привлечения пользователей, тем больше вероятность окупить затраты. Так на 11 лайфтайм пользователи PC окупились полностью, Android - на 85%, а Mac и iPhone - на 70%.


```python
# конверсия с разбивкой по устройствам
conversion_raw, conversion_grouped, conversion_history = get_conversion(
    profiles, orders, observation_date, horizon_days, dimensions=dimensions
)
plot_conversion(conversion_grouped, conversion_history, horizon_days) 
```


    
![png](output_61_0.png)
    


Судя по графикам, пользователи Mac и iPhone конвертируются очень хорошо, причём постоянно. Видимо, дело в удержании.


```python
# удержание с разбивкой по устройствам
retention_raw, retention_grouped, retention_history = get_retention(
    profiles, visits, observation_date, horizon_days, dimensions=dimensions
)
plot_retention(retention_grouped, retention_history, horizon_days) 
```


    
![png](output_63_0.png)
    


Удержание платящих по всем устройтсвам равномерное.

### Анализ окупаемости рекламы с разбивкой по странам
[Содержание](#intro)


```python
# проанализируем окупаемость рекламы с разбивкой по странам
dimensions = ['region']

ltv_raw, ltv_grouped, ltv_history, roi_grouped, roi_history = get_ltv(
    profiles, orders, observation_date, horizon_days, dimensions=dimensions
)
plot_ltv_roi(
    ltv_grouped, ltv_history, roi_grouped, roi_history, horizon_days, window=14
) 
```


    
![png](output_66_0.png)
    


По графикам стран видно:
- Показатель LTV стабилен. Больше всего платящих пользователей из United States, потом почти одной линией идут пользователи из UK, Germany и ниже всех France. LTV подвержен сезонности.
- Заметен аномальный рост CAC для United States мая 2019.
- ROI показывает, что реклама в United States не окупается (60%).
- Динамика ROI показывает, что с 2019-05 при резком увеличении трат на рекламу, ROI вышла за уровнь окупаемости.
- Быстрее всего окупаются пользователи из UK (с 4 лайфтайма), потом Germany и France (на 5 лайфтайм.)
Пользователи из United States окупаются на 70%


```python
profiles.head(5)
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
      <th>first_ts</th>
      <th>channel</th>
      <th>device</th>
      <th>region</th>
      <th>dt</th>
      <th>month</th>
      <th>payer</th>
      <th>acquisition_cost</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>599326</td>
      <td>2019-05-07 20:58:57</td>
      <td>FaceBoom</td>
      <td>Mac</td>
      <td>United States</td>
      <td>2019-05-07</td>
      <td>2019-05-01</td>
      <td>True</td>
      <td>1.088172</td>
    </tr>
    <tr>
      <th>1</th>
      <td>4919697</td>
      <td>2019-07-09 12:46:07</td>
      <td>FaceBoom</td>
      <td>iPhone</td>
      <td>United States</td>
      <td>2019-07-09</td>
      <td>2019-07-01</td>
      <td>False</td>
      <td>1.107237</td>
    </tr>
    <tr>
      <th>3</th>
      <td>22593348</td>
      <td>2019-08-22 21:35:48</td>
      <td>AdNonSense</td>
      <td>PC</td>
      <td>Germany</td>
      <td>2019-08-22</td>
      <td>2019-08-01</td>
      <td>False</td>
      <td>0.988235</td>
    </tr>
    <tr>
      <th>4</th>
      <td>31989216</td>
      <td>2019-10-02 00:07:44</td>
      <td>YRabbit</td>
      <td>iPhone</td>
      <td>United States</td>
      <td>2019-10-02</td>
      <td>2019-10-01</td>
      <td>False</td>
      <td>0.230769</td>
    </tr>
    <tr>
      <th>7</th>
      <td>46006712</td>
      <td>2019-06-30 03:46:29</td>
      <td>AdNonSense</td>
      <td>Android</td>
      <td>France</td>
      <td>2019-06-30</td>
      <td>2019-06-01</td>
      <td>True</td>
      <td>1.008000</td>
    </tr>
  </tbody>
</table>
</div>




```python
usa_profiles = profiles.query('region == "United States"')

usa_profiles = usa_profiles.groupby('month').agg({'user_id': 'count', 'acquisition_cost': 'sum', 'payer': 'sum'})
usa_profiles['p/u'] = usa_profiles['payer'] / usa_profiles['user_id']
usa_profiles.sort_values('payer')
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
      <th>acquisition_cost</th>
      <th>payer</th>
      <th>p/u</th>
    </tr>
    <tr>
      <th>month</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>2019-05-01</th>
      <td>8553</td>
      <td>7621.640</td>
      <td>748</td>
      <td>0.087455</td>
    </tr>
    <tr>
      <th>2019-07-01</th>
      <td>9777</td>
      <td>14192.430</td>
      <td>987</td>
      <td>0.100951</td>
    </tr>
    <tr>
      <th>2019-06-01</th>
      <td>10005</td>
      <td>12746.615</td>
      <td>1018</td>
      <td>0.101749</td>
    </tr>
    <tr>
      <th>2019-10-01</th>
      <td>10667</td>
      <td>18495.795</td>
      <td>1045</td>
      <td>0.097966</td>
    </tr>
    <tr>
      <th>2019-08-01</th>
      <td>11416</td>
      <td>18008.810</td>
      <td>1139</td>
      <td>0.099772</td>
    </tr>
    <tr>
      <th>2019-09-01</th>
      <td>11411</td>
      <td>19863.310</td>
      <td>1171</td>
      <td>0.102620</td>
    </tr>
  </tbody>
</table>
</div>




```python
# конверсия с разбивкой по странам
conversion_raw, conversion_grouped, conversion_history = get_conversion(
    profiles, orders, observation_date, horizon_days, dimensions=dimensions
)

plot_conversion(conversion_grouped, conversion_history, horizon_days) 
```


    
![png](output_70_0.png)
    


Самая высокая конверсия у United States, посмотрим удержание:


```python
# удержание с разбивкой по странам
retention_raw, retention_grouped, retention_history = get_retention(
    profiles, visits, observation_date, horizon_days, dimensions=dimensions
)
plot_retention(retention_grouped, retention_history, horizon_days) 
```


    
![png](output_72_0.png)
    


А вот удержание пользователей в United States самое низкое. Скорее всего, причина в какой-нибудь технической проблеме.

### Анализ окупаемости рекламы с разбивкой по рекламным каналам
[Содержание](#intro)


```python
# проанализируем окупаемость рекламы с разбивкой по рекламным каналам
dimensions = ['channel']

ltv_raw, ltv_grouped, ltv_history, roi_grouped, roi_history = get_ltv(
    profiles, orders, observation_date, horizon_days, dimensions=dimensions
)
plot_ltv_roi(
    ltv_grouped, ltv_history, roi_grouped, roi_history, horizon_days, window=14
) 
```


    
![png](output_75_0.png)
    


По графику LTV максимум у LambdaMediaAds, на втором месте - TipTop.  
По графику CAC динамика стоимости привлечения пользователей по каналу TipTop растет аномольно быстро.  
Самый быстро окупаемый канал - YRabbit.  
Канал TipTop окупается примерно на 50%, а FaceBoom и AdNonSence окупаются на 80%.  


```python
# конверсия с разбивкой по каналам
conversion_raw, conversion_grouped, conversion_history = get_conversion(
    profiles, orders, observation_date, horizon_days, dimensions=dimensions
)
plot_conversion(conversion_grouped, conversion_history, horizon_days) 
```


    
![png](output_77_0.png)
    


Лучшая конверсия пользователей у канала FaceBoom, следом AdNonSense и lambdaMediaAds.


```python
# удержание с разбивкой по рекламным каналам
retention_raw, retention_grouped, retention_history = get_retention(
    profiles, visits, observation_date, horizon_days, dimensions=dimensions
)
plot_retention(retention_grouped, retention_history, horizon_days) 
```


    
![png](output_79_0.png)
    


А вот удержание платящих пользователей по каналам FaceBoom и AdNonSense самое низкое.

**Вывод:**

По графикам можно сделать такие вывод:
- Реклама не окупается. ROI в конце недели — чуть выше 80%.
- CAC растет. Возможно, дело в увеличении рекламного бюджета. С 2019-06 при резком увеличении трат на рекламу, ROI пошло ниже уровня окупаемости.
- Показатель LTV достаточно стабилен. Значит, дело не в ухудшении качества пользователей.

Чтобы разобраться в причинах, пройдём по всем доступным характеристикам пользователей — стране, источнику и устройству первого посещения.

Пользователи пользуются следующими устройствами:
- Android,
- Mac,
- PC,
- iPhone. 

По графику LTV видно, что платящиеся пользователи чаще всего заходят из iPhone и Mac, Android и меньше всего PC. Но разница у них очень маленькая. В отличии от трат на рекламу.  
По CAC и ROI видны различия - чем меньше стоимость привлечения пользователей, тем больше вероятность окупить затраты. Так на 11 лайфтайм пользователи PC окупились полностью, Android - на 85%, а Mac и iPhone - на 70%.  
Судя по графикам, пользователи Mac и iPhone конвертируются очень хорошо, причём постоянно. Видимо, дело в удержании.  
Удержание платящих по всем устройтсвам равномерное.

По графикам стран видно:
- Показатель LTV стабилен. Больше всего платящих пользователей из United States, потом почти одной линией идут пользователи из UK, Germany и ниже всех France. LTV подвержен сезонности.
- Заметен аномальный рост CAC для United States мая 2019.
- ROI показывает, что реклама в United States не окупается (60%).
- Динамика ROI показывает, что с 2019-05 при резком увеличении трат на рекламу, ROI вышла за уровнь окупаемости.
- Быстрее всего окупаются пользователи из UK (с 4 лайфтайма), потом Germany и France (на 5 лайфтайм.)
Пользователи из United States окупаются на 70%  
Самая высокая конверсия у United States. А вот удержание пользователей в United States самое низкое. Скорее всего, причина в какой-нибудь технической проблеме.  

По графику LTV максимум у LambdaMediaAds, на втором месте - TipTop.  
По графику CAC динамика стоимости привлечения пользователей по каналу TipTop растет аномольно быстро.  
Самый быстро окупаемый канал - YRabbit.  
Канал TipTop окупается примерно на 50%, а FaceBoom и AdNonSence окупаются на 80%.  
Лучшая конверсия пользователей у канала FaceBoom, следом AdNonSense и lambdaMediaAds. А вот удержание платящих пользователей по каналам FaceBoom и AdNonSense самое низкое.

# Шаг 6. Общий вывод
[Содержание](#intro)

Причины неэффективности привлечения пользователей:

Расходы на рекламу по устройствам iPhone и Mac завышенные.  
Завышенные расходы на рекламу в стране United States. Хоть конверсия высокая, но окупаемости нет и удержание платящих в этой стране крайне маленькая.  
Крайне завышенные траты на канал TipTop.  
Слабые показатели удержания у канала AdNonSense и FaceBoom.

Рекомендации для отдела маркетинг:
- Снизить расходы всех девайсов до уровня PC. Т.к. «пожизненная ценность» клиента по этим устройствам почти одинаковы. Хотя у РС ниже, но окупается через 2 недели, в отличии от других.
- Снизить расходы на рекламу в США до уровня других стран.
- Снизить расходы на каналы TipTop, AdNonSense и FaceBoom до уровня 0,5.
- Обратить особое внимание на канал YRabbit - приносит большую прибыль и крайне быстро окупается. Стоит увеличить бюджет.
- Высокую «пожизненная ценность» клиента имеют каналы lambdaMediaAds, WahooNetBanner и RocketSuperAds. Здесь тоже можно увеличить рекламный бюджет.

[Содержание](#intro)
