# Исследование объявлений о продаже квартир

В вашем распоряжении данные сервиса Яндекс.Недвижимость — архив объявлений о продаже квартир в Санкт-Петербурге и соседних населённых пунктов за несколько лет. Нужно научиться определять рыночную стоимость объектов недвижимости. Ваша задача — установить параметры. Это позволит построить автоматизированную систему: она отследит аномалии и мошенническую деятельность. 

По каждой квартире на продажу доступны два вида данных. Первые вписаны пользователем, вторые — получены автоматически на основе картографических данных. Например, расстояние до центра, аэропорта, ближайшего парка и водоёма. 

<a id='intro'></a>

<h1>Содержание<span class="tocSkip"></span></h1>
<div class="toc"><ul class="toc-item"><li><span><a href="#Откройте-файл-с-данными-и-изучите-общую-информацию." data-toc-modified-id="Откройте-файл-с-данными-и-изучите-общую-информацию.-1"><span class="toc-item-num">1&nbsp;&nbsp;</span>Откройте файл с данными и изучите общую информацию.</a></span></li><li><span><a href="#Предобработка-данных" data-toc-modified-id="Предобработка-данных-2"><span class="toc-item-num">2&nbsp;&nbsp;</span>Предобработка данных</a></span><ul class="toc-item"><li><span><a href="#Переименование-столбца" data-toc-modified-id="Переименование-столбца-2.1"><span class="toc-item-num">2.1&nbsp;&nbsp;</span>Переименование столбца</a></span></li><li><span><a href="#Столбец-total_images" data-toc-modified-id="Столбец-total_images-2.2"><span class="toc-item-num">2.2&nbsp;&nbsp;</span>Столбец <code>total_images</code></a></span></li><li><span><a href="#Столбец-last_price" data-toc-modified-id="Столбец-last_price-2.3"><span class="toc-item-num">2.3&nbsp;&nbsp;</span>Столбец <code>last_price</code></a></span></li><li><span><a href="#Столбец-total_area" data-toc-modified-id="Столбец-total_area-2.4"><span class="toc-item-num">2.4&nbsp;&nbsp;</span>Столбец <code>total_area</code></a></span></li><li><span><a href="#Столбец-first_day_exposition" data-toc-modified-id="Столбец-first_day_exposition-2.5"><span class="toc-item-num">2.5&nbsp;&nbsp;</span>Столбец <code>first_day_exposition</code></a></span></li><li><span><a href="#Столбец-rooms" data-toc-modified-id="Столбец-rooms-2.6"><span class="toc-item-num">2.6&nbsp;&nbsp;</span>Столбец <code>rooms</code></a></span></li><li><span><a href="#Столбец-ceiling_height" data-toc-modified-id="Столбец-ceiling_height-2.7"><span class="toc-item-num">2.7&nbsp;&nbsp;</span>Столбец <code>ceiling_height</code></a></span></li><li><span><a href="#Столбец-floors_total" data-toc-modified-id="Столбец-floors_total-2.8"><span class="toc-item-num">2.8&nbsp;&nbsp;</span>Столбец <code>floors_total</code></a></span></li><li><span><a href="#Столбец-living_area" data-toc-modified-id="Столбец-living_area-2.9"><span class="toc-item-num">2.9&nbsp;&nbsp;</span>Столбец <code>living_area</code></a></span></li><li><span><a href="#Столбец-floor" data-toc-modified-id="Столбец-floor-2.10"><span class="toc-item-num">2.10&nbsp;&nbsp;</span>Столбец <code>floor</code></a></span></li><li><span><a href="#Столбец-is_apartment" data-toc-modified-id="Столбец-is_apartment-2.11"><span class="toc-item-num">2.11&nbsp;&nbsp;</span>Столбец <code>is_apartment</code></a></span></li><li><span><a href="#Столбец--studio" data-toc-modified-id="Столбец--studio-2.12"><span class="toc-item-num">2.12&nbsp;&nbsp;</span>Столбец  <code>studio</code></a></span></li><li><span><a href="#Столбец-open_plan" data-toc-modified-id="Столбец-open_plan-2.13"><span class="toc-item-num">2.13&nbsp;&nbsp;</span>Столбец <code>open_plan</code></a></span></li><li><span><a href="#Столбец-kitchen_area" data-toc-modified-id="Столбец-kitchen_area-2.14"><span class="toc-item-num">2.14&nbsp;&nbsp;</span>Столбец <code>kitchen_area</code></a></span></li><li><span><a href="#Столбец-balcony" data-toc-modified-id="Столбец-balcony-2.15"><span class="toc-item-num">2.15&nbsp;&nbsp;</span>Столбец <code>balcony</code></a></span></li><li><span><a href="#Столбец-locality_name" data-toc-modified-id="Столбец-locality_name-2.16"><span class="toc-item-num">2.16&nbsp;&nbsp;</span>Столбец <code>locality_name</code></a></span></li><li><span><a href="#Столбец-airports_nearest" data-toc-modified-id="Столбец-airports_nearest-2.17"><span class="toc-item-num">2.17&nbsp;&nbsp;</span>Столбец <code>airports_nearest</code></a></span></li><li><span><a href="#Столбец-city_centers_nearest" data-toc-modified-id="Столбец-city_centers_nearest-2.18"><span class="toc-item-num">2.18&nbsp;&nbsp;</span>Столбец <code>city_centers_nearest</code></a></span></li><li><span><a href="#Столбец-parks_around3000" data-toc-modified-id="Столбец-parks_around3000-2.19"><span class="toc-item-num">2.19&nbsp;&nbsp;</span>Столбец <code>parks_around3000</code></a></span></li><li><span><a href="#Столбец-parks_nearest" data-toc-modified-id="Столбец-parks_nearest-2.20"><span class="toc-item-num">2.20&nbsp;&nbsp;</span>Столбец <code>parks_nearest</code></a></span></li><li><span><a href="#Столбец-ponds_around3000" data-toc-modified-id="Столбец-ponds_around3000-2.21"><span class="toc-item-num">2.21&nbsp;&nbsp;</span>Столбец <code>ponds_around3000</code></a></span></li><li><span><a href="#Столбец-ponds_nearest" data-toc-modified-id="Столбец-ponds_nearest-2.22"><span class="toc-item-num">2.22&nbsp;&nbsp;</span>Столбец <code>ponds_nearest</code></a></span></li><li><span><a href="#Столбец-days_exposition" data-toc-modified-id="Столбец-days_exposition-2.23"><span class="toc-item-num">2.23&nbsp;&nbsp;</span>Столбец <code>days_exposition</code></a></span></li><li><span><a href="#Поиск-и-удаление-дубликатов" data-toc-modified-id="Поиск-и-удаление-дубликатов-2.24"><span class="toc-item-num">2.24&nbsp;&nbsp;</span>Поиск и удаление дубликатов</a></span></li><li><span><a href="#Изменение-типов-данных" data-toc-modified-id="Изменение-типов-данных-2.25"><span class="toc-item-num">2.25&nbsp;&nbsp;</span>Изменение типов данных</a></span></li><li><span><a href="#Вывод" data-toc-modified-id="Вывод-2.26"><span class="toc-item-num">2.26&nbsp;&nbsp;</span>Вывод</a></span></li></ul></li><li><span><a href="#Посчитайте-и-добавьте-в-таблицу-новые-столбцы" data-toc-modified-id="Посчитайте-и-добавьте-в-таблицу-новые-столбцы-3"><span class="toc-item-num">3&nbsp;&nbsp;</span>Посчитайте и добавьте в таблицу новые столбцы</a></span><ul class="toc-item"><li><span><a href="#Цена-одного-квадратного-метра" data-toc-modified-id="Цена-одного-квадратного-метра-3.1"><span class="toc-item-num">3.1&nbsp;&nbsp;</span>Цена одного квадратного метра</a></span></li><li><span><a href="#День-недели-публикации-объявления-|-Месяц-публикации-объявления-|-Год-публикации-объявления" data-toc-modified-id="День-недели-публикации-объявления-|-Месяц-публикации-объявления-|-Год-публикации-объявления-3.2"><span class="toc-item-num">3.2&nbsp;&nbsp;</span>День недели публикации объявления | Месяц публикации объявления | Год публикации объявления</a></span></li><li><span><a href="#Тип-этажа-квартиры" data-toc-modified-id="Тип-этажа-квартиры-3.3"><span class="toc-item-num">3.3&nbsp;&nbsp;</span>Тип этажа квартиры</a></span></li><li><span><a href="#Расстояние-до-центра-города-в-километрах" data-toc-modified-id="Расстояние-до-центра-города-в-километрах-3.4"><span class="toc-item-num">3.4&nbsp;&nbsp;</span>Расстояние до центра города в километрах</a></span></li></ul></li><li><span><a href="#Проведите-исследовательский-анализ-данных" data-toc-modified-id="Проведите-исследовательский-анализ-данных-4"><span class="toc-item-num">4&nbsp;&nbsp;</span>Проведите исследовательский анализ данных</a></span><ul class="toc-item"><li><span><a href="#Изучаем-параметры-объектов" data-toc-modified-id="Изучаем-параметры-объектов-4.1"><span class="toc-item-num">4.1&nbsp;&nbsp;</span>Изучаем параметры объектов</a></span></li><li><span><a href="#Изучаем,-как-быстро-продавались-квартиры" data-toc-modified-id="Изучаем,-как-быстро-продавались-квартиры-4.2"><span class="toc-item-num">4.2&nbsp;&nbsp;</span>Изучаем, как быстро продавались квартиры</a></span></li><li><span><a href="#Факторы-больше-всего-влияющие-на-общую-(полную)-стоимость-объекта" data-toc-modified-id="Факторы-больше-всего-влияющие-на-общую-(полную)-стоимость-объекта-4.3"><span class="toc-item-num">4.3&nbsp;&nbsp;</span>Факторы больше всего влияющие на общую (полную) стоимость объекта</a></span></li><li><span><a href="#Средняя-цена-одного-квадратного-метра." data-toc-modified-id="Средняя-цена-одного-квадратного-метра.-4.4"><span class="toc-item-num">4.4&nbsp;&nbsp;</span>Средняя цена одного квадратного метра.</a></span></li><li><span><a href="#Средняя-цена-километра-в-Санкт-Петербурге" data-toc-modified-id="Средняя-цена-километра-в-Санкт-Петербурге-4.5"><span class="toc-item-num">4.5&nbsp;&nbsp;</span>Средняя цена километра в Санкт-Петербурге</a></span></li></ul></li><li><span><a href="#Общий-вывод" data-toc-modified-id="Общий-вывод-5"><span class="toc-item-num">5&nbsp;&nbsp;</span>Общий вывод</a></span></li></ul></div>

**Описание данных**

`airports_nearest` — расстояние до ближайшего аэропорта в метрах (м)  
`balcony` — число балконов  
`ceiling_height` — высота потолков (м)  
`cityCenters_nearest` — расстояние до центра города (м)  
`days_exposition` — сколько дней было размещено объявление (от публикации до снятия)  
`first_day_exposition` — дата публикации  
`floor` — этаж  
`floors_total` — всего этажей в доме  
`is_apartment` — апартаменты (булев тип)  
`kitchen_area` — площадь кухни в квадратных метрах (м²)  
`last_price` — цена на момент снятия с публикации  
`living_area` — жилая площадь в квадратных метрах (м²)  
`locality_name` — название населённого пункта  
`open_plan` — свободная планировка (булев тип)  
`parks_around3000` — число парков в радиусе 3 км  
`parks_nearest` — расстояние до ближайшего парка (м)  
`ponds_around3000` — число водоёмов в радиусе 3 км  
`ponds_nearest` — расстояние до ближайшего водоёма (м)  
`rooms` — число комнат  
`studio` — квартира-студия (булев тип)  
`total_area` — общая площадь квартиры в квадратных метрах (м²)  
`total_images` — число фотографий квартиры в объявлении

### Откройте файл с данными и изучите общую информацию. 

[Содержание](#intro)


```python
import pandas as pd
import matplotlib.pyplot as plt
import seaborn as sns

try:
    df = pd.read_csv('/datasets/real_estate_data.csv', sep='\t')
except FileNotFoundError:
    print('Некорректно задан путь к файлу')
    
df.head()
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
      <th>total_images</th>
      <th>last_price</th>
      <th>total_area</th>
      <th>first_day_exposition</th>
      <th>rooms</th>
      <th>ceiling_height</th>
      <th>floors_total</th>
      <th>living_area</th>
      <th>floor</th>
      <th>is_apartment</th>
      <th>...</th>
      <th>kitchen_area</th>
      <th>balcony</th>
      <th>locality_name</th>
      <th>airports_nearest</th>
      <th>cityCenters_nearest</th>
      <th>parks_around3000</th>
      <th>parks_nearest</th>
      <th>ponds_around3000</th>
      <th>ponds_nearest</th>
      <th>days_exposition</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>20</td>
      <td>13000000.0</td>
      <td>108.0</td>
      <td>2019-03-07T00:00:00</td>
      <td>3</td>
      <td>2.70</td>
      <td>16.0</td>
      <td>51.0</td>
      <td>8</td>
      <td>NaN</td>
      <td>...</td>
      <td>25.0</td>
      <td>NaN</td>
      <td>Санкт-Петербург</td>
      <td>18863.0</td>
      <td>16028.0</td>
      <td>1.0</td>
      <td>482.0</td>
      <td>2.0</td>
      <td>755.0</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>1</th>
      <td>7</td>
      <td>3350000.0</td>
      <td>40.4</td>
      <td>2018-12-04T00:00:00</td>
      <td>1</td>
      <td>NaN</td>
      <td>11.0</td>
      <td>18.6</td>
      <td>1</td>
      <td>NaN</td>
      <td>...</td>
      <td>11.0</td>
      <td>2.0</td>
      <td>посёлок Шушары</td>
      <td>12817.0</td>
      <td>18603.0</td>
      <td>0.0</td>
      <td>NaN</td>
      <td>0.0</td>
      <td>NaN</td>
      <td>81.0</td>
    </tr>
    <tr>
      <th>2</th>
      <td>10</td>
      <td>5196000.0</td>
      <td>56.0</td>
      <td>2015-08-20T00:00:00</td>
      <td>2</td>
      <td>NaN</td>
      <td>5.0</td>
      <td>34.3</td>
      <td>4</td>
      <td>NaN</td>
      <td>...</td>
      <td>8.3</td>
      <td>0.0</td>
      <td>Санкт-Петербург</td>
      <td>21741.0</td>
      <td>13933.0</td>
      <td>1.0</td>
      <td>90.0</td>
      <td>2.0</td>
      <td>574.0</td>
      <td>558.0</td>
    </tr>
    <tr>
      <th>3</th>
      <td>0</td>
      <td>64900000.0</td>
      <td>159.0</td>
      <td>2015-07-24T00:00:00</td>
      <td>3</td>
      <td>NaN</td>
      <td>14.0</td>
      <td>NaN</td>
      <td>9</td>
      <td>NaN</td>
      <td>...</td>
      <td>NaN</td>
      <td>0.0</td>
      <td>Санкт-Петербург</td>
      <td>28098.0</td>
      <td>6800.0</td>
      <td>2.0</td>
      <td>84.0</td>
      <td>3.0</td>
      <td>234.0</td>
      <td>424.0</td>
    </tr>
    <tr>
      <th>4</th>
      <td>2</td>
      <td>10000000.0</td>
      <td>100.0</td>
      <td>2018-06-19T00:00:00</td>
      <td>2</td>
      <td>3.03</td>
      <td>14.0</td>
      <td>32.0</td>
      <td>13</td>
      <td>NaN</td>
      <td>...</td>
      <td>41.0</td>
      <td>NaN</td>
      <td>Санкт-Петербург</td>
      <td>31856.0</td>
      <td>8098.0</td>
      <td>2.0</td>
      <td>112.0</td>
      <td>1.0</td>
      <td>48.0</td>
      <td>121.0</td>
    </tr>
  </tbody>
</table>
<p>5 rows × 22 columns</p>
</div>




```python
df.info()
```

    <class 'pandas.core.frame.DataFrame'>
    RangeIndex: 23699 entries, 0 to 23698
    Data columns (total 22 columns):
     #   Column                Non-Null Count  Dtype  
    ---  ------                --------------  -----  
     0   total_images          23699 non-null  int64  
     1   last_price            23699 non-null  float64
     2   total_area            23699 non-null  float64
     3   first_day_exposition  23699 non-null  object 
     4   rooms                 23699 non-null  int64  
     5   ceiling_height        14504 non-null  float64
     6   floors_total          23613 non-null  float64
     7   living_area           21796 non-null  float64
     8   floor                 23699 non-null  int64  
     9   is_apartment          2775 non-null   object 
     10  studio                23699 non-null  bool   
     11  open_plan             23699 non-null  bool   
     12  kitchen_area          21421 non-null  float64
     13  balcony               12180 non-null  float64
     14  locality_name         23650 non-null  object 
     15  airports_nearest      18157 non-null  float64
     16  cityCenters_nearest   18180 non-null  float64
     17  parks_around3000      18181 non-null  float64
     18  parks_nearest         8079 non-null   float64
     19  ponds_around3000      18181 non-null  float64
     20  ponds_nearest         9110 non-null   float64
     21  days_exposition       20518 non-null  float64
    dtypes: bool(2), float64(14), int64(3), object(3)
    memory usage: 3.7+ MB



```python
df.describe()
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
      <th>total_images</th>
      <th>last_price</th>
      <th>total_area</th>
      <th>rooms</th>
      <th>ceiling_height</th>
      <th>floors_total</th>
      <th>living_area</th>
      <th>floor</th>
      <th>kitchen_area</th>
      <th>balcony</th>
      <th>airports_nearest</th>
      <th>cityCenters_nearest</th>
      <th>parks_around3000</th>
      <th>parks_nearest</th>
      <th>ponds_around3000</th>
      <th>ponds_nearest</th>
      <th>days_exposition</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>count</th>
      <td>23699.000000</td>
      <td>2.369900e+04</td>
      <td>23699.000000</td>
      <td>23699.000000</td>
      <td>14504.000000</td>
      <td>23613.000000</td>
      <td>21796.000000</td>
      <td>23699.000000</td>
      <td>21421.000000</td>
      <td>12180.000000</td>
      <td>18157.000000</td>
      <td>18180.000000</td>
      <td>18181.000000</td>
      <td>8079.000000</td>
      <td>18181.000000</td>
      <td>9110.000000</td>
      <td>20518.000000</td>
    </tr>
    <tr>
      <th>mean</th>
      <td>9.858475</td>
      <td>6.541549e+06</td>
      <td>60.348651</td>
      <td>2.070636</td>
      <td>2.771499</td>
      <td>10.673824</td>
      <td>34.457852</td>
      <td>5.892358</td>
      <td>10.569807</td>
      <td>1.150082</td>
      <td>28793.672193</td>
      <td>14191.277833</td>
      <td>0.611408</td>
      <td>490.804555</td>
      <td>0.770255</td>
      <td>517.980900</td>
      <td>180.888634</td>
    </tr>
    <tr>
      <th>std</th>
      <td>5.682529</td>
      <td>1.088701e+07</td>
      <td>35.654083</td>
      <td>1.078405</td>
      <td>1.261056</td>
      <td>6.597173</td>
      <td>22.030445</td>
      <td>4.885249</td>
      <td>5.905438</td>
      <td>1.071300</td>
      <td>12630.880622</td>
      <td>8608.386210</td>
      <td>0.802074</td>
      <td>342.317995</td>
      <td>0.938346</td>
      <td>277.720643</td>
      <td>219.727988</td>
    </tr>
    <tr>
      <th>min</th>
      <td>0.000000</td>
      <td>1.219000e+04</td>
      <td>12.000000</td>
      <td>0.000000</td>
      <td>1.000000</td>
      <td>1.000000</td>
      <td>2.000000</td>
      <td>1.000000</td>
      <td>1.300000</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>181.000000</td>
      <td>0.000000</td>
      <td>1.000000</td>
      <td>0.000000</td>
      <td>13.000000</td>
      <td>1.000000</td>
    </tr>
    <tr>
      <th>25%</th>
      <td>6.000000</td>
      <td>3.400000e+06</td>
      <td>40.000000</td>
      <td>1.000000</td>
      <td>2.520000</td>
      <td>5.000000</td>
      <td>18.600000</td>
      <td>2.000000</td>
      <td>7.000000</td>
      <td>0.000000</td>
      <td>18585.000000</td>
      <td>9238.000000</td>
      <td>0.000000</td>
      <td>288.000000</td>
      <td>0.000000</td>
      <td>294.000000</td>
      <td>45.000000</td>
    </tr>
    <tr>
      <th>50%</th>
      <td>9.000000</td>
      <td>4.650000e+06</td>
      <td>52.000000</td>
      <td>2.000000</td>
      <td>2.650000</td>
      <td>9.000000</td>
      <td>30.000000</td>
      <td>4.000000</td>
      <td>9.100000</td>
      <td>1.000000</td>
      <td>26726.000000</td>
      <td>13098.500000</td>
      <td>0.000000</td>
      <td>455.000000</td>
      <td>1.000000</td>
      <td>502.000000</td>
      <td>95.000000</td>
    </tr>
    <tr>
      <th>75%</th>
      <td>14.000000</td>
      <td>6.800000e+06</td>
      <td>69.900000</td>
      <td>3.000000</td>
      <td>2.800000</td>
      <td>16.000000</td>
      <td>42.300000</td>
      <td>8.000000</td>
      <td>12.000000</td>
      <td>2.000000</td>
      <td>37273.000000</td>
      <td>16293.000000</td>
      <td>1.000000</td>
      <td>612.000000</td>
      <td>1.000000</td>
      <td>729.000000</td>
      <td>232.000000</td>
    </tr>
    <tr>
      <th>max</th>
      <td>50.000000</td>
      <td>7.630000e+08</td>
      <td>900.000000</td>
      <td>19.000000</td>
      <td>100.000000</td>
      <td>60.000000</td>
      <td>409.700000</td>
      <td>33.000000</td>
      <td>112.000000</td>
      <td>5.000000</td>
      <td>84869.000000</td>
      <td>65968.000000</td>
      <td>3.000000</td>
      <td>3190.000000</td>
      <td>3.000000</td>
      <td>1344.000000</td>
      <td>1580.000000</td>
    </tr>
  </tbody>
</table>
</div>




```python
df.hist(figsize=(15, 20))
```




    array([[<AxesSubplot:title={'center':'total_images'}>,
            <AxesSubplot:title={'center':'last_price'}>,
            <AxesSubplot:title={'center':'total_area'}>,
            <AxesSubplot:title={'center':'rooms'}>],
           [<AxesSubplot:title={'center':'ceiling_height'}>,
            <AxesSubplot:title={'center':'floors_total'}>,
            <AxesSubplot:title={'center':'living_area'}>,
            <AxesSubplot:title={'center':'floor'}>],
           [<AxesSubplot:title={'center':'kitchen_area'}>,
            <AxesSubplot:title={'center':'balcony'}>,
            <AxesSubplot:title={'center':'airports_nearest'}>,
            <AxesSubplot:title={'center':'cityCenters_nearest'}>],
           [<AxesSubplot:title={'center':'parks_around3000'}>,
            <AxesSubplot:title={'center':'parks_nearest'}>,
            <AxesSubplot:title={'center':'ponds_around3000'}>,
            <AxesSubplot:title={'center':'ponds_nearest'}>],
           [<AxesSubplot:title={'center':'days_exposition'}>, <AxesSubplot:>,
            <AxesSubplot:>, <AxesSubplot:>]], dtype=object)




    
![png](output_8_1.png)
    


На первый взгляд данные выглядят нормально.
Но уже можно заметить некорректные данные:
- 19 комнат в квартире
- Площадь 900 метров
- Высота потолка 100 метров
- Минимальная стоимость 12000

### Предобработка данных

#### Переименование столбца

[Содержание](#intro)

Приведем названия столбцов к змеиному регистру. Посчитаем количество пропусков в каждом столбце.


```python
df = df.rename(columns={'cityCenters_nearest':'city_centers_nearest'})
df.isna().sum()
```




    total_images                0
    last_price                  0
    total_area                  0
    first_day_exposition        0
    rooms                       0
    ceiling_height           9195
    floors_total               86
    living_area              1903
    floor                       0
    is_apartment            20924
    studio                      0
    open_plan                   0
    kitchen_area             2278
    balcony                 11519
    locality_name              49
    airports_nearest         5542
    city_centers_nearest     5519
    parks_around3000         5518
    parks_nearest           15620
    ponds_around3000         5518
    ponds_nearest           14589
    days_exposition          3181
    dtype: int64



**Пропусков в данных огромное количество. Просто удалить не получится, будут искажения в исследовании.**

Теперь проверим данные в каждом стобце. Построим графики, решим как и чем заполнять данные. 

Так как в дальнейшем ожидают однотипные запросы информации и графиков, создадим нужную функцию:


```python
def des_column(col):
    df.hist(col, bins=200, figsize=(15, 5))
    plt.show()
    display(df[col].describe())
    df.boxplot(column=col, figsize=(5, 10))
    plt.show()
```

#### Столбец `total_images`

[Содержание](#intro)

Пропусков нет, посмотрим на данные:


```python
display(df['total_images'].value_counts())
df['total_images'].hist(bins=50)
```


    10    1798
    9     1725
    20    1694
    8     1585
    7     1521
    6     1482
    11    1362
    5     1301
    12    1225
    0     1059
    13    1015
    4      986
    14     986
    15     948
    1      872
    3      769
    16     761
    17     650
    18     642
    2      640
    19     603
    23      16
    21      12
    24       8
    22       8
    26       5
    28       4
    32       4
    29       3
    50       3
    27       2
    35       2
    30       2
    31       2
    39       1
    25       1
    42       1
    37       1
    Name: total_images, dtype: int64





    <AxesSubplot:>




    
![png](output_18_2.png)
    


**Данные выглядят корректно, двигаемся дальше.**

#### Столбец `last_price`
[Содержание](#intro)

Пропусков нет. Посмотрим описание данных и построим графики:


```python
des_column('last_price')
```


    
![png](output_21_0.png)
    



    count    2.369900e+04
    mean     6.541549e+06
    std      1.088701e+07
    min      1.219000e+04
    25%      3.400000e+06
    50%      4.650000e+06
    75%      6.800000e+06
    max      7.630000e+08
    Name: last_price, dtype: float64



    
![png](output_21_2.png)
    


Вызывают сомнения минимальное (12190) и максимальное значение (763млн).

Посмотрим значения на более детальных графиках:


```python
df.hist('last_price', bins=100, range=(0, 500000), figsize=(15, 5))
df.hist('last_price', bins=100, range=(500001, 100000000), figsize=(15, 5))
df.hist('last_price', bins=100, range=(100000001, 800000000), figsize=(15, 5))
```




    array([[<AxesSubplot:title={'center':'last_price'}>]], dtype=object)




    
![png](output_23_1.png)
    



    
![png](output_23_2.png)
    



    
![png](output_23_3.png)
    


Имеются выбросы. Оставим все значения в диапазоне от 400т. до 500млн.


```python
df = df.query('400000 <= last_price <= 500000000')
df['last_price'].describe()
```




    count    2.369700e+04
    mean     6.509902e+06
    std      9.715219e+06
    min      4.300000e+05
    25%      3.400000e+06
    50%      4.650000e+06
    75%      6.800000e+06
    max      4.200000e+08
    Name: last_price, dtype: float64



**Столбец готов, лишние строки удалены.**


```python
print('После удаления осталось строк:', df['last_price'].shape[0])
print('Удалено данных %:', (23699 - (df['last_price'].shape[0])) / 23699 * 100)
```

    После удаления осталось строк: 23697
    Удалено данных %: 0.008439174648719355


#### Столбец `total_area`

[Содержание](#intro)

Пропусков нет, посмотрим данные и изучим графики:


```python
des_column('total_area')
```


    
![png](output_30_0.png)
    



    count    23697.000000
    mean        60.332265
    std         35.585844
    min         12.000000
    25%         40.000000
    50%         52.000000
    75%         69.800000
    max        900.000000
    Name: total_area, dtype: float64



    
![png](output_30_2.png)
    


Имеются подозрительные значения в последней четверти значений "на ящике с усами". Посмотрим как распределяются значения выше 400 кв.м.


```python
df.hist('total_area', bins=200, range=(400, 1000), figsize=(15, 5))
```




    array([[<AxesSubplot:title={'center':'total_area'}>]], dtype=object)




    
![png](output_32_1.png)
    


Удалим выброс


```python
df = df.query('total_area < 700')
df['total_area'].describe()
```




    count    23696.000000
    mean        60.296830
    std         35.166029
    min         12.000000
    25%         40.000000
    50%         52.000000
    75%         69.800000
    max        631.200000
    Name: total_area, dtype: float64



**Столбец готов, можно идти дальше.**

#### Столбец `first_day_exposition`

[Содержание](#intro)

Посмотрим данные:


```python
display(df['first_day_exposition'].head())
```


    0    2019-03-07T00:00:00
    1    2018-12-04T00:00:00
    2    2015-08-20T00:00:00
    3    2015-07-24T00:00:00
    4    2018-06-19T00:00:00
    Name: first_day_exposition, dtype: object


Данные записаны в текстовом формате, изменим тип.


```python
df['first_day_exposition'] = pd.to_datetime(df['first_day_exposition'], format='%Y-%m-%dT%H:%M:%S')
display(df['first_day_exposition'].head())
```


    0   2019-03-07
    1   2018-12-04
    2   2015-08-20
    3   2015-07-24
    4   2018-06-19
    Name: first_day_exposition, dtype: datetime64[ns]


**Столбец исправили, двигаемся дальше.**

#### Столбец `rooms`

[Содержание](#intro)

Пропусков значений нет. Посмотрим на данные и решим, что делать дальше:


```python
des_column('rooms')
df['rooms'].value_counts()
```


    
![png](output_44_0.png)
    



    count    23696.000000
    mean         2.070012
    std          1.076066
    min          0.000000
    25%          1.000000
    50%          2.000000
    75%          3.000000
    max         19.000000
    Name: rooms, dtype: float64



    
![png](output_44_2.png)
    





    1     8047
    2     7939
    3     5814
    4     1180
    5      326
    0      197
    6      105
    7       58
    8       12
    9        8
    10       3
    11       2
    14       2
    16       1
    19       1
    15       1
    Name: rooms, dtype: int64



Исходя из данных мы имеем квартиры с количеством комнат 0.

Значение 0 в столбце может быть из-за квартир-студий, где комната и кухня объединены. 

Отфильтруем квартиры-студии и заменим значения 0 на 1


```python
df.loc[(df['rooms'] == 0) & (df['studio'] == True), 'rooms'] = 1
df['rooms'].value_counts()
```




    1     8185
    2     7939
    3     5814
    4     1180
    5      326
    6      105
    0       59
    7       58
    8       12
    9        8
    10       3
    11       2
    14       2
    16       1
    19       1
    15       1
    Name: rooms, dtype: int64



После замены значений в столбце осталось еще 59 квартир.

Посмотрим на площать этих квартир.


```python
df[df['rooms'] == 0]['total_area'].value_counts()
```




    25.00     7
    24.00     5
    26.00     3
    35.00     2
    27.00     2
    28.00     2
    26.10     2
    27.30     2
    29.00     2
    25.41     1
    27.10     1
    28.30     1
    25.27     1
    26.80     1
    20.00     1
    27.32     1
    28.05     1
    28.01     1
    22.00     1
    16.00     1
    32.30     1
    25.20     1
    30.50     1
    23.98     1
    27.50     1
    24.20     1
    34.40     1
    21.00     1
    31.00     1
    28.50     1
    23.00     1
    23.06     1
    22.50     1
    27.70     1
    30.00     1
    25.90     1
    31.10     1
    34.00     1
    42.63     1
    28.20     1
    371.00    1
    Name: total_area, dtype: int64



Из полученных данных мы видим, что большая часть квартир по площади меньше 43 кв.м.
Проверим среднюю площадь квартир с 1 и 2 комнатами по всему датафрейму.


```python
for j in range(1, 3):
    print(f'{j} комнатная квартира:',df.query('rooms == @j')['total_area'].mean())
```

    1 комнатная квартира: 37.47892852779475
    2 комнатная квартира: 55.84839526388714


После проверки, можем сделать вывод, что эти квартиры 1 комнатные. Поставим соответсвующее значение в столбец.

По площади в 371 кв.м. сложно определить количество комнат, поэтому просто удалим стороку.


```python
df.loc[(df['rooms'] == 0) & (df['total_area'] < 43), 'rooms'] = 1
df = df.query('rooms != 0')
df['rooms'].value_counts()
```




    1     8243
    2     7939
    3     5814
    4     1180
    5      326
    6      105
    7       58
    8       12
    9        8
    10       3
    11       2
    14       2
    16       1
    19       1
    15       1
    Name: rooms, dtype: int64



**Столбец готов.**

#### Столбец `ceiling_height`

[Содержание](#intro)

В столбце много пропусков. Изучим данные и решим, что делать.


```python
des_column('ceiling_height')
```


    
![png](output_56_0.png)
    



    count    14501.000000
    mean         2.771443
    std          1.261169
    min          1.000000
    25%          2.520000
    50%          2.650000
    75%          2.800000
    max        100.000000
    Name: ceiling_height, dtype: float64



    
![png](output_56_2.png)
    


Из [Яндекс поиска](https://yandex.ru/search/?text=типовая+высота+потолка&lr=6&clid=1955453&win=515):

В массовой типовой многоквартирной застройке высота потолков зависит от климатической зоны и не может быть ниже 2,5 м или 2,7 м. Обычно называют высокими потолки от 3 м. В частном домостроении таких ограничений нет, архитектурные решения позволяют строить дома с высотой потолков до 5–6 м.

Возьмем еще до -20 см на натяжные или подвесные потолки

Посмотрим подробнее как распределяются данные.


```python
df['ceiling_height'].hist(figsize=(15,5), bins=60, range=(0, 2.3))
plt.show()
df['ceiling_height'].hist(figsize=(15,5), bins=60, range=(2.3, 5))
plt.show()
df['ceiling_height'].hist(figsize=(15,5), bins=60, range=(5, 100))
plt.show()
```


    
![png](output_58_0.png)
    



    
![png](output_58_1.png)
    



    
![png](output_58_2.png)
    


По графикам видно, что основные значения находятся в районе 2,5 - 3 м. На последнем графике виден всплеск значений от 20 до 40 м. Это может быть связано с тем, что в значении высоты пропущена запятая(точка).


```python
df.query('19 < ceiling_height < 40')
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
      <th>total_images</th>
      <th>last_price</th>
      <th>total_area</th>
      <th>first_day_exposition</th>
      <th>rooms</th>
      <th>ceiling_height</th>
      <th>floors_total</th>
      <th>living_area</th>
      <th>floor</th>
      <th>is_apartment</th>
      <th>...</th>
      <th>kitchen_area</th>
      <th>balcony</th>
      <th>locality_name</th>
      <th>airports_nearest</th>
      <th>city_centers_nearest</th>
      <th>parks_around3000</th>
      <th>parks_nearest</th>
      <th>ponds_around3000</th>
      <th>ponds_nearest</th>
      <th>days_exposition</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>355</th>
      <td>17</td>
      <td>3600000.0</td>
      <td>55.2</td>
      <td>2018-07-12</td>
      <td>2</td>
      <td>25.0</td>
      <td>5.0</td>
      <td>32.0</td>
      <td>2</td>
      <td>False</td>
      <td>...</td>
      <td>NaN</td>
      <td>2.0</td>
      <td>Гатчина</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>259.0</td>
    </tr>
    <tr>
      <th>3148</th>
      <td>14</td>
      <td>2900000.0</td>
      <td>75.0</td>
      <td>2018-11-12</td>
      <td>3</td>
      <td>32.0</td>
      <td>3.0</td>
      <td>53.0</td>
      <td>2</td>
      <td>NaN</td>
      <td>...</td>
      <td>8.0</td>
      <td>NaN</td>
      <td>Волхов</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>4643</th>
      <td>0</td>
      <td>4300000.0</td>
      <td>45.0</td>
      <td>2018-02-01</td>
      <td>2</td>
      <td>25.0</td>
      <td>9.0</td>
      <td>30.0</td>
      <td>2</td>
      <td>NaN</td>
      <td>...</td>
      <td>7.0</td>
      <td>1.0</td>
      <td>Санкт-Петербург</td>
      <td>12016.0</td>
      <td>13256.0</td>
      <td>1.0</td>
      <td>658.0</td>
      <td>1.0</td>
      <td>331.0</td>
      <td>181.0</td>
    </tr>
    <tr>
      <th>4876</th>
      <td>7</td>
      <td>3000000.0</td>
      <td>25.0</td>
      <td>2017-09-27</td>
      <td>1</td>
      <td>27.0</td>
      <td>25.0</td>
      <td>17.0</td>
      <td>17</td>
      <td>NaN</td>
      <td>...</td>
      <td>NaN</td>
      <td>2.0</td>
      <td>посёлок Мурино</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>28.0</td>
    </tr>
    <tr>
      <th>5076</th>
      <td>0</td>
      <td>3850000.0</td>
      <td>30.5</td>
      <td>2018-10-03</td>
      <td>1</td>
      <td>24.0</td>
      <td>5.0</td>
      <td>19.5</td>
      <td>1</td>
      <td>True</td>
      <td>...</td>
      <td>5.5</td>
      <td>NaN</td>
      <td>Санкт-Петербург</td>
      <td>29686.0</td>
      <td>8389.0</td>
      <td>3.0</td>
      <td>397.0</td>
      <td>1.0</td>
      <td>578.0</td>
      <td>7.0</td>
    </tr>
    <tr>
      <th>5246</th>
      <td>0</td>
      <td>2500000.0</td>
      <td>54.0</td>
      <td>2017-10-13</td>
      <td>2</td>
      <td>27.0</td>
      <td>5.0</td>
      <td>30.0</td>
      <td>3</td>
      <td>NaN</td>
      <td>...</td>
      <td>9.0</td>
      <td>2.0</td>
      <td>деревня Мины</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>540.0</td>
    </tr>
    <tr>
      <th>5669</th>
      <td>4</td>
      <td>4400000.0</td>
      <td>50.0</td>
      <td>2017-08-08</td>
      <td>2</td>
      <td>26.0</td>
      <td>9.0</td>
      <td>21.3</td>
      <td>3</td>
      <td>NaN</td>
      <td>...</td>
      <td>7.0</td>
      <td>2.0</td>
      <td>Санкт-Петербург</td>
      <td>28981.0</td>
      <td>10912.0</td>
      <td>1.0</td>
      <td>305.0</td>
      <td>0.0</td>
      <td>NaN</td>
      <td>267.0</td>
    </tr>
    <tr>
      <th>5807</th>
      <td>17</td>
      <td>8150000.0</td>
      <td>80.0</td>
      <td>2019-01-09</td>
      <td>2</td>
      <td>27.0</td>
      <td>36.0</td>
      <td>41.0</td>
      <td>13</td>
      <td>NaN</td>
      <td>...</td>
      <td>12.0</td>
      <td>5.0</td>
      <td>Санкт-Петербург</td>
      <td>18732.0</td>
      <td>20444.0</td>
      <td>0.0</td>
      <td>NaN</td>
      <td>3.0</td>
      <td>80.0</td>
      <td>38.0</td>
    </tr>
    <tr>
      <th>6246</th>
      <td>6</td>
      <td>3300000.0</td>
      <td>44.4</td>
      <td>2019-03-25</td>
      <td>2</td>
      <td>25.0</td>
      <td>5.0</td>
      <td>31.3</td>
      <td>5</td>
      <td>NaN</td>
      <td>...</td>
      <td>5.7</td>
      <td>NaN</td>
      <td>Кронштадт</td>
      <td>68923.0</td>
      <td>50649.0</td>
      <td>1.0</td>
      <td>417.0</td>
      <td>2.0</td>
      <td>73.0</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>9379</th>
      <td>5</td>
      <td>3950000.0</td>
      <td>42.0</td>
      <td>2017-03-26</td>
      <td>3</td>
      <td>25.0</td>
      <td>5.0</td>
      <td>30.0</td>
      <td>2</td>
      <td>NaN</td>
      <td>...</td>
      <td>5.2</td>
      <td>NaN</td>
      <td>Санкт-Петербург</td>
      <td>11647.0</td>
      <td>13581.0</td>
      <td>0.0</td>
      <td>NaN</td>
      <td>0.0</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>10773</th>
      <td>8</td>
      <td>3800000.0</td>
      <td>58.0</td>
      <td>2017-10-13</td>
      <td>2</td>
      <td>27.0</td>
      <td>10.0</td>
      <td>30.1</td>
      <td>3</td>
      <td>False</td>
      <td>...</td>
      <td>8.1</td>
      <td>2.0</td>
      <td>посёлок Мурино</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>71.0</td>
    </tr>
    <tr>
      <th>11285</th>
      <td>0</td>
      <td>1950000.0</td>
      <td>37.0</td>
      <td>2019-03-20</td>
      <td>1</td>
      <td>25.0</td>
      <td>5.0</td>
      <td>17.0</td>
      <td>4</td>
      <td>False</td>
      <td>...</td>
      <td>9.0</td>
      <td>2.0</td>
      <td>Луга</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>18.0</td>
    </tr>
    <tr>
      <th>14382</th>
      <td>9</td>
      <td>1700000.0</td>
      <td>35.0</td>
      <td>2015-12-04</td>
      <td>1</td>
      <td>25.0</td>
      <td>5.0</td>
      <td>20.0</td>
      <td>2</td>
      <td>False</td>
      <td>...</td>
      <td>8.0</td>
      <td>1.0</td>
      <td>поселок Новый Свет</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>206.0</td>
    </tr>
    <tr>
      <th>17496</th>
      <td>15</td>
      <td>6700000.0</td>
      <td>92.9</td>
      <td>2019-02-19</td>
      <td>3</td>
      <td>20.0</td>
      <td>17.0</td>
      <td>53.2</td>
      <td>14</td>
      <td>NaN</td>
      <td>...</td>
      <td>12.0</td>
      <td>5.0</td>
      <td>Санкт-Петербург</td>
      <td>16295.0</td>
      <td>15092.0</td>
      <td>1.0</td>
      <td>967.0</td>
      <td>0.0</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>17857</th>
      <td>1</td>
      <td>3900000.0</td>
      <td>56.0</td>
      <td>2017-12-22</td>
      <td>3</td>
      <td>27.0</td>
      <td>5.0</td>
      <td>33.0</td>
      <td>4</td>
      <td>False</td>
      <td>...</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>Санкт-Петербург</td>
      <td>41030.0</td>
      <td>15543.0</td>
      <td>0.0</td>
      <td>NaN</td>
      <td>0.0</td>
      <td>NaN</td>
      <td>73.0</td>
    </tr>
    <tr>
      <th>18545</th>
      <td>6</td>
      <td>3750000.0</td>
      <td>43.0</td>
      <td>2019-03-18</td>
      <td>2</td>
      <td>25.0</td>
      <td>5.0</td>
      <td>29.0</td>
      <td>3</td>
      <td>False</td>
      <td>...</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>Санкт-Петербург</td>
      <td>27054.0</td>
      <td>8033.0</td>
      <td>1.0</td>
      <td>540.0</td>
      <td>0.0</td>
      <td>NaN</td>
      <td>12.0</td>
    </tr>
    <tr>
      <th>20478</th>
      <td>11</td>
      <td>8000000.0</td>
      <td>45.0</td>
      <td>2017-07-18</td>
      <td>1</td>
      <td>27.0</td>
      <td>4.0</td>
      <td>22.0</td>
      <td>2</td>
      <td>NaN</td>
      <td>...</td>
      <td>10.0</td>
      <td>1.0</td>
      <td>Санкт-Петербург</td>
      <td>18975.0</td>
      <td>3246.0</td>
      <td>0.0</td>
      <td>NaN</td>
      <td>3.0</td>
      <td>449.0</td>
      <td>429.0</td>
    </tr>
    <tr>
      <th>20507</th>
      <td>12</td>
      <td>5950000.0</td>
      <td>60.0</td>
      <td>2018-02-19</td>
      <td>2</td>
      <td>22.6</td>
      <td>14.0</td>
      <td>35.0</td>
      <td>11</td>
      <td>NaN</td>
      <td>...</td>
      <td>13.0</td>
      <td>1.0</td>
      <td>Санкт-Петербург</td>
      <td>27028.0</td>
      <td>12570.0</td>
      <td>0.0</td>
      <td>NaN</td>
      <td>0.0</td>
      <td>NaN</td>
      <td>40.0</td>
    </tr>
    <tr>
      <th>21377</th>
      <td>19</td>
      <td>4900000.0</td>
      <td>42.0</td>
      <td>2017-04-18</td>
      <td>1</td>
      <td>27.5</td>
      <td>24.0</td>
      <td>37.7</td>
      <td>19</td>
      <td>False</td>
      <td>...</td>
      <td>11.0</td>
      <td>2.0</td>
      <td>Санкт-Петербург</td>
      <td>42742.0</td>
      <td>9760.0</td>
      <td>0.0</td>
      <td>NaN</td>
      <td>0.0</td>
      <td>NaN</td>
      <td>61.0</td>
    </tr>
    <tr>
      <th>21824</th>
      <td>20</td>
      <td>2450000.0</td>
      <td>44.0</td>
      <td>2019-02-12</td>
      <td>2</td>
      <td>27.0</td>
      <td>2.0</td>
      <td>38.0</td>
      <td>2</td>
      <td>False</td>
      <td>...</td>
      <td>8.6</td>
      <td>2.0</td>
      <td>городской поселок Большая Ижора</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>22336</th>
      <td>19</td>
      <td>9999000.0</td>
      <td>92.4</td>
      <td>2019-04-05</td>
      <td>2</td>
      <td>32.0</td>
      <td>6.0</td>
      <td>55.5</td>
      <td>5</td>
      <td>False</td>
      <td>...</td>
      <td>16.5</td>
      <td>4.0</td>
      <td>Санкт-Петербург</td>
      <td>18838.0</td>
      <td>3506.0</td>
      <td>0.0</td>
      <td>NaN</td>
      <td>3.0</td>
      <td>511.0</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>22938</th>
      <td>14</td>
      <td>4000000.0</td>
      <td>98.0</td>
      <td>2018-03-15</td>
      <td>4</td>
      <td>27.0</td>
      <td>2.0</td>
      <td>73.0</td>
      <td>2</td>
      <td>True</td>
      <td>...</td>
      <td>9.0</td>
      <td>1.0</td>
      <td>деревня Нижняя</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>27.0</td>
    </tr>
  </tbody>
</table>
<p>22 rows × 22 columns</p>
</div>



Исходя из данных о площади - это обычные квартиры, поэтому разделем значения в столбце на 10.


```python
df.loc[(df['ceiling_height'] > 19) & (df['ceiling_height'] < 41), 'ceiling_height'] = df.loc[(df['ceiling_height'] > 19) & (df['ceiling_height'] < 41), 'ceiling_height'] / 10
display(df.query('19 < ceiling_height < 41'))
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
      <th>total_images</th>
      <th>last_price</th>
      <th>total_area</th>
      <th>first_day_exposition</th>
      <th>rooms</th>
      <th>ceiling_height</th>
      <th>floors_total</th>
      <th>living_area</th>
      <th>floor</th>
      <th>is_apartment</th>
      <th>...</th>
      <th>kitchen_area</th>
      <th>balcony</th>
      <th>locality_name</th>
      <th>airports_nearest</th>
      <th>city_centers_nearest</th>
      <th>parks_around3000</th>
      <th>parks_nearest</th>
      <th>ponds_around3000</th>
      <th>ponds_nearest</th>
      <th>days_exposition</th>
    </tr>
  </thead>
  <tbody>
  </tbody>
</table>
<p>0 rows × 22 columns</p>
</div>


Теперь удалим некорректные значения:

- Квартиру с высотой потолка 100, так как даже разделив на 10, значение слишком высокое
- Квартиры с высотой потолка до 2 м , так как это подозрительно мало
- Все значения от 6 (для квартиры это много) до 20 м (от 10 до 20 - это значения без запятой)


```python
drop_height = df.query('ceiling_height == 100 or ceiling_height < 2 or 6 < ceiling_height < 20')['ceiling_height']
#переменная со значениями которые необходимо удалить
df = df.query('ceiling_height not in @drop_height')
display(df.query('ceiling_height == 100 or ceiling_height < 2 or 6 < ceiling_height < 20'))
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
      <th>total_images</th>
      <th>last_price</th>
      <th>total_area</th>
      <th>first_day_exposition</th>
      <th>rooms</th>
      <th>ceiling_height</th>
      <th>floors_total</th>
      <th>living_area</th>
      <th>floor</th>
      <th>is_apartment</th>
      <th>...</th>
      <th>kitchen_area</th>
      <th>balcony</th>
      <th>locality_name</th>
      <th>airports_nearest</th>
      <th>city_centers_nearest</th>
      <th>parks_around3000</th>
      <th>parks_nearest</th>
      <th>ponds_around3000</th>
      <th>ponds_nearest</th>
      <th>days_exposition</th>
    </tr>
  </thead>
  <tbody>
  </tbody>
</table>
<p>0 rows × 22 columns</p>
</div>


Теперь необходимо решить проблему с пропусками. Для замены лучше всего подойдет медианное значение так как оно не подверженно влиянию сильных отклонений.


```python
df['ceiling_height'] = df['ceiling_height'].fillna(df['ceiling_height'].median())
print('Количество пропусков:', df['ceiling_height'].isna().sum())
print()
print(df['ceiling_height'].describe())
```

    Количество пропусков: 0
    
    count    23685.000000
    mean         2.696840
    std          0.221119
    min          2.000000
    25%          2.600000
    50%          2.650000
    75%          2.700000
    max          6.000000
    Name: ceiling_height, dtype: float64


**Данные в столбце отредактированы.**

#### Столбец `floors_total`

[Содержание](#intro)

Имеет небольшое количество пропусков, Взгляним на данные:


```python
des_column('floors_total')
```


    
![png](output_70_0.png)
    



    count    23599.000000
    mean        10.671893
    std          6.594674
    min          1.000000
    25%          5.000000
    50%          9.000000
    75%         16.000000
    max         60.000000
    Name: floors_total, dtype: float64



    
![png](output_70_2.png)
    


Данные выглядят корректно, переходим к пропускам данных.

Так как пропусков небольшое количество (86), заполним пропущенные значения медианой.


```python
df['floors_total'] = df['floors_total'].fillna(df['floors_total'].median())
print('Количество пропусков:', df['floors_total'].isna().sum())
print()
print(df['floors_total'].describe())
```

    Количество пропусков: 0
    
    count    23685.000000
    mean        10.665822
    std          6.583458
    min          1.000000
    25%          5.000000
    50%          9.000000
    75%         16.000000
    max         60.000000
    Name: floors_total, dtype: float64


**Готово.**

#### Столбец `living_area`

[Содержание](#intro)

К жилой площади относятся только комнаты — спальни, детские, гостиные. Так как в датафрейме имеются студии, то проверим и заполним данный отдельно:
- Сначала для квартир-студий
- Потом для всех остальных


```python
print('Студий всего:', len(df[df['studio'] == True]))
print('Студии с указанной площадью:') 
display(df.loc[df['studio'] == True, 'living_area'].describe())
```

    Студий всего: 149
    Студии с указанной площадью:



    count    139.000000
    mean      18.995396
    std        7.345598
    min        2.000000
    25%       16.000000
    50%       18.000000
    75%       19.850000
    max       68.000000
    Name: living_area, dtype: float64


Из полученных данных видим, что есть квартиры с площадью 2 кв.м.
Возможно, это связано с недописанными цифрами в конце значений.
Посмотрим, сколько таких значений:


```python
df[df['studio'] == True]['living_area'].hist(figsize=(15,5), bins=60, range=(2, 16))
plt.show()
```


    
![png](output_78_0.png)
    


Всего две квартиры. Удаление или замена значений на медиану - не повлияет на конечный результат.

Ради интереса сравним жилую зону с общей и посмотрим на аномалии.


```python
studio_plot = df[df['studio'] == True]
studio_plot.plot(y='living_area', x='total_area', style="o", figsize=(15, 5), grid=True, xlim=(0, 70), alpha=0.3);
```


    
![png](output_80_0.png)
    


На графике как раз выделяются два значения с большой общей площадью и аномально маленькой жилой. Правильнее указать медианное значение.


```python
df.loc[(df['living_area'] < 6) & (df['studio'] == True), 'living_area'] = df.loc[(df['studio'] == True), 'living_area'].median()
df[df['studio'] == True]['living_area'].hist(figsize=(15,5), bins=60, range=(2, 16))
plt.show()
```


    
![png](output_82_0.png)
    


Теперь заполним пропуски во всех студиях медианным значением:


```python
df.loc[(df['studio'] == True), 'living_area'] = df.loc[(df['studio'] == True), 'living_area'].fillna(df.loc[(df['studio'] == True), 'living_area'].median())
print('Количество пропущенных значений для студий:', df.loc[(df['studio'] == True), 'living_area'].isna().sum())
studio_plot2 = df[df['studio'] == True]
studio_plot2.plot(y='living_area', x='total_area', style="o", figsize=(15, 5), grid=True, xlim=(0, 70), alpha=0.3)
plt.show()
```

    Количество пропущенных значений для студий: 0



    
![png](output_84_1.png)
    


Теперь заполним пропуски для остальных квартир:


```python
print('Обычных квартир всего:', len(df[df['studio'] == False]))
print('Количество пропущенных значений:', df.loc[(df['studio'] == False), 'living_area'].isna().sum())
print('Квартир с указанной площадью:') 
display(df.loc[df['studio'] == False, 'living_area'].describe())
```

    Обычных квартир всего: 23536
    Количество пропущенных значений: 1892
    Квартир с указанной площадью:



    count    21644.000000
    mean        34.534628
    std         21.866137
    min          2.000000
    25%         18.700000
    50%         30.000000
    75%         42.400000
    max        409.000000
    Name: living_area, dtype: float64


Здесь так же видим квартиры с аномально маленькой жилой площадью. Проделаем теже операции.


```python
df[df['studio'] == False]['living_area'].hist(figsize=(15,5), bins=60, range=(2, 10))
plt.show()
other_plot = df[df['studio'] == False]
other_plot.plot(y='living_area', x='total_area', style="o", figsize=(15, 5), grid=True, xlim=(0, 70), ylim=(0, 30), alpha=0.3)
plt.show()
```


    
![png](output_88_0.png)
    



    
![png](output_88_1.png)
    


При общей площади больше 40 кв.м. - жилая меньше 5.

Исходя из данных графика, жилую площадь меньше 10 кв.м. будем считать аномальной. Исправим  аномальные значения на медиану.


```python
df.loc[(df['living_area'] < 10) & (df['studio'] == False), 'living_area'] = df.loc[(df['studio'] == False), 'living_area'].median()
df[df['studio'] == False]['living_area'].hist(figsize=(15,5), bins=60, range=(2, 60))
plt.show()
df.plot(y='living_area', x='total_area', style="o", figsize=(15, 5), grid=True, xlim=(0, 150), ylim=(0, 150), alpha=0.3)
plt.show()
```


    
![png](output_90_0.png)
    



    
![png](output_90_1.png)
    


Заменим все пропущенные значения на медиану:


```python
#df2 = df #создал второй ДФ для наглядности
#df2.loc[(df2['studio'] == False), 'living_area'] = df2.loc[(df2['studio'] == False), 'living_area'].fillna(df2.loc[(df2['studio'] == False), 'living_area'].median())
#print('Количество пропущенных значений:', df2.loc[(df2['studio'] == False), 'living_area'].isna().sum())
#df2.plot(y='living_area', x='total_area', style="o", figsize=(15, 5), grid=True, xlim=(0, 550), ylim=(0, 400), alpha=0.3)
#plt.show()
```

Данный метод заполнения данных не работает и создает другие аномалии. Попробуем заполнить данные в соотношении с общей площадью. Чем больше общая площадь - тем больше жилая.

Посмотрим соотношение в заполненных данных:


```python
area = df.dropna(subset=['living_area', 'total_area'])
ratio = (area['living_area'] / area['total_area']).mean()
print('Жилая зона от общей занимает:',ratio)
```

    Жилая зона от общей занимает: 0.5655469912497462


Заполним пропущенные значения долей от общей площади:


```python
df.loc[(df['studio'] == False), 'living_area'] = df.loc[(df['studio'] == False), 'living_area'].fillna(df.loc[(df['studio'] == False), 'total_area'] * 0.5678)
df.plot(y='living_area', x='total_area', style="o", figsize=(15, 5), grid=True, xlim=(0, 150), ylim=(0, 150), alpha=0.3)
plt.show()
print('Количество пропущенных значений:', df.loc[(df['studio'] == False), 'living_area'].isna().sum())
display(df.loc[df['studio'] == False, 'living_area'].describe())
```


    
![png](output_96_0.png)
    


    Количество пропущенных значений: 0



    count    23536.000000
    mean        34.672067
    std         22.149114
    min          7.381400
    25%         19.000000
    50%         30.000000
    75%         42.200000
    max        409.000000
    Name: living_area, dtype: float64


**Теперь выглядит корректно, двигаемся дальше.**

#### Столбец `floor`

[Содержание](#intro)

Пропусков в столбце нет. Посмотрим на значения:


```python
df['floor'].value_counts()
```




    2     3366
    3     3073
    1     2915
    4     2804
    5     2618
    6     1304
    7     1217
    8     1083
    9     1051
    10     686
    12     526
    11     523
    13     379
    15     342
    14     336
    16     315
    17     227
    18     178
    19     147
    21     125
    22     113
    20     110
    23     100
    24      63
    25      44
    26      24
    27      10
    28       1
    29       1
    32       1
    30       1
    33       1
    31       1
    Name: floor, dtype: int64



**Данные выглядят корректно, идем дальше.**

#### Столбец `is_apartment`

[Содержание](#intro)

В столбце имеются пропуски, скорее всего это "не апартаменты".

Подставим 0, поменяем тип на `bool` и на этом закончим.


```python
print('До:')
display(df['is_apartment'].value_counts())
df.loc[df['is_apartment'] != True, 'is_apartment'] = False
df['is_apartment'] = df['is_apartment'].astype('bool')
print('После:')
display(df['is_apartment'].value_counts())
```

    До:



    False    2724
    True       49
    Name: is_apartment, dtype: int64


    После:



    False    23636
    True        49
    Name: is_apartment, dtype: int64


**Готово, двигаемся дальше.**

#### Столбец  `studio`

[Содержание](#intro)

Корректный столбец. Мы с ним уже работали.


```python
display(df['studio'].value_counts())
display(df['studio'].dtype)
```


    False    23536
    True       149
    Name: studio, dtype: int64



    dtype('bool')


#### Столбец `open_plan`

[Содержание](#intro)

Корректный столбец.


```python
display(df['open_plan'].value_counts())
display(df['open_plan'].dtype)
```


    False    23619
    True        66
    Name: open_plan, dtype: int64



    dtype('bool')


#### Столбец `kitchen_area`

[Содержание](#intro)

В столбце имеются пропуски. Проверим данные и заполним значения по аналогии с столбцом `living_area`.

Сначала разберемся с квартирами студиями:


```python
print('Студий всего:', len(df[df['studio'] == True]))
print('Студии с указанной площадью кухни:') 
display(df.loc[df['studio'] == True, 'kitchen_area'].describe())
display(df[df['studio'] == True].head())
```

    Студий всего: 149
    Студии с указанной площадью кухни:



    count    0.0
    mean     NaN
    std      NaN
    min      NaN
    25%      NaN
    50%      NaN
    75%      NaN
    max      NaN
    Name: kitchen_area, dtype: float64



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
      <th>total_images</th>
      <th>last_price</th>
      <th>total_area</th>
      <th>first_day_exposition</th>
      <th>rooms</th>
      <th>ceiling_height</th>
      <th>floors_total</th>
      <th>living_area</th>
      <th>floor</th>
      <th>is_apartment</th>
      <th>...</th>
      <th>kitchen_area</th>
      <th>balcony</th>
      <th>locality_name</th>
      <th>airports_nearest</th>
      <th>city_centers_nearest</th>
      <th>parks_around3000</th>
      <th>parks_nearest</th>
      <th>ponds_around3000</th>
      <th>ponds_nearest</th>
      <th>days_exposition</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>144</th>
      <td>1</td>
      <td>2450000.0</td>
      <td>27.00</td>
      <td>2017-03-30</td>
      <td>1</td>
      <td>2.65</td>
      <td>24.0</td>
      <td>15.50</td>
      <td>2</td>
      <td>False</td>
      <td>...</td>
      <td>NaN</td>
      <td>2.0</td>
      <td>посёлок Мурино</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>181.0</td>
    </tr>
    <tr>
      <th>440</th>
      <td>8</td>
      <td>2480000.0</td>
      <td>27.11</td>
      <td>2018-03-12</td>
      <td>1</td>
      <td>2.65</td>
      <td>17.0</td>
      <td>24.75</td>
      <td>4</td>
      <td>False</td>
      <td>...</td>
      <td>NaN</td>
      <td>2.0</td>
      <td>Санкт-Петербург</td>
      <td>38171.0</td>
      <td>15015.0</td>
      <td>0.0</td>
      <td>NaN</td>
      <td>2.0</td>
      <td>982.0</td>
      <td>64.0</td>
    </tr>
    <tr>
      <th>608</th>
      <td>2</td>
      <td>1850000.0</td>
      <td>25.00</td>
      <td>2019-02-20</td>
      <td>1</td>
      <td>2.65</td>
      <td>10.0</td>
      <td>18.00</td>
      <td>7</td>
      <td>False</td>
      <td>...</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>посёлок Шушары</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>29.0</td>
    </tr>
    <tr>
      <th>697</th>
      <td>12</td>
      <td>2500000.0</td>
      <td>24.10</td>
      <td>2017-12-01</td>
      <td>1</td>
      <td>2.75</td>
      <td>25.0</td>
      <td>17.50</td>
      <td>21</td>
      <td>False</td>
      <td>...</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>деревня Кудрово</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>201.0</td>
    </tr>
    <tr>
      <th>716</th>
      <td>5</td>
      <td>1500000.0</td>
      <td>17.00</td>
      <td>2017-06-07</td>
      <td>1</td>
      <td>2.70</td>
      <td>9.0</td>
      <td>12.00</td>
      <td>1</td>
      <td>False</td>
      <td>...</td>
      <td>NaN</td>
      <td>0.0</td>
      <td>посёлок Шушары</td>
      <td>18654.0</td>
      <td>29846.0</td>
      <td>0.0</td>
      <td>NaN</td>
      <td>0.0</td>
      <td>NaN</td>
      <td>40.0</td>
    </tr>
  </tbody>
</table>
<p>5 rows × 22 columns</p>
</div>


Так как квартиры-студии это жилая комната объедененная с кухней, подставим вместо пропущенныйх значений - 0.


```python
df.loc[(df['studio'] == True), 'kitchen_area'] = 0
display(df.loc[df['studio'] == True, 'kitchen_area'].describe())
display(df[df['studio'] == True].head())
```


    count    149.0
    mean       0.0
    std        0.0
    min        0.0
    25%        0.0
    50%        0.0
    75%        0.0
    max        0.0
    Name: kitchen_area, dtype: float64



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
      <th>total_images</th>
      <th>last_price</th>
      <th>total_area</th>
      <th>first_day_exposition</th>
      <th>rooms</th>
      <th>ceiling_height</th>
      <th>floors_total</th>
      <th>living_area</th>
      <th>floor</th>
      <th>is_apartment</th>
      <th>...</th>
      <th>kitchen_area</th>
      <th>balcony</th>
      <th>locality_name</th>
      <th>airports_nearest</th>
      <th>city_centers_nearest</th>
      <th>parks_around3000</th>
      <th>parks_nearest</th>
      <th>ponds_around3000</th>
      <th>ponds_nearest</th>
      <th>days_exposition</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>144</th>
      <td>1</td>
      <td>2450000.0</td>
      <td>27.00</td>
      <td>2017-03-30</td>
      <td>1</td>
      <td>2.65</td>
      <td>24.0</td>
      <td>15.50</td>
      <td>2</td>
      <td>False</td>
      <td>...</td>
      <td>0.0</td>
      <td>2.0</td>
      <td>посёлок Мурино</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>181.0</td>
    </tr>
    <tr>
      <th>440</th>
      <td>8</td>
      <td>2480000.0</td>
      <td>27.11</td>
      <td>2018-03-12</td>
      <td>1</td>
      <td>2.65</td>
      <td>17.0</td>
      <td>24.75</td>
      <td>4</td>
      <td>False</td>
      <td>...</td>
      <td>0.0</td>
      <td>2.0</td>
      <td>Санкт-Петербург</td>
      <td>38171.0</td>
      <td>15015.0</td>
      <td>0.0</td>
      <td>NaN</td>
      <td>2.0</td>
      <td>982.0</td>
      <td>64.0</td>
    </tr>
    <tr>
      <th>608</th>
      <td>2</td>
      <td>1850000.0</td>
      <td>25.00</td>
      <td>2019-02-20</td>
      <td>1</td>
      <td>2.65</td>
      <td>10.0</td>
      <td>18.00</td>
      <td>7</td>
      <td>False</td>
      <td>...</td>
      <td>0.0</td>
      <td>NaN</td>
      <td>посёлок Шушары</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>29.0</td>
    </tr>
    <tr>
      <th>697</th>
      <td>12</td>
      <td>2500000.0</td>
      <td>24.10</td>
      <td>2017-12-01</td>
      <td>1</td>
      <td>2.75</td>
      <td>25.0</td>
      <td>17.50</td>
      <td>21</td>
      <td>False</td>
      <td>...</td>
      <td>0.0</td>
      <td>NaN</td>
      <td>деревня Кудрово</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>201.0</td>
    </tr>
    <tr>
      <th>716</th>
      <td>5</td>
      <td>1500000.0</td>
      <td>17.00</td>
      <td>2017-06-07</td>
      <td>1</td>
      <td>2.70</td>
      <td>9.0</td>
      <td>12.00</td>
      <td>1</td>
      <td>False</td>
      <td>...</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>посёлок Шушары</td>
      <td>18654.0</td>
      <td>29846.0</td>
      <td>0.0</td>
      <td>NaN</td>
      <td>0.0</td>
      <td>NaN</td>
      <td>40.0</td>
    </tr>
  </tbody>
</table>
<p>5 rows × 22 columns</p>
</div>


Со студиями разобрались, теперь переходим к обычным квартирам:


```python
print('Обычных квартир всего:', len(df[df['studio'] == False]))
print('Количество пропущенных значений:', df.loc[(df['studio'] == False), 'kitchen_area'].isna().sum())
print('Квартир с указанной площадью:') 
display(df.loc[df['studio'] == False, 'kitchen_area'].describe())
```

    Обычных квартир всего: 23536
    Количество пропущенных значений: 2125
    Квартир с указанной площадью:



    count    21411.000000
    mean        10.564481
    std          5.862196
    min          1.300000
    25%          7.000000
    50%          9.100000
    75%         12.000000
    max        107.000000
    Name: kitchen_area, dtype: float64


Имеются квартиры с аномально маленькой и большой площадью кухни, посмотрим на графики:


```python
other_plot2 = df[df['studio'] == False]
other_plot2.plot(y='kitchen_area', x='total_area', style="o", figsize=(15, 5), grid=True, xlim=(0, 200), ylim=(0, 100), alpha=0.3)
plt.show()
df['kitchen_area'].hist(figsize=(15,5), bins=60, range=(0, 5))
plt.show()
df['kitchen_area'].hist(figsize=(15,5), bins=60, range=(5, 50))
plt.show()
df['kitchen_area'].hist(figsize=(15,5), bins=60, range=(50, 100))
plt.show()
```


    
![png](output_120_0.png)
    



    
![png](output_120_1.png)
    



    
![png](output_120_2.png)
    



    
![png](output_120_3.png)
    


Выделяющихся значений мало. Поэтому скорректируем все значения меньше 3 и больше 60 кв.м., все что выходит за пределы заменим на максимальные значения.


```python
df.loc[(df['kitchen_area'] < 3) & (df['studio']==False), 'kitchen_area'] = 3
df.loc[(df['kitchen_area'] > 60) & (df['studio']==False), 'kitchen_area'] = 60

df.loc[(df['studio']==False),'kitchen_area'].describe()
```




    count    21411.000000
    mean        10.548887
    std          5.684628
    min          3.000000
    25%          7.000000
    50%          9.100000
    75%         12.000000
    max         60.000000
    Name: kitchen_area, dtype: float64



Теперь заполним пропуски в соотношении с общей площадью. Чем больше общая площадь - тем больше кухня.

Посмотрим соотношение и заполним пропуски:


```python
area2 = df.dropna(subset=['kitchen_area', 'total_area'])
ratio2 = (area2['kitchen_area'] / area2['total_area']).mean()
print('Кухня от общей площади занимает:',ratio2)
```

    Кухня от общей площади занимает: 0.18599091011239105



```python
df.loc[(df['studio'] == False), 'kitchen_area'] = df.loc[(df['studio'] == False), 'kitchen_area'].fillna(df.loc[(df['studio'] == False), 'total_area'] * 0.186)
df.plot(y='kitchen_area', x='total_area', style="o", figsize=(15, 5), grid=True, xlim=(0, 150), ylim=(0, 150), alpha=0.3)
plt.show()
print('Количество пропущенных значений:', df.loc[(df['studio'] == False), 'kitchen_area'].isna().sum())
display(df.loc[df['studio'] == False, 'kitchen_area'].describe())
```


    
![png](output_125_0.png)
    


    Количество пропущенных значений: 0



    count    23536.000000
    mean        10.575373
    std          5.870685
    min          2.232000
    25%          7.000000
    50%          9.100000
    75%         12.000000
    max         93.000000
    Name: kitchen_area, dtype: float64


**Аномалии устранены, пропуски заполненны.**

#### Столбец `balcony`

[Содержание](#intro)

Имеется много пропущенных значений. Возможно пропуски связаны с отсутсвимем балкона в квартире. 

Посмторим на данные.


```python
des_column('balcony')
print('Пропущенных значений:',df['balcony'].isna().sum())
```


    
![png](output_129_0.png)
    



    count    12175.000000
    mean         1.149651
    std          1.070895
    min          0.000000
    25%          0.000000
    50%          1.000000
    75%          2.000000
    max          5.000000
    Name: balcony, dtype: float64



    
![png](output_129_2.png)
    


    Пропущенных значений: 11510


Удалять такое количество строк нельзя. Так как в дальнейшем этот столбец нам не понадобится, то просто подставим вместо пропущенных значений - 0.


```python
df['balcony'] = df['balcony'].fillna(0)
print(df['balcony'].value_counts())
print('Пропущенных значений:',df['balcony'].isna().sum())
```

    0.0    15268
    1.0     4193
    2.0     3657
    5.0      303
    4.0      183
    3.0       81
    Name: balcony, dtype: int64
    Пропущенных значений: 0


**Готово, переходим к следующему столбцу.**

#### Столбец `locality_name`

[Содержание](#intro)

Пропусков мало, посмотрим на данные и решим, что будем делать:


```python
print('Количество пропущеных значений:', df['locality_name'].isna().sum())
print()
display(df['locality_name'].sort_values().unique())
```

    Количество пропущеных значений: 49
    



    array(['Бокситогорск', 'Волосово', 'Волхов', 'Всеволожск', 'Выборг',
           'Высоцк', 'Гатчина', 'Зеленогорск', 'Ивангород', 'Каменногорск',
           'Кингисепп', 'Кириши', 'Кировск', 'Колпино', 'Коммунар',
           'Красное Село', 'Кронштадт', 'Кудрово', 'Лодейное Поле',
           'Ломоносов', 'Луга', 'Любань', 'Мурино', 'Никольское',
           'Новая Ладога', 'Отрадное', 'Павловск', 'Петергоф', 'Пикалёво',
           'Подпорожье', 'Приморск', 'Приозерск', 'Пушкин', 'Санкт-Петербург',
           'Светогорск', 'Сертолово', 'Сестрорецк', 'Сланцы', 'Сосновый Бор',
           'Сясьстрой', 'Тихвин', 'Тосно', 'Шлиссельбург',
           'городской поселок Большая Ижора', 'городской поселок Янино-1',
           'городской посёлок Будогощь', 'городской посёлок Виллози',
           'городской посёлок Лесогорский', 'городской посёлок Мга',
           'городской посёлок Назия', 'городской посёлок Новоселье',
           'городской посёлок Павлово', 'городской посёлок Рощино',
           'городской посёлок Свирьстрой', 'городской посёлок Советский',
           'городской посёлок Фёдоровское', 'городской посёлок Янино-1',
           'деревня Агалатово', 'деревня Аро', 'деревня Батово',
           'деревня Бегуницы', 'деревня Белогорка', 'деревня Большая Вруда',
           'деревня Большая Пустомержа', 'деревня Большие Колпаны',
           'деревня Большое Рейзино', 'деревня Большой Сабск', 'деревня Бор',
           'деревня Борисова Грива', 'деревня Ваганово', 'деревня Вартемяги',
           'деревня Вахнова Кара', 'деревня Выскатка', 'деревня Гарболово',
           'деревня Глинка', 'деревня Горбунки', 'деревня Гостилицы',
           'деревня Заклинье', 'деревня Заневка', 'деревня Зимитицы',
           'деревня Извара', 'деревня Иссад', 'деревня Калитино',
           'деревня Кальтино', 'деревня Камышовка', 'деревня Каськово',
           'деревня Келози', 'деревня Кипень', 'деревня Кисельня',
           'деревня Колтуши', 'деревня Коркино', 'деревня Котлы',
           'деревня Кривко', 'деревня Кудрово', 'деревня Кузьмолово',
           'деревня Курковицы', 'деревня Куровицы', 'деревня Куттузи',
           'деревня Лаврики', 'деревня Лаголово', 'деревня Лампово',
           'деревня Лесколово', 'деревня Лопухинка', 'деревня Лупполово',
           'деревня Малая Романовка', 'деревня Малое Верево',
           'деревня Малое Карлино', 'деревня Малые Колпаны',
           'деревня Мануйлово', 'деревня Меньково', 'деревня Мины',
           'деревня Мистолово', 'деревня Ненимяки', 'деревня Нижние Осельки',
           'деревня Нижняя', 'деревня Низино', 'деревня Новое Девяткино',
           'деревня Новолисино', 'деревня Нурма', 'деревня Оржицы',
           'деревня Парицы', 'деревня Пельгора', 'деревня Пеники',
           'деревня Пижма', 'деревня Пикколово', 'деревня Пудомяги',
           'деревня Пустынка', 'деревня Пчева', 'деревня Рабитицы',
           'деревня Разбегаево', 'деревня Раздолье', 'деревня Разметелево',
           'деревня Рапполово', 'деревня Реброво', 'деревня Русско',
           'деревня Сижно', 'деревня Снегирёвка', 'деревня Старая',
           'деревня Старая Пустошь', 'деревня Старое Хинколово',
           'деревня Старополье', 'деревня Старосиверская',
           'деревня Старые Бегуницы', 'деревня Суоранда',
           'деревня Сяськелево', 'деревня Тарасово', 'деревня Терпилицы',
           'деревня Тихковицы', 'деревня Тойворово', 'деревня Торосово',
           'деревня Торошковичи', 'деревня Трубников Бор',
           'деревня Фалилеево', 'деревня Фёдоровское', 'деревня Хапо-Ое',
           'деревня Хязельки', 'деревня Чудской Бор', 'деревня Шпаньково',
           'деревня Щеглово', 'деревня Юкки', 'деревня Ялгино',
           'деревня Яльгелево', 'деревня Ям-Тесово',
           'коттеджный поселок Кивеннапа Север', 'коттеджный поселок Счастье',
           'коттеджный посёлок Лесное', 'поселок Аннино', 'поселок Барышево',
           'поселок Бугры', 'поселок Возрождение', 'поселок Войсковицы',
           'поселок Володарское', 'поселок Гаврилово', 'поселок Гарболово',
           'поселок Гладкое', 'поселок Глажево', 'поселок Глебычево',
           'поселок Гончарово', 'поселок Громово', 'поселок Дружноселье',
           'поселок Елизаветино', 'поселок Жилгородок', 'поселок Жилпосёлок',
           'поселок Житково', 'поселок Заводской', 'поселок Запорожское',
           'поселок Зимитицы', 'поселок Ильичёво', 'поселок Калитино',
           'поселок Каложицы', 'поселок Кингисеппский', 'поселок Кирпичное',
           'поселок Кобралово', 'поселок Кобринское', 'поселок Коммунары',
           'поселок Коробицыно', 'поселок Котельский',
           'поселок Красная Долина', 'поселок Красносельское',
           'поселок Лесное', 'поселок Лисий Нос', 'поселок Лукаши',
           'поселок Любань', 'поселок Мельниково', 'поселок Мичуринское',
           'поселок Молодцово', 'поселок Мурино', 'поселок Новый Свет',
           'поселок Новый Учхоз', 'поселок Оредеж',
           'поселок Пансионат Зелёный Бор', 'поселок Первомайское',
           'поселок Перово', 'поселок Петровское', 'поселок Победа',
           'поселок Поляны', 'поселок Почап', 'поселок Починок',
           'поселок Пушное', 'поселок Пчевжа', 'поселок Рабитицы',
           'поселок Романовка', 'поселок Ромашки', 'поселок Рябово',
           'поселок Севастьяново', 'поселок Селезнёво', 'поселок Сельцо',
           'поселок Семиозерье', 'поселок Семрино', 'поселок Серебрянский',
           'поселок Совхозный', 'поселок Старая Малукса',
           'поселок Стеклянный', 'поселок Сумино', 'поселок Суходолье',
           'поселок Тельмана', 'поселок Терволово', 'поселок Торковичи',
           'поселок Тёсово-4', 'поселок Углово', 'поселок Усть-Луга',
           'поселок Ушаки', 'поселок Цвелодубово', 'поселок Цвылёво',
           'поселок городского типа Большая Ижора',
           'поселок городского типа Вырица',
           'поселок городского типа Дружная Горка',
           'поселок городского типа Дубровка',
           'поселок городского типа Ефимовский',
           'поселок городского типа Кондратьево',
           'поселок городского типа Красный Бор',
           'поселок городского типа Кузьмоловский',
           'поселок городского типа Лебяжье',
           'поселок городского типа Лесогорский',
           'поселок городского типа Назия',
           'поселок городского типа Никольский',
           'поселок городского типа Приладожский',
           'поселок городского типа Рахья', 'поселок городского типа Рощино',
           'поселок городского типа Рябово',
           'поселок городского типа Синявино',
           'поселок городского типа Советский',
           'поселок городского типа Токсово',
           'поселок городского типа Форносово',
           'поселок городского типа имени Свердлова',
           'поселок станции Вещево', 'поселок станции Корнево',
           'поселок станции Лужайка', 'поселок станции Приветнинское',
           'посёлок Александровская', 'посёлок Алексеевка', 'посёлок Аннино',
           'посёлок Белоостров', 'посёлок Бугры', 'посёлок Возрождение',
           'посёлок Войскорово', 'посёлок Высокоключевой',
           'посёлок Гаврилово', 'посёлок Дзержинского', 'посёлок Жилгородок',
           'посёлок Ильичёво', 'посёлок Кикерино', 'посёлок Кобралово',
           'посёлок Коробицыно', 'посёлок Левашово', 'посёлок Ленинское',
           'посёлок Лисий Нос', 'посёлок Мельниково', 'посёлок Металлострой',
           'посёлок Мичуринское', 'посёлок Молодёжное', 'посёлок Мурино',
           'посёлок Мыза-Ивановка', 'посёлок Новогорелово',
           'посёлок Новый Свет', 'посёлок Пансионат Зелёный Бор',
           'посёлок Парголово', 'посёлок Перово', 'посёлок Песочный',
           'посёлок Петро-Славянка', 'посёлок Петровское',
           'посёлок Платформа 69-й километр', 'посёлок Плодовое',
           'посёлок Плоское', 'посёлок Победа', 'посёлок Поляны',
           'посёлок Понтонный', 'посёлок Пригородный', 'посёлок Пудость',
           'посёлок Репино', 'посёлок Ропша', 'посёлок Сапёрное',
           'посёлок Сапёрный', 'посёлок Сосново', 'посёлок Старая Малукса',
           'посёлок Стеклянный', 'посёлок Стрельна', 'посёлок Суйда',
           'посёлок Сумино', 'посёлок Тельмана', 'посёлок Терволово',
           'посёлок Торфяное', 'посёлок Усть-Ижора', 'посёлок Усть-Луга',
           'посёлок Форт Красная Горка', 'посёлок Шугозеро', 'посёлок Шушары',
           'посёлок Щеглово', 'посёлок городского типа Важины',
           'посёлок городского типа Вознесенье',
           'посёлок городского типа Вырица',
           'посёлок городского типа Красный Бор',
           'посёлок городского типа Кузнечное',
           'посёлок городского типа Кузьмоловский',
           'посёлок городского типа Лебяжье', 'посёлок городского типа Мга',
           'посёлок городского типа Павлово',
           'посёлок городского типа Рощино', 'посёлок городского типа Рябово',
           'посёлок городского типа Сиверский',
           'посёлок городского типа Тайцы', 'посёлок городского типа Токсово',
           'посёлок городского типа Ульяновка',
           'посёлок городского типа Форносово',
           'посёлок городского типа имени Морозова',
           'посёлок городского типа имени Свердлова',
           'посёлок при железнодорожной станции Вещево',
           'посёлок при железнодорожной станции Приветнинское',
           'посёлок станции Громово', 'посёлок станции Свирь',
           'садоводческое некоммерческое товарищество Лесная Поляна',
           'садовое товарищество Новая Ропша',
           'садовое товарищество Приладожский', 'садовое товарищество Рахья',
           'садовое товарищество Садко', 'село Копорье', 'село Никольское',
           'село Павлово', 'село Паша', 'село Путилово', 'село Рождествено',
           'село Русско-Высоцкое', 'село Старая Ладога', 'село Шум', nan],
          dtype=object)


Кроме пропусков, имеются также неявные дубликаты(написание слов с `е` - `ё`) и возможно разное написание вспомогательных слов.

**Вариант решения:**
- Привести все к нижнему регистру
- Удалить все вспомогательные слова
- Заменим все `ё` на `е`(на всякий случай)


```python
local_name = ['городской', 'поселок', 'посёлок', 'деревня', 'коттеджный', 'городского', 'типа', 'при', 'железнодорожной', 'станции', 'садоводческое', 'некоммерческое', 'садовое', 'товарищество', 'село']
#список значений, которые необходимо заменить
df['locality_name'] = df['locality_name'].str.lower() #приводим все к нижнему регистру
df['locality_name'] = df['locality_name'].replace(local_name, ' ', regex = True)#меняем список на пробел
df['locality_name'] = df['locality_name'].str.replace('ё', 'е', regex = True)
df['locality_name'] = df['locality_name'].str.strip()#удаляем пробелы
display(df['locality_name'].sort_values().unique())
```


    array(['агалатово', 'александровская', 'алексеевка', 'аннино', 'аро',
           'барышево', 'батово', 'бегуницы', 'белогорка', 'белоостров',
           'бокситогорск', 'большая вруда', 'большая ижора',
           'большая пустомержа', 'большие колпаны', 'большое рейзино',
           'большой сабск', 'бор', 'борисова грива', 'бугры', 'будогощь',
           'ваганово', 'важины', 'вартемяги', 'вахнова кара', 'ветнинское',
           'вещево', 'виллози', 'вознесенье', 'возрождение', 'войсковицы',
           'войскорово', 'володарское', 'волосово', 'волхов', 'всеволожск',
           'выборг', 'вырица', 'выскатка', 'высокоключевой', 'высоцк',
           'гаврилово', 'гарболово', 'гатчина', 'гладкое', 'глажево',
           'глебычево', 'глинка', 'гончарово', 'горбунки', 'городный',
           'гостилицы', 'громово', 'дзержинского', 'дружная горка',
           'дружноселье', 'дубровка', 'елизаветино', 'ефимовский', 'жил',
           'жилгородок', 'житково', 'заводской', 'заклинье', 'заневка',
           'запорожское', 'зеленогорск', 'зимитицы', 'ивангород', 'извара',
           'ильичево', 'имени морозова', 'имени свердлова', 'иссад',
           'калитино', 'каложицы', 'кальтино', 'каменногорск', 'камышовка',
           'каськово', 'келози', 'кивеннапа север', 'кикерино', 'кингисепп',
           'кингисеппский', 'кипень', 'кириши', 'кировск', 'кирпичное',
           'кисельня', 'кобралово', 'кобринское', 'колпино', 'колтуши',
           'коммунар', 'коммунары', 'кондратьево', 'копорье', 'коркино',
           'корнево', 'коробицыно', 'котельский', 'котлы', 'красная долина',
           'красное', 'красносельское', 'красный бор', 'кривко', 'кронштадт',
           'кудрово', 'кузнечное', 'кузьмолово', 'кузьмоловский', 'курковицы',
           'куровицы', 'куттузи', 'лаврики', 'лаголово', 'ладожский',
           'лампово', 'лебяжье', 'левашово', 'ленинское', 'лесколово',
           'лесная поляна', 'лесное', 'лесогорский', 'лисий нос',
           'лодейное поле', 'ломоносов', 'лопухинка', 'луга', 'лужайка',
           'лукаши', 'лупполово', 'любань', 'малая романовка', 'малое верево',
           'малое карлино', 'малые колпаны', 'мануйлово', 'мга', 'мельниково',
           'меньково', 'металлострой', 'мины', 'мистолово', 'мичуринское',
           'молодежное', 'молодцово', 'морск', 'мурино', 'мыза-ивановка',
           'назия', 'ненимяки', 'нижние осельки', 'нижняя', 'низино',
           'никольский', 'никольское', 'новая ладога', 'новая ропша',
           'новогорелово', 'новое девяткино', 'новолисино', 'новоселье',
           'новый свет', 'новый учхоз', 'нурма', 'озерск', 'оредеж', 'оржицы',
           'отрадное', 'павлово', 'павловск', 'пансионат зеленый бор',
           'парголово', 'парицы', 'паша', 'пельгора', 'пеники',
           'первомайское', 'перово', 'песочный', 'петергоф', 'петро-славянка',
           'петровское', 'пижма', 'пикалево', 'пикколово',
           'платформа 69-й километр', 'плодовое', 'плоское', 'победа',
           'подпорожье', 'поляны', 'понтонный', 'почап', 'починок',
           'пудомяги', 'пудость', 'пустынка', 'путилово', 'пушкин', 'пушное',
           'пчева', 'пчевжа', 'рабитицы', 'разбегаево', 'раздолье',
           'разметелево', 'рапполово', 'рахья', 'реброво', 'репино',
           'рождествено', 'романовка', 'ромашки', 'ропша', 'рощино', 'русско',
           'русско-высоцкое', 'рябово', 'садко', 'санкт-петербург',
           'саперное', 'саперный', 'светогорск', 'свирь', 'свирьстрой',
           'севастьяново', 'селезнево', 'сельцо', 'семиозерье', 'семрино',
           'серебрянский', 'сертолово', 'сестрорецк', 'сиверский', 'сижно',
           'синявино', 'сланцы', 'снегиревка', 'советский', 'совхозный',
           'сосново', 'сосновый бор', 'старая', 'старая ладога',
           'старая малукса', 'старая пустошь', 'старое хинколово',
           'старополье', 'старосиверская', 'старые бегуницы', 'стеклянный',
           'стрельна', 'суйда', 'сумино', 'суоранда', 'суходолье', 'счастье',
           'сяськелево', 'сясьстрой', 'тайцы', 'тарасово', 'тельмана',
           'терволово', 'терпилицы', 'тесово-4', 'тихвин', 'тихковицы',
           'тойворово', 'токсово', 'торковичи', 'торосово', 'торошковичи',
           'торфяное', 'тосно', 'трубников бор', 'углово', 'ульяновка',
           'усть-ижора', 'усть-луга', 'ушаки', 'фалилеево', 'федоровское',
           'форносово', 'форт красная горка', 'хапо-ое', 'хязельки',
           'цвелодубово', 'цвылево', 'чудской бор', 'шлиссельбург',
           'шпаньково', 'шугозеро', 'шум', 'шушары', 'щеглово', 'юкки',
           'ялгино', 'яльгелево', 'ям-тесово', 'янино-1', nan], dtype=object)


Готово, теперь разберемся с пропусками.
Попробуем определить населенный пункт по расстояниям.

Возьмем растояния до аэропорта и центра, в остальных много пропусков.


```python
loc_df_nan = df.loc[df['locality_name'].isna(), ['city_centers_nearest', 'airports_nearest']]
loc_df_nan.head()
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
      <th>city_centers_nearest</th>
      <th>airports_nearest</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>1097</th>
      <td>4258.0</td>
      <td>23478.0</td>
    </tr>
    <tr>
      <th>2033</th>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>2603</th>
      <td>17369.0</td>
      <td>22041.0</td>
    </tr>
    <tr>
      <th>2632</th>
      <td>17369.0</td>
      <td>22041.0</td>
    </tr>
    <tr>
      <th>3574</th>
      <td>8127.0</td>
      <td>27419.0</td>
    </tr>
  </tbody>
</table>
</div>



Удалим пропуски в `city_centers_nearest`, `airports_nearest`


```python
loc_df_nan = loc_df_nan.dropna(subset=['city_centers_nearest', 'airports_nearest'])
loc_df_nan.head()
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
      <th>city_centers_nearest</th>
      <th>airports_nearest</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>1097</th>
      <td>4258.0</td>
      <td>23478.0</td>
    </tr>
    <tr>
      <th>2603</th>
      <td>17369.0</td>
      <td>22041.0</td>
    </tr>
    <tr>
      <th>2632</th>
      <td>17369.0</td>
      <td>22041.0</td>
    </tr>
    <tr>
      <th>3574</th>
      <td>8127.0</td>
      <td>27419.0</td>
    </tr>
    <tr>
      <th>4151</th>
      <td>3902.0</td>
      <td>25054.0</td>
    </tr>
  </tbody>
</table>
</div>



С помощью цикла проверим совпадения по расстоянию в общем датафрейме, будем искать похожие значения этой пары столбцов.

Максимальное отклонение от значений возьмем 1000 м. (т.е. искомый `locality_name` будет в приделах 1км от найденого совпадения).


```python
loc_list=[] # сюда запишем совпадения 
delta = 1000 # отклонение, в пределах которого ищем совпадения
for j in loc_df_nan.index:
    city = loc_df_nan.loc[j, 'city_centers_nearest']
    air = loc_df_nan.loc[j, 'airports_nearest']
    loc_list = df.query('((@city-@delta) < city_centers_nearest < (@city+@delta)) and ((@air-@delta) < airports_nearest < (@air+@delta))')['locality_name']
print('Найденные совпадения:', loc_list.value_counts())
print()
print('Количество пропущеных значений в df:', df['locality_name'].isna().sum())
```

    Найденные совпадения: санкт-петербург    90
    Name: locality_name, dtype: int64
    
    Количество пропущеных значений в df: 49


Из полученных результатов можно сказать, что все объявления с незаполненным городом из Санкт-Петербурга. Теперь заполним пропущенные знчения.


```python
df.loc[df['locality_name'].isna(),'locality_name'] = 'санкт-петербург'
print('Количество пропущеных значений в `locality_name`:', df['locality_name'].isna().sum())
df['locality_name'].value_counts()
```

    Количество пропущеных значений в `locality_name`: 0





    санкт-петербург    15759
    мурино               590
    кудрово              472
    шушары               440
    всеволожск           398
                       ...  
    кирпичное              1
    кривко                 1
    левашово               1
    садко                  1
    снегиревка             1
    Name: locality_name, Length: 305, dtype: int64



**Столбец отредактирован, идем дальше**

#### Столбец `airports_nearest`

[Содержание](#intro)

В столбце много пропусков. Изучим данные и посмотрим, что можно сделать.


```python
des_column('airports_nearest')
print('Количество пропущеных значений в `airports_nearest`:', df['airports_nearest'].isna().sum())
```


    
![png](output_149_0.png)
    



    count    18145.000000
    mean     28794.758942
    std      12633.360549
    min          0.000000
    25%      18582.000000
    50%      26726.000000
    75%      37284.000000
    max      84869.000000
    Name: airports_nearest, dtype: float64



    
![png](output_149_2.png)
    


    Количество пропущеных значений в `airports_nearest`: 5540


Есть строки со значением 0, посмотрим сколько их и что за города.


```python
df['airports_nearest'].hist(range=(0,100),bins=100)
plt.show()
df[df['airports_nearest'] == 0]
```


    
![png](output_151_0.png)
    





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
      <th>total_images</th>
      <th>last_price</th>
      <th>total_area</th>
      <th>first_day_exposition</th>
      <th>rooms</th>
      <th>ceiling_height</th>
      <th>floors_total</th>
      <th>living_area</th>
      <th>floor</th>
      <th>is_apartment</th>
      <th>...</th>
      <th>kitchen_area</th>
      <th>balcony</th>
      <th>locality_name</th>
      <th>airports_nearest</th>
      <th>city_centers_nearest</th>
      <th>parks_around3000</th>
      <th>parks_nearest</th>
      <th>ponds_around3000</th>
      <th>ponds_nearest</th>
      <th>days_exposition</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>21085</th>
      <td>0</td>
      <td>7000000.0</td>
      <td>34.7</td>
      <td>2018-09-23</td>
      <td>1</td>
      <td>2.7</td>
      <td>9.0</td>
      <td>19.8</td>
      <td>3</td>
      <td>False</td>
      <td>...</td>
      <td>6.0</td>
      <td>2.0</td>
      <td>санкт-петербург</td>
      <td>0.0</td>
      <td>22801.0</td>
      <td>0.0</td>
      <td>NaN</td>
      <td>0.0</td>
      <td>NaN</td>
      <td>60.0</td>
    </tr>
  </tbody>
</table>
<p>1 rows × 22 columns</p>
</div>



Заполним медианным значением для этого города.


```python
df.loc[21085, 'airports_nearest'] = df.loc[(df['locality_name'] == 'санкт-петербург'), 'airports_nearest'].median()
df[df['airports_nearest'] == 0]
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
      <th>total_images</th>
      <th>last_price</th>
      <th>total_area</th>
      <th>first_day_exposition</th>
      <th>rooms</th>
      <th>ceiling_height</th>
      <th>floors_total</th>
      <th>living_area</th>
      <th>floor</th>
      <th>is_apartment</th>
      <th>...</th>
      <th>kitchen_area</th>
      <th>balcony</th>
      <th>locality_name</th>
      <th>airports_nearest</th>
      <th>city_centers_nearest</th>
      <th>parks_around3000</th>
      <th>parks_nearest</th>
      <th>ponds_around3000</th>
      <th>ponds_nearest</th>
      <th>days_exposition</th>
    </tr>
  </thead>
  <tbody>
  </tbody>
</table>
<p>0 rows × 22 columns</p>
</div>



Теперь для каждого населенного пункта нужно заполнить пропущенные значения медианами (если они есть). Перебирать в ручную очень долго, поэтому воспользуемся функцией.

- Сгруппируем по городам
- Посчитаем известные медианы
- Напишем функцию


```python
print('Медианы расстояний:')
loc_median = df.groupby('locality_name')['airports_nearest'].median()
loc_median = loc_median.dropna() #удалим города без значений
display(loc_median.head(10))

def med_nearest(nearest):
    if nearest in loc_median:
        return loc_median[nearest]
```

    Медианы расстояний:



    locality_name
    александровская    12896.5
    белоостров         57769.0
    зеленогорск        72282.0
    колпино            26232.0
    красное            25717.0
    кронштадт          67850.0
    левашово           52693.0
    лисий нос          55909.0
    ломоносов          48415.5
    металлострой       25758.0
    Name: airports_nearest, dtype: float64



```python
med_nearest('александровская')
```




    12896.5




```python
df.loc[df['airports_nearest'].isna(), 'airports_nearest'] = df['locality_name'].apply(med_nearest)
print('Количество пропущеных значений в `airports_nearest` до замены: 5540; после:', df['airports_nearest'].isna().sum())

```

    Количество пропущеных значений в `airports_nearest` до замены: 5540; после: 4827


**Пропуски остались, но в дальнейшем столбец не нужен. Переходим к cледующему.**

#### Столбец `city_centers_nearest`

[Содержание](#intro)

Изучим данные и заполним пропуски.


```python
des_column('city_centers_nearest')
print('Количество пропущеных значений в `city_centers_nearest`:', df['city_centers_nearest'].isna().sum())
```


    
![png](output_161_0.png)
    



    count    18168.000000
    mean     14192.647072
    std       8609.536392
    min        181.000000
    25%       9238.000000
    50%      13101.000000
    75%      16293.000000
    max      65968.000000
    Name: city_centers_nearest, dtype: float64



    
![png](output_161_2.png)
    


    Количество пропущеных значений в `city_centers_nearest`: 5517



```python
df['city_centers_nearest'].hist(figsize=(15,5), bins=100, range=(0, 200))
plt.show()
df['city_centers_nearest'].hist(figsize=(15,5), bins=100, range=(201, 17000))
plt.show()
df['city_centers_nearest'].hist(figsize=(15,5), bins=100, range=(17000, 66000))
plt.show()
```


    
![png](output_162_0.png)
    



    
![png](output_162_1.png)
    



    
![png](output_162_2.png)
    


Данные выглядят корректно, займемся пропусками.

Здесь поступим аналогично как с прошлым столбцом:
- Сгруппируем данные
- Применим функцию


```python
print('Медианы расстояний:')
loc_median = df.groupby('locality_name')['city_centers_nearest'].median()
loc_median = loc_median.dropna() #удалим города без значений
display(loc_median.head(10))
```

    Медианы расстояний:



    locality_name
    александровская    27468.0
    белоостров         38868.0
    зеленогорск        53381.0
    колпино            32018.0
    красное            29142.0
    кронштадт          49575.0
    левашово           25727.0
    лисий нос          28226.0
    ломоносов          51677.0
    металлострой       27602.0
    Name: city_centers_nearest, dtype: float64



```python
med_nearest('александровская')
```




    27468.0




```python
df.loc[df['city_centers_nearest'].isna(), 'city_centers_nearest'] = df['locality_name'].apply(med_nearest)
print('Количество пропущеных значений в `city_centers_nearest` до замены: 5517; после:', df['city_centers_nearest'].isna().sum())

```

    Количество пропущеных значений в `city_centers_nearest` до замены: 5517; после: 4827


Пропущенных значений осталось все так же много. Удалять такое количество строк или заполнять их медианой или средней нельзя, может исказится общая картина при анализе. 

**Оставим данные с таким количеством пропусков. Переходим дальше.**

#### Столбец `parks_around3000`

[Содержание](#intro)

Посмотрим на значения в столбце.


```python
df['parks_around3000'].value_counts(dropna=False)
```




    0.0    10101
    1.0     5676
    NaN     5516
    2.0     1745
    3.0      647
    Name: parks_around3000, dtype: int64



Логично предположить, что если значение не проставлено, то парков вокруг нет. Если в пределах этого радиуса ни парков не было, то и указать в этом столбце системе нечего. Пропуски трогать не будем.

**Переходим к следующему столбцу.**

#### Столбец `parks_nearest`

[Содержание](#intro)

Посмотрим данные и заполним пропуски.


```python
des_column('parks_nearest')
print('Количество пропущеных значений в `parks_nearest`:', df['parks_nearest'].isna().sum())
```


    
![png](output_174_0.png)
    



    count    8072.000000
    mean      490.762512
    std       342.404788
    min         1.000000
    25%       288.000000
    50%       455.000000
    75%       612.000000
    max      3190.000000
    Name: parks_nearest, dtype: float64



    
![png](output_174_2.png)
    


    Количество пропущеных значений в `parks_nearest`: 15613



```python
df['parks_nearest'].hist(figsize=(15,5), bins=100, range=(0, 200))
plt.show()
df['parks_nearest'].hist(figsize=(15,5), bins=100, range=(201, 600))
plt.show()
df['parks_nearest'].hist(figsize=(15,5), bins=100, range=(600, 3200))
plt.show()
```


    
![png](output_175_0.png)
    



    
![png](output_175_1.png)
    



    
![png](output_175_2.png)
    


Данные выглядят равномерно, удалять ничего не нужно.

Логично предположить, что если значение не проставлено, то парков вокруг нет. Если в пределах определенного радиуса ни парков не было, то и указать в этом столбце системе нечего. Пропуски трогать не будем.

**Оставим все как есть.**

#### Столбец `ponds_around3000`

[Содержание](#intro)

Проверим и заполним как `parks_around3000`


```python
df['ponds_around3000'].value_counts(dropna=False)
```




    0.0    9067
    1.0    5715
    NaN    5516
    2.0    1889
    3.0    1498
    Name: ponds_around3000, dtype: int64



Логично предположить, что если значение не проставлено, то водоемов вокруг нет. Если в пределах этого радиуса ни парков, ни водоемов не было, то и указать в этом столбце системе нечего. Пропуски трогать не будем.

**Переходим к следующему столбцу.**

#### Столбец `ponds_nearest`

[Содержание](#intro)

Есть пропуски. Посмотрим на данные:


```python
des_column('ponds_nearest')
print('Количество пропущеных значений в `ponds_nearest`:', df['ponds_nearest'].isna().sum())
```


    
![png](output_185_0.png)
    



    count    9102.000000
    mean      518.093386
    std       277.724575
    min        13.000000
    25%       294.000000
    50%       502.000000
    75%       729.750000
    max      1344.000000
    Name: ponds_nearest, dtype: float64



    
![png](output_185_2.png)
    


    Количество пропущеных значений в `ponds_nearest`: 14583



```python
df['ponds_nearest'].hist(figsize=(15,5), bins=100, range=(0, 200))
plt.show()
df['ponds_nearest'].hist(figsize=(15,5), bins=100, range=(201, 600))
plt.show()
df['ponds_nearest'].hist(figsize=(15,5), bins=100, range=(600, 1344))
plt.show()
```


    
![png](output_186_0.png)
    



    
![png](output_186_1.png)
    



    
![png](output_186_2.png)
    


Выбросов нет.

Логично предположить, что если значение не проставлено, то водоемов вокруг нет. Если в пределах нужного радиуса ни парков, ни водоемов не было, то и указать в этом столбце системе нечего. Пропуски трогать не будем.

Cделаем отступление и посмотрим на корреляцию:


```python
df.describe()
plt.figure(figsize=(15, 15))
sns.heatmap(df.corr(), annot=True, cmap="Blues", fmt='.2f')
```




    <AxesSubplot:>




    
![png](output_189_1.png)
    


По графику можем понять, что присутствие рядом парков и водоемов оказывает минимальное влиеняие на цену квартиры. Вообще многие из этих параметров оказывают довольно слабое влияние, их как раз можно объединить в одну группу.

Так же я думаю, что данный график хорошо подойдет для проведения исследовательского анализа в конце.

**Переходим к следующему столбцу.**

#### Столбец `days_exposition`

[Содержание](#intro)

В столбце много пропусков. Посмотрим на данные и решим, что делать.


```python
des_column('days_exposition')
print('Количество пропущеных значений в `days_exposition`:', df['days_exposition'].isna().sum())
```


    
![png](output_193_0.png)
    



    count    20504.000000
    mean       180.908701
    std        219.763436
    min          1.000000
    25%         45.000000
    50%         95.000000
    75%        232.000000
    max       1580.000000
    Name: days_exposition, dtype: float64



    
![png](output_193_2.png)
    


    Количество пропущеных значений в `days_exposition`: 3181



```python
df['days_exposition'].hist(figsize=(15,5), bins=100, range=(0, 100))
plt.show()
df['days_exposition'].hist(figsize=(15,5), bins=100, range=(101, 800))
plt.show()
df['days_exposition'].hist(figsize=(15,5), bins=100, range=(801, 1600))
plt.show()
```


    
![png](output_194_0.png)
    



    
![png](output_194_1.png)
    



    
![png](output_194_2.png)
    


Данные распределены равномерно, сильных выбросов нет. Удалять ничего не будем. Есть ярко выраженные пики значений.

Займемся пропусками значений.


```python
date_df = df[['first_day_exposition', 'days_exposition']]
date_df.head(15)
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
      <th>first_day_exposition</th>
      <th>days_exposition</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>2019-03-07</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>1</th>
      <td>2018-12-04</td>
      <td>81.0</td>
    </tr>
    <tr>
      <th>2</th>
      <td>2015-08-20</td>
      <td>558.0</td>
    </tr>
    <tr>
      <th>3</th>
      <td>2015-07-24</td>
      <td>424.0</td>
    </tr>
    <tr>
      <th>4</th>
      <td>2018-06-19</td>
      <td>121.0</td>
    </tr>
    <tr>
      <th>5</th>
      <td>2018-09-10</td>
      <td>55.0</td>
    </tr>
    <tr>
      <th>6</th>
      <td>2017-11-02</td>
      <td>155.0</td>
    </tr>
    <tr>
      <th>7</th>
      <td>2019-04-18</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>8</th>
      <td>2018-05-23</td>
      <td>189.0</td>
    </tr>
    <tr>
      <th>9</th>
      <td>2017-02-26</td>
      <td>289.0</td>
    </tr>
    <tr>
      <th>10</th>
      <td>2017-11-16</td>
      <td>137.0</td>
    </tr>
    <tr>
      <th>11</th>
      <td>2018-08-27</td>
      <td>7.0</td>
    </tr>
    <tr>
      <th>12</th>
      <td>2016-06-30</td>
      <td>90.0</td>
    </tr>
    <tr>
      <th>13</th>
      <td>2017-07-01</td>
      <td>366.0</td>
    </tr>
    <tr>
      <th>14</th>
      <td>2016-06-23</td>
      <td>203.0</td>
    </tr>
  </tbody>
</table>
</div>




```python
print('Пропущено значений в столбце:', df['days_exposition'].isna().sum())
date_df_nan = df.loc[df['days_exposition'].isna(), ['first_day_exposition', 'days_exposition']]
display(date_df_nan.sort_values('first_day_exposition').head(15))
```

    Пропущено значений в столбце: 3181



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
      <th>first_day_exposition</th>
      <th>days_exposition</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>3873</th>
      <td>2014-11-27</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>15857</th>
      <td>2014-11-27</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>3291</th>
      <td>2014-11-27</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>6922</th>
      <td>2014-12-08</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>15614</th>
      <td>2014-12-09</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>260</th>
      <td>2014-12-09</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>11811</th>
      <td>2014-12-09</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>5956</th>
      <td>2014-12-09</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>16327</th>
      <td>2014-12-09</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>3056</th>
      <td>2014-12-09</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>11055</th>
      <td>2014-12-09</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>3974</th>
      <td>2014-12-10</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>18386</th>
      <td>2014-12-10</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>5629</th>
      <td>2014-12-10</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>2814</th>
      <td>2014-12-11</td>
      <td>NaN</td>
    </tr>
  </tbody>
</table>
</div>


Если стоит пропуск, то скорее всего объявление на момент сбора данных было актуально. Тогда поле можно не заполнять или заполнить маркером, который позволит нам отличать такие объявления.


```python
df['days_exposition'] = df['days_exposition'].fillna(2222)
display(df.loc[df['first_day_exposition'] == '2014-11-27', ['first_day_exposition', 'days_exposition']])
print('Пропущено значений в столбце:', df['days_exposition'].isna().sum())
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
      <th>first_day_exposition</th>
      <th>days_exposition</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>617</th>
      <td>2014-11-27</td>
      <td>606.0</td>
    </tr>
    <tr>
      <th>696</th>
      <td>2014-11-27</td>
      <td>574.0</td>
    </tr>
    <tr>
      <th>2831</th>
      <td>2014-11-27</td>
      <td>1069.0</td>
    </tr>
    <tr>
      <th>3291</th>
      <td>2014-11-27</td>
      <td>2222.0</td>
    </tr>
    <tr>
      <th>3486</th>
      <td>2014-11-27</td>
      <td>1078.0</td>
    </tr>
    <tr>
      <th>3873</th>
      <td>2014-11-27</td>
      <td>2222.0</td>
    </tr>
    <tr>
      <th>3955</th>
      <td>2014-11-27</td>
      <td>583.0</td>
    </tr>
    <tr>
      <th>4812</th>
      <td>2014-11-27</td>
      <td>586.0</td>
    </tr>
    <tr>
      <th>6726</th>
      <td>2014-11-27</td>
      <td>1406.0</td>
    </tr>
    <tr>
      <th>7027</th>
      <td>2014-11-27</td>
      <td>1214.0</td>
    </tr>
    <tr>
      <th>8393</th>
      <td>2014-11-27</td>
      <td>972.0</td>
    </tr>
    <tr>
      <th>10132</th>
      <td>2014-11-27</td>
      <td>573.0</td>
    </tr>
    <tr>
      <th>10364</th>
      <td>2014-11-27</td>
      <td>1391.0</td>
    </tr>
    <tr>
      <th>13246</th>
      <td>2014-11-27</td>
      <td>1341.0</td>
    </tr>
    <tr>
      <th>15427</th>
      <td>2014-11-27</td>
      <td>1149.0</td>
    </tr>
    <tr>
      <th>15857</th>
      <td>2014-11-27</td>
      <td>2222.0</td>
    </tr>
    <tr>
      <th>16159</th>
      <td>2014-11-27</td>
      <td>1076.0</td>
    </tr>
    <tr>
      <th>20635</th>
      <td>2014-11-27</td>
      <td>1002.0</td>
    </tr>
    <tr>
      <th>21867</th>
      <td>2014-11-27</td>
      <td>1107.0</td>
    </tr>
  </tbody>
</table>
</div>


    Пропущено значений в столбце: 0


#### Поиск и удаление дубликатов

[Содержание](#intro)

Искать дубликаты будем в столбцах,  в которых были все значения изначально и особенно в которых маловероятны совпадения:
- `total_area`
- `last_price`
- `first_day_exposition`
- `total_images`


```python
dupl = df[df[['total_images', 'last_price','first_day_exposition','total_area']].duplicated(keep=False)]
dupl.sort_values(by='total_area')
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
      <th>total_images</th>
      <th>last_price</th>
      <th>total_area</th>
      <th>first_day_exposition</th>
      <th>rooms</th>
      <th>ceiling_height</th>
      <th>floors_total</th>
      <th>living_area</th>
      <th>floor</th>
      <th>is_apartment</th>
      <th>...</th>
      <th>kitchen_area</th>
      <th>balcony</th>
      <th>locality_name</th>
      <th>airports_nearest</th>
      <th>city_centers_nearest</th>
      <th>parks_around3000</th>
      <th>parks_nearest</th>
      <th>ponds_around3000</th>
      <th>ponds_nearest</th>
      <th>days_exposition</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>19052</th>
      <td>3</td>
      <td>4370000.0</td>
      <td>38.00</td>
      <td>2016-06-23</td>
      <td>1</td>
      <td>2.65</td>
      <td>9.0</td>
      <td>19.5000</td>
      <td>1</td>
      <td>False</td>
      <td>...</td>
      <td>7.0680</td>
      <td>0.0</td>
      <td>санкт-петербург</td>
      <td>27103.0</td>
      <td>7640.0</td>
      <td>1.0</td>
      <td>624.0</td>
      <td>0.0</td>
      <td>NaN</td>
      <td>620.0</td>
    </tr>
    <tr>
      <th>19325</th>
      <td>3</td>
      <td>4370000.0</td>
      <td>38.00</td>
      <td>2016-06-23</td>
      <td>1</td>
      <td>2.65</td>
      <td>9.0</td>
      <td>21.5764</td>
      <td>2</td>
      <td>False</td>
      <td>...</td>
      <td>7.0680</td>
      <td>0.0</td>
      <td>санкт-петербург</td>
      <td>27103.0</td>
      <td>7640.0</td>
      <td>1.0</td>
      <td>624.0</td>
      <td>0.0</td>
      <td>NaN</td>
      <td>620.0</td>
    </tr>
    <tr>
      <th>9661</th>
      <td>1</td>
      <td>2533531.0</td>
      <td>42.50</td>
      <td>2016-09-08</td>
      <td>1</td>
      <td>2.56</td>
      <td>18.0</td>
      <td>19.8000</td>
      <td>7</td>
      <td>False</td>
      <td>...</td>
      <td>7.9050</td>
      <td>0.0</td>
      <td>никольское</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>627.0</td>
    </tr>
    <tr>
      <th>18425</th>
      <td>1</td>
      <td>2533531.0</td>
      <td>42.50</td>
      <td>2016-09-08</td>
      <td>1</td>
      <td>2.56</td>
      <td>18.0</td>
      <td>20.0000</td>
      <td>10</td>
      <td>False</td>
      <td>...</td>
      <td>7.9050</td>
      <td>0.0</td>
      <td>никольское</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>627.0</td>
    </tr>
    <tr>
      <th>10099</th>
      <td>10</td>
      <td>4400000.0</td>
      <td>44.00</td>
      <td>2017-11-22</td>
      <td>1</td>
      <td>2.65</td>
      <td>17.0</td>
      <td>17.0000</td>
      <td>14</td>
      <td>False</td>
      <td>...</td>
      <td>13.0000</td>
      <td>2.0</td>
      <td>санкт-петербург</td>
      <td>42901.0</td>
      <td>9267.0</td>
      <td>0.0</td>
      <td>NaN</td>
      <td>0.0</td>
      <td>NaN</td>
      <td>71.0</td>
    </tr>
    <tr>
      <th>21142</th>
      <td>10</td>
      <td>4400000.0</td>
      <td>44.00</td>
      <td>2017-11-22</td>
      <td>2</td>
      <td>2.65</td>
      <td>9.0</td>
      <td>27.0000</td>
      <td>8</td>
      <td>False</td>
      <td>...</td>
      <td>6.0000</td>
      <td>1.0</td>
      <td>санкт-петербург</td>
      <td>49917.0</td>
      <td>16755.0</td>
      <td>1.0</td>
      <td>235.0</td>
      <td>0.0</td>
      <td>NaN</td>
      <td>72.0</td>
    </tr>
    <tr>
      <th>14739</th>
      <td>10</td>
      <td>5142565.0</td>
      <td>54.65</td>
      <td>2018-10-01</td>
      <td>2</td>
      <td>2.65</td>
      <td>5.0</td>
      <td>26.0000</td>
      <td>3</td>
      <td>False</td>
      <td>...</td>
      <td>10.1649</td>
      <td>0.0</td>
      <td>санкт-петербург</td>
      <td>47303.0</td>
      <td>25866.0</td>
      <td>1.0</td>
      <td>251.0</td>
      <td>1.0</td>
      <td>350.0</td>
      <td>145.0</td>
    </tr>
    <tr>
      <th>18624</th>
      <td>10</td>
      <td>5142565.0</td>
      <td>54.65</td>
      <td>2018-10-01</td>
      <td>2</td>
      <td>2.65</td>
      <td>5.0</td>
      <td>26.0000</td>
      <td>5</td>
      <td>False</td>
      <td>...</td>
      <td>10.1649</td>
      <td>0.0</td>
      <td>санкт-петербург</td>
      <td>47303.0</td>
      <td>25866.0</td>
      <td>1.0</td>
      <td>251.0</td>
      <td>1.0</td>
      <td>350.0</td>
      <td>145.0</td>
    </tr>
  </tbody>
</table>
<p>8 rows × 22 columns</p>
</div>



- 19052 и 19325 - отличается только этаж, удалим 19325
- 9661 и 18425 - отличается этаж, а площадь почти одинаковая, удалим 18425
- 10099 и 21142 - много отличий, оставим оба
- 14739 и 18624 - отличается только этаж, удалим 18624


```python
df = df.drop([19325,18425,18624], axis=0)
dupl = df[df[['total_images', 'last_price','first_day_exposition','total_area']].duplicated(keep=False)]
dupl.sort_values(by='total_area')
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
      <th>total_images</th>
      <th>last_price</th>
      <th>total_area</th>
      <th>first_day_exposition</th>
      <th>rooms</th>
      <th>ceiling_height</th>
      <th>floors_total</th>
      <th>living_area</th>
      <th>floor</th>
      <th>is_apartment</th>
      <th>...</th>
      <th>kitchen_area</th>
      <th>balcony</th>
      <th>locality_name</th>
      <th>airports_nearest</th>
      <th>city_centers_nearest</th>
      <th>parks_around3000</th>
      <th>parks_nearest</th>
      <th>ponds_around3000</th>
      <th>ponds_nearest</th>
      <th>days_exposition</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>10099</th>
      <td>10</td>
      <td>4400000.0</td>
      <td>44.0</td>
      <td>2017-11-22</td>
      <td>1</td>
      <td>2.65</td>
      <td>17.0</td>
      <td>17.0</td>
      <td>14</td>
      <td>False</td>
      <td>...</td>
      <td>13.0</td>
      <td>2.0</td>
      <td>санкт-петербург</td>
      <td>42901.0</td>
      <td>9267.0</td>
      <td>0.0</td>
      <td>NaN</td>
      <td>0.0</td>
      <td>NaN</td>
      <td>71.0</td>
    </tr>
    <tr>
      <th>21142</th>
      <td>10</td>
      <td>4400000.0</td>
      <td>44.0</td>
      <td>2017-11-22</td>
      <td>2</td>
      <td>2.65</td>
      <td>9.0</td>
      <td>27.0</td>
      <td>8</td>
      <td>False</td>
      <td>...</td>
      <td>6.0</td>
      <td>1.0</td>
      <td>санкт-петербург</td>
      <td>49917.0</td>
      <td>16755.0</td>
      <td>1.0</td>
      <td>235.0</td>
      <td>0.0</td>
      <td>NaN</td>
      <td>72.0</td>
    </tr>
  </tbody>
</table>
<p>2 rows × 22 columns</p>
</div>



**Дубликаты удалены. Идем дальше.**

#### Изменение типов данных 

[Содержание](#intro)

Поменяем тип в столбцах:

- last_price - в целочисленный тип (копейки нам не нужны);
- total_area, living_area, kitchen_area  - округлим до 1го знака после запятой (так проще воспринимать);
- first_day_exposition - уже поменяли;
- ceiling_height - округлим до 1го знака после запятой (так проще воспринимать);
- floors_total - в целочисленный тип (половины этажей не бывает);
- balcony - в целочисленный тип (половины болкона тоже);
- airports_nearest, city_centers_nearest, parks_around3000, parks_nearest, ponds_around3000, ponds_nearest - все расстояния указаны в метрах, переводим в целочисленный формат (так проще воспринимать);
- days_exposition - в целочисленный тип (счиатать целыми днями проще:)).


```python
#меняем на целочисленный тип
list_to_int = ['last_price', 'floors_total', 'balcony', 'airports_nearest', 'city_centers_nearest', 'parks_around3000',
               'parks_nearest', 'ponds_around3000', 'ponds_nearest', 'days_exposition']
for column in list_to_int:
    df[column] = df[df[column].notnull()][column].astype('int')

#оставляем один знак после запятой  
list_to_float = ['total_area', 'ceiling_height', 'living_area', 'kitchen_area']

for column in list_to_float:
    df[column] = df[df[column].notnull()][column].round(1)
    
display(df.head(10))

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
      <th>total_images</th>
      <th>last_price</th>
      <th>total_area</th>
      <th>first_day_exposition</th>
      <th>rooms</th>
      <th>ceiling_height</th>
      <th>floors_total</th>
      <th>living_area</th>
      <th>floor</th>
      <th>is_apartment</th>
      <th>...</th>
      <th>kitchen_area</th>
      <th>balcony</th>
      <th>locality_name</th>
      <th>airports_nearest</th>
      <th>city_centers_nearest</th>
      <th>parks_around3000</th>
      <th>parks_nearest</th>
      <th>ponds_around3000</th>
      <th>ponds_nearest</th>
      <th>days_exposition</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>20</td>
      <td>13000000</td>
      <td>108.0</td>
      <td>2019-03-07</td>
      <td>3</td>
      <td>2.7</td>
      <td>16</td>
      <td>51.0</td>
      <td>8</td>
      <td>False</td>
      <td>...</td>
      <td>25.0</td>
      <td>0</td>
      <td>санкт-петербург</td>
      <td>18863.0</td>
      <td>16028.0</td>
      <td>1.0</td>
      <td>482.0</td>
      <td>2.0</td>
      <td>755.0</td>
      <td>2222</td>
    </tr>
    <tr>
      <th>1</th>
      <td>7</td>
      <td>3350000</td>
      <td>40.4</td>
      <td>2018-12-04</td>
      <td>1</td>
      <td>2.6</td>
      <td>11</td>
      <td>18.6</td>
      <td>1</td>
      <td>False</td>
      <td>...</td>
      <td>11.0</td>
      <td>2</td>
      <td>шушары</td>
      <td>12817.0</td>
      <td>18603.0</td>
      <td>0.0</td>
      <td>NaN</td>
      <td>0.0</td>
      <td>NaN</td>
      <td>81</td>
    </tr>
    <tr>
      <th>2</th>
      <td>10</td>
      <td>5196000</td>
      <td>56.0</td>
      <td>2015-08-20</td>
      <td>2</td>
      <td>2.6</td>
      <td>5</td>
      <td>34.3</td>
      <td>4</td>
      <td>False</td>
      <td>...</td>
      <td>8.3</td>
      <td>0</td>
      <td>санкт-петербург</td>
      <td>21741.0</td>
      <td>13933.0</td>
      <td>1.0</td>
      <td>90.0</td>
      <td>2.0</td>
      <td>574.0</td>
      <td>558</td>
    </tr>
    <tr>
      <th>3</th>
      <td>0</td>
      <td>64900000</td>
      <td>159.0</td>
      <td>2015-07-24</td>
      <td>3</td>
      <td>2.6</td>
      <td>14</td>
      <td>90.3</td>
      <td>9</td>
      <td>False</td>
      <td>...</td>
      <td>29.6</td>
      <td>0</td>
      <td>санкт-петербург</td>
      <td>28098.0</td>
      <td>6800.0</td>
      <td>2.0</td>
      <td>84.0</td>
      <td>3.0</td>
      <td>234.0</td>
      <td>424</td>
    </tr>
    <tr>
      <th>4</th>
      <td>2</td>
      <td>10000000</td>
      <td>100.0</td>
      <td>2018-06-19</td>
      <td>2</td>
      <td>3.0</td>
      <td>14</td>
      <td>32.0</td>
      <td>13</td>
      <td>False</td>
      <td>...</td>
      <td>41.0</td>
      <td>0</td>
      <td>санкт-петербург</td>
      <td>31856.0</td>
      <td>8098.0</td>
      <td>2.0</td>
      <td>112.0</td>
      <td>1.0</td>
      <td>48.0</td>
      <td>121</td>
    </tr>
    <tr>
      <th>5</th>
      <td>10</td>
      <td>2890000</td>
      <td>30.4</td>
      <td>2018-09-10</td>
      <td>1</td>
      <td>2.6</td>
      <td>12</td>
      <td>14.4</td>
      <td>5</td>
      <td>False</td>
      <td>...</td>
      <td>9.1</td>
      <td>0</td>
      <td>янино-1</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>55</td>
    </tr>
    <tr>
      <th>6</th>
      <td>6</td>
      <td>3700000</td>
      <td>37.3</td>
      <td>2017-11-02</td>
      <td>1</td>
      <td>2.6</td>
      <td>26</td>
      <td>10.6</td>
      <td>6</td>
      <td>False</td>
      <td>...</td>
      <td>14.4</td>
      <td>1</td>
      <td>парголово</td>
      <td>52996.0</td>
      <td>19143.0</td>
      <td>0.0</td>
      <td>NaN</td>
      <td>0.0</td>
      <td>NaN</td>
      <td>155</td>
    </tr>
    <tr>
      <th>7</th>
      <td>5</td>
      <td>7915000</td>
      <td>71.6</td>
      <td>2019-04-18</td>
      <td>2</td>
      <td>2.6</td>
      <td>24</td>
      <td>40.7</td>
      <td>22</td>
      <td>False</td>
      <td>...</td>
      <td>18.9</td>
      <td>2</td>
      <td>санкт-петербург</td>
      <td>23982.0</td>
      <td>11634.0</td>
      <td>0.0</td>
      <td>NaN</td>
      <td>0.0</td>
      <td>NaN</td>
      <td>2222</td>
    </tr>
    <tr>
      <th>8</th>
      <td>20</td>
      <td>2900000</td>
      <td>33.2</td>
      <td>2018-05-23</td>
      <td>1</td>
      <td>2.6</td>
      <td>27</td>
      <td>15.4</td>
      <td>26</td>
      <td>False</td>
      <td>...</td>
      <td>8.8</td>
      <td>0</td>
      <td>мурино</td>
      <td>51553.0</td>
      <td>21888.0</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>189</td>
    </tr>
    <tr>
      <th>9</th>
      <td>18</td>
      <td>5400000</td>
      <td>61.0</td>
      <td>2017-02-26</td>
      <td>3</td>
      <td>2.5</td>
      <td>9</td>
      <td>43.6</td>
      <td>7</td>
      <td>False</td>
      <td>...</td>
      <td>6.5</td>
      <td>2</td>
      <td>санкт-петербург</td>
      <td>50898.0</td>
      <td>15008.0</td>
      <td>0.0</td>
      <td>NaN</td>
      <td>0.0</td>
      <td>NaN</td>
      <td>289</td>
    </tr>
  </tbody>
</table>
<p>10 rows × 22 columns</p>
</div>


#### Вывод

[Содержание](#intro)


```python
df.info()
```

    <class 'pandas.core.frame.DataFrame'>
    Int64Index: 23682 entries, 0 to 23698
    Data columns (total 22 columns):
     #   Column                Non-Null Count  Dtype         
    ---  ------                --------------  -----         
     0   total_images          23682 non-null  int64         
     1   last_price            23682 non-null  int64         
     2   total_area            23682 non-null  float64       
     3   first_day_exposition  23682 non-null  datetime64[ns]
     4   rooms                 23682 non-null  int64         
     5   ceiling_height        23682 non-null  float64       
     6   floors_total          23682 non-null  int64         
     7   living_area           23682 non-null  float64       
     8   floor                 23682 non-null  int64         
     9   is_apartment          23682 non-null  bool          
     10  studio                23682 non-null  bool          
     11  open_plan             23682 non-null  bool          
     12  kitchen_area          23682 non-null  float64       
     13  balcony               23682 non-null  int64         
     14  locality_name         23682 non-null  object        
     15  airports_nearest      18856 non-null  float64       
     16  city_centers_nearest  18856 non-null  float64       
     17  parks_around3000      18167 non-null  float64       
     18  parks_nearest         8070 non-null   float64       
     19  ponds_around3000      18167 non-null  float64       
     20  ponds_nearest         9101 non-null   float64       
     21  days_exposition       23682 non-null  int64         
    dtypes: bool(3), datetime64[ns](1), float64(10), int64(7), object(1)
    memory usage: 3.7+ MB


После обработки данных из 23699 осталось 23682 строк, удалено 0,07% данных.
- Большая часть пропусков заменена на медианные значения.
- Удалено 3 дубликата
- Обрезаны сильные выбросы в столбцах, там где они были.
- В столбцах с расстояниями заменили пропуски не полностью, так как если заполнить все данные медианой, могут исказиться результаты исследования.
- В числовых столбцах заменены типы на int, кроме столбцов с площадями, высотой потолка  и расстояниями(округлено до 0,1)

### Посчитайте и добавьте в таблицу новые столбцы

#### Цена одного квадратного метра

[Содержание](#intro)


```python
df['price_metr'] = df['last_price'] / df['total_area']
df['price_metr'] = df['price_metr'].astype(int)
display(df.head())
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
      <th>total_images</th>
      <th>last_price</th>
      <th>total_area</th>
      <th>first_day_exposition</th>
      <th>rooms</th>
      <th>ceiling_height</th>
      <th>floors_total</th>
      <th>living_area</th>
      <th>floor</th>
      <th>is_apartment</th>
      <th>...</th>
      <th>balcony</th>
      <th>locality_name</th>
      <th>airports_nearest</th>
      <th>city_centers_nearest</th>
      <th>parks_around3000</th>
      <th>parks_nearest</th>
      <th>ponds_around3000</th>
      <th>ponds_nearest</th>
      <th>days_exposition</th>
      <th>price_metr</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>20</td>
      <td>13000000</td>
      <td>108.0</td>
      <td>2019-03-07</td>
      <td>3</td>
      <td>2.7</td>
      <td>16</td>
      <td>51.0</td>
      <td>8</td>
      <td>False</td>
      <td>...</td>
      <td>0</td>
      <td>санкт-петербург</td>
      <td>18863.0</td>
      <td>16028.0</td>
      <td>1.0</td>
      <td>482.0</td>
      <td>2.0</td>
      <td>755.0</td>
      <td>2222</td>
      <td>120370</td>
    </tr>
    <tr>
      <th>1</th>
      <td>7</td>
      <td>3350000</td>
      <td>40.4</td>
      <td>2018-12-04</td>
      <td>1</td>
      <td>2.6</td>
      <td>11</td>
      <td>18.6</td>
      <td>1</td>
      <td>False</td>
      <td>...</td>
      <td>2</td>
      <td>шушары</td>
      <td>12817.0</td>
      <td>18603.0</td>
      <td>0.0</td>
      <td>NaN</td>
      <td>0.0</td>
      <td>NaN</td>
      <td>81</td>
      <td>82920</td>
    </tr>
    <tr>
      <th>2</th>
      <td>10</td>
      <td>5196000</td>
      <td>56.0</td>
      <td>2015-08-20</td>
      <td>2</td>
      <td>2.6</td>
      <td>5</td>
      <td>34.3</td>
      <td>4</td>
      <td>False</td>
      <td>...</td>
      <td>0</td>
      <td>санкт-петербург</td>
      <td>21741.0</td>
      <td>13933.0</td>
      <td>1.0</td>
      <td>90.0</td>
      <td>2.0</td>
      <td>574.0</td>
      <td>558</td>
      <td>92785</td>
    </tr>
    <tr>
      <th>3</th>
      <td>0</td>
      <td>64900000</td>
      <td>159.0</td>
      <td>2015-07-24</td>
      <td>3</td>
      <td>2.6</td>
      <td>14</td>
      <td>90.3</td>
      <td>9</td>
      <td>False</td>
      <td>...</td>
      <td>0</td>
      <td>санкт-петербург</td>
      <td>28098.0</td>
      <td>6800.0</td>
      <td>2.0</td>
      <td>84.0</td>
      <td>3.0</td>
      <td>234.0</td>
      <td>424</td>
      <td>408176</td>
    </tr>
    <tr>
      <th>4</th>
      <td>2</td>
      <td>10000000</td>
      <td>100.0</td>
      <td>2018-06-19</td>
      <td>2</td>
      <td>3.0</td>
      <td>14</td>
      <td>32.0</td>
      <td>13</td>
      <td>False</td>
      <td>...</td>
      <td>0</td>
      <td>санкт-петербург</td>
      <td>31856.0</td>
      <td>8098.0</td>
      <td>2.0</td>
      <td>112.0</td>
      <td>1.0</td>
      <td>48.0</td>
      <td>121</td>
      <td>100000</td>
    </tr>
  </tbody>
</table>
<p>5 rows × 23 columns</p>
</div>


#### День недели публикации объявления | Месяц публикации объявления | Год публикации объявления

[Содержание](#intro)


```python
df['weekday'] = df['first_day_exposition'].dt.weekday
df['month'] = df['first_day_exposition'].dt.month
df['year'] = df['first_day_exposition'].dt.year
display(df.head())
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
      <th>total_images</th>
      <th>last_price</th>
      <th>total_area</th>
      <th>first_day_exposition</th>
      <th>rooms</th>
      <th>ceiling_height</th>
      <th>floors_total</th>
      <th>living_area</th>
      <th>floor</th>
      <th>is_apartment</th>
      <th>...</th>
      <th>city_centers_nearest</th>
      <th>parks_around3000</th>
      <th>parks_nearest</th>
      <th>ponds_around3000</th>
      <th>ponds_nearest</th>
      <th>days_exposition</th>
      <th>price_metr</th>
      <th>weekday</th>
      <th>month</th>
      <th>year</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>20</td>
      <td>13000000</td>
      <td>108.0</td>
      <td>2019-03-07</td>
      <td>3</td>
      <td>2.7</td>
      <td>16</td>
      <td>51.0</td>
      <td>8</td>
      <td>False</td>
      <td>...</td>
      <td>16028.0</td>
      <td>1.0</td>
      <td>482.0</td>
      <td>2.0</td>
      <td>755.0</td>
      <td>2222</td>
      <td>120370</td>
      <td>3</td>
      <td>3</td>
      <td>2019</td>
    </tr>
    <tr>
      <th>1</th>
      <td>7</td>
      <td>3350000</td>
      <td>40.4</td>
      <td>2018-12-04</td>
      <td>1</td>
      <td>2.6</td>
      <td>11</td>
      <td>18.6</td>
      <td>1</td>
      <td>False</td>
      <td>...</td>
      <td>18603.0</td>
      <td>0.0</td>
      <td>NaN</td>
      <td>0.0</td>
      <td>NaN</td>
      <td>81</td>
      <td>82920</td>
      <td>1</td>
      <td>12</td>
      <td>2018</td>
    </tr>
    <tr>
      <th>2</th>
      <td>10</td>
      <td>5196000</td>
      <td>56.0</td>
      <td>2015-08-20</td>
      <td>2</td>
      <td>2.6</td>
      <td>5</td>
      <td>34.3</td>
      <td>4</td>
      <td>False</td>
      <td>...</td>
      <td>13933.0</td>
      <td>1.0</td>
      <td>90.0</td>
      <td>2.0</td>
      <td>574.0</td>
      <td>558</td>
      <td>92785</td>
      <td>3</td>
      <td>8</td>
      <td>2015</td>
    </tr>
    <tr>
      <th>3</th>
      <td>0</td>
      <td>64900000</td>
      <td>159.0</td>
      <td>2015-07-24</td>
      <td>3</td>
      <td>2.6</td>
      <td>14</td>
      <td>90.3</td>
      <td>9</td>
      <td>False</td>
      <td>...</td>
      <td>6800.0</td>
      <td>2.0</td>
      <td>84.0</td>
      <td>3.0</td>
      <td>234.0</td>
      <td>424</td>
      <td>408176</td>
      <td>4</td>
      <td>7</td>
      <td>2015</td>
    </tr>
    <tr>
      <th>4</th>
      <td>2</td>
      <td>10000000</td>
      <td>100.0</td>
      <td>2018-06-19</td>
      <td>2</td>
      <td>3.0</td>
      <td>14</td>
      <td>32.0</td>
      <td>13</td>
      <td>False</td>
      <td>...</td>
      <td>8098.0</td>
      <td>2.0</td>
      <td>112.0</td>
      <td>1.0</td>
      <td>48.0</td>
      <td>121</td>
      <td>100000</td>
      <td>1</td>
      <td>6</td>
      <td>2018</td>
    </tr>
  </tbody>
</table>
<p>5 rows × 26 columns</p>
</div>


#### Тип этажа квартиры

[Содержание](#intro)


```python
def floor_type(row):
    if row['floor'] == 1:
        return 'первый'
    elif row['floor'] == row['floors_total']:
        return 'последний'
    else:
        return 'другой'
df['floor_type'] = df.apply(floor_type, axis=1)
df.head()
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
      <th>total_images</th>
      <th>last_price</th>
      <th>total_area</th>
      <th>first_day_exposition</th>
      <th>rooms</th>
      <th>ceiling_height</th>
      <th>floors_total</th>
      <th>living_area</th>
      <th>floor</th>
      <th>is_apartment</th>
      <th>...</th>
      <th>parks_around3000</th>
      <th>parks_nearest</th>
      <th>ponds_around3000</th>
      <th>ponds_nearest</th>
      <th>days_exposition</th>
      <th>price_metr</th>
      <th>weekday</th>
      <th>month</th>
      <th>year</th>
      <th>floor_type</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>20</td>
      <td>13000000</td>
      <td>108.0</td>
      <td>2019-03-07</td>
      <td>3</td>
      <td>2.7</td>
      <td>16</td>
      <td>51.0</td>
      <td>8</td>
      <td>False</td>
      <td>...</td>
      <td>1.0</td>
      <td>482.0</td>
      <td>2.0</td>
      <td>755.0</td>
      <td>2222</td>
      <td>120370</td>
      <td>3</td>
      <td>3</td>
      <td>2019</td>
      <td>другой</td>
    </tr>
    <tr>
      <th>1</th>
      <td>7</td>
      <td>3350000</td>
      <td>40.4</td>
      <td>2018-12-04</td>
      <td>1</td>
      <td>2.6</td>
      <td>11</td>
      <td>18.6</td>
      <td>1</td>
      <td>False</td>
      <td>...</td>
      <td>0.0</td>
      <td>NaN</td>
      <td>0.0</td>
      <td>NaN</td>
      <td>81</td>
      <td>82920</td>
      <td>1</td>
      <td>12</td>
      <td>2018</td>
      <td>первый</td>
    </tr>
    <tr>
      <th>2</th>
      <td>10</td>
      <td>5196000</td>
      <td>56.0</td>
      <td>2015-08-20</td>
      <td>2</td>
      <td>2.6</td>
      <td>5</td>
      <td>34.3</td>
      <td>4</td>
      <td>False</td>
      <td>...</td>
      <td>1.0</td>
      <td>90.0</td>
      <td>2.0</td>
      <td>574.0</td>
      <td>558</td>
      <td>92785</td>
      <td>3</td>
      <td>8</td>
      <td>2015</td>
      <td>другой</td>
    </tr>
    <tr>
      <th>3</th>
      <td>0</td>
      <td>64900000</td>
      <td>159.0</td>
      <td>2015-07-24</td>
      <td>3</td>
      <td>2.6</td>
      <td>14</td>
      <td>90.3</td>
      <td>9</td>
      <td>False</td>
      <td>...</td>
      <td>2.0</td>
      <td>84.0</td>
      <td>3.0</td>
      <td>234.0</td>
      <td>424</td>
      <td>408176</td>
      <td>4</td>
      <td>7</td>
      <td>2015</td>
      <td>другой</td>
    </tr>
    <tr>
      <th>4</th>
      <td>2</td>
      <td>10000000</td>
      <td>100.0</td>
      <td>2018-06-19</td>
      <td>2</td>
      <td>3.0</td>
      <td>14</td>
      <td>32.0</td>
      <td>13</td>
      <td>False</td>
      <td>...</td>
      <td>2.0</td>
      <td>112.0</td>
      <td>1.0</td>
      <td>48.0</td>
      <td>121</td>
      <td>100000</td>
      <td>1</td>
      <td>6</td>
      <td>2018</td>
      <td>другой</td>
    </tr>
  </tbody>
</table>
<p>5 rows × 27 columns</p>
</div>



#### Расстояние до центра города в километрах

[Содержание](#intro)


```python
df['city_centers_nearest_km'] = (df['city_centers_nearest'] / 1000).round()
df.head()
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
      <th>total_images</th>
      <th>last_price</th>
      <th>total_area</th>
      <th>first_day_exposition</th>
      <th>rooms</th>
      <th>ceiling_height</th>
      <th>floors_total</th>
      <th>living_area</th>
      <th>floor</th>
      <th>is_apartment</th>
      <th>...</th>
      <th>parks_nearest</th>
      <th>ponds_around3000</th>
      <th>ponds_nearest</th>
      <th>days_exposition</th>
      <th>price_metr</th>
      <th>weekday</th>
      <th>month</th>
      <th>year</th>
      <th>floor_type</th>
      <th>city_centers_nearest_km</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>20</td>
      <td>13000000</td>
      <td>108.0</td>
      <td>2019-03-07</td>
      <td>3</td>
      <td>2.7</td>
      <td>16</td>
      <td>51.0</td>
      <td>8</td>
      <td>False</td>
      <td>...</td>
      <td>482.0</td>
      <td>2.0</td>
      <td>755.0</td>
      <td>2222</td>
      <td>120370</td>
      <td>3</td>
      <td>3</td>
      <td>2019</td>
      <td>другой</td>
      <td>16.0</td>
    </tr>
    <tr>
      <th>1</th>
      <td>7</td>
      <td>3350000</td>
      <td>40.4</td>
      <td>2018-12-04</td>
      <td>1</td>
      <td>2.6</td>
      <td>11</td>
      <td>18.6</td>
      <td>1</td>
      <td>False</td>
      <td>...</td>
      <td>NaN</td>
      <td>0.0</td>
      <td>NaN</td>
      <td>81</td>
      <td>82920</td>
      <td>1</td>
      <td>12</td>
      <td>2018</td>
      <td>первый</td>
      <td>19.0</td>
    </tr>
    <tr>
      <th>2</th>
      <td>10</td>
      <td>5196000</td>
      <td>56.0</td>
      <td>2015-08-20</td>
      <td>2</td>
      <td>2.6</td>
      <td>5</td>
      <td>34.3</td>
      <td>4</td>
      <td>False</td>
      <td>...</td>
      <td>90.0</td>
      <td>2.0</td>
      <td>574.0</td>
      <td>558</td>
      <td>92785</td>
      <td>3</td>
      <td>8</td>
      <td>2015</td>
      <td>другой</td>
      <td>14.0</td>
    </tr>
    <tr>
      <th>3</th>
      <td>0</td>
      <td>64900000</td>
      <td>159.0</td>
      <td>2015-07-24</td>
      <td>3</td>
      <td>2.6</td>
      <td>14</td>
      <td>90.3</td>
      <td>9</td>
      <td>False</td>
      <td>...</td>
      <td>84.0</td>
      <td>3.0</td>
      <td>234.0</td>
      <td>424</td>
      <td>408176</td>
      <td>4</td>
      <td>7</td>
      <td>2015</td>
      <td>другой</td>
      <td>7.0</td>
    </tr>
    <tr>
      <th>4</th>
      <td>2</td>
      <td>10000000</td>
      <td>100.0</td>
      <td>2018-06-19</td>
      <td>2</td>
      <td>3.0</td>
      <td>14</td>
      <td>32.0</td>
      <td>13</td>
      <td>False</td>
      <td>...</td>
      <td>112.0</td>
      <td>1.0</td>
      <td>48.0</td>
      <td>121</td>
      <td>100000</td>
      <td>1</td>
      <td>6</td>
      <td>2018</td>
      <td>другой</td>
      <td>8.0</td>
    </tr>
  </tbody>
</table>
<p>5 rows × 28 columns</p>
</div>



### Проведите исследовательский анализ данных

#### Изучаем параметры объектов

[Содержание](#intro)

Проведем анализ по группам показателей. Для каждого показателя построим график.

**общая площадь; количество комнат; этаж квартиры; жилая площадь; площадь кухни;**


```python
print('total_area')
df['total_area'].hist(figsize=(15,5), bins=100, range=(0, 300))
plt.show()
print('rooms')
df['rooms'].hist(figsize=(15,5), bins=10, range=(1, 10))
plt.show()
print('floor')
df['floor'].hist(figsize=(15,5), bins=60, range=(1, 60))
plt.show()
```

    total_area



    
![png](output_225_1.png)
    


    rooms



    
![png](output_225_3.png)
    


    floor



    
![png](output_225_5.png)
    


На графиках видно, что больше всего квартир с площадями 30-60 кв.м - это самые популярные 1 и 2-х комнатные. Следом идут 3 комнатные с площадью до 80 кв.м. Типичное расположение этих квартир с 1 по 10 этаж.

Так же можно заметить, что по мере роста площади квартир, их количество начинает убывать - обратная зависимость. Также квартиры меньше 30 кв.м тоже довольно редки. 

Распределение более похоже на распределение Пуассона.


```python
print('living_area')
df['living_area'].hist(figsize=(15,5), bins=100, range=(0, 200))
plt.show()
print('kitchen_area')
df['kitchen_area'].hist(figsize=(15,5), bins=100, range=(0, 60))
plt.show()
```

    living_area



    
![png](output_227_1.png)
    


    kitchen_area



    
![png](output_227_3.png)
    


На этих двух графиках похожая картина. Видна чёткая зависимость жилой и кухонной площадей от общей. С ростом площади уменьшается количество квартир.

Наиболее популярна жилая площадь от 15 до 40 метров, кухня от 7 до 10 квадратных метра.

На графике с жилой площадью заметен провал в районе 25 кв.м., скорее всего, это связано с переходом от однокомнатных квартир к двухкомнатным (первый всплеск - количество 1 + 2 комнатных квартир, второй - только 2 комнатные). Проще говоря, однокомнатных квартир с жилой площадью 25 кв.м. мало, а двухкомнатных с такой площадью еще нет или их количество равнозначно мало.

**высота потолков; общее количество этажей в доме;**


```python
print('ceiling_height')
df['ceiling_height'].hist(figsize=(15,5), bins=60, range=(1, 6))
plt.show()
print('floors_total')
df['floors_total'].hist(figsize=(15,5), bins=60, range=(1, 60))
plt.show()
```

    ceiling_height



    
![png](output_230_1.png)
    


    floors_total



    
![png](output_230_3.png)
    


Больше всего квартир с высотой потолков 2.5 - 3 м, подавляющее большинство с высотой 2,6 - 2,7 см. Остальное - нестандартный размер и чем выше тем реже встречается. Ниже нормального диапазона - тоже большая редкость. Похоже на распределение Пуассона

Так же больше всего квартир продается в пяти и девятиэтажных домах, скорее всего это самые распространенные дома советской постройки в старых районах города.

**цена объекта; тип этажа квартиры («первый», «последний», «другой»); день и месяц публикации объявления;**


```python
print('last_price')
df['last_price'].hist(figsize=(15,5), bins=100, range=(0, 10000000))
plt.show()
print('floor_type')
df['floor_type'].hist(figsize=(15,5), bins=3, range=(0, 3))
plt.show()
print('weekday')
df['weekday'].hist(figsize=(15,5), bins=7, range=(0, 7))
plt.show()
print('month')
df['month'].hist(figsize=(15,5), bins=12, range=(0, 12))
plt.show()
```

    last_price



    
![png](output_233_1.png)
    


    floor_type



    
![png](output_233_3.png)
    


    weekday



    
![png](output_233_5.png)
    


    month



    
![png](output_233_7.png)
    


Можно сделать вывод, что больше всего квартир продаются по цене в диапазоне 2 - 7 млн.руб. Основная масса продается в цене около 4 мнн. А в остальных случаях чем ниже или выше цена количество объявлений пропорционально падает. Распределение более похоже на распределение Пуассона.

Логично, что больше всего квартир продается в "середине дома", так как дома в основном многоэтажные. Количество объявлений с типом "последний" чуть больше, возможно из-за того, что квартиры на первых этажах сдаются в аренду как коммерческая недвижимость.

Чаще всего объявления публикуются в буднии дни, скорее всего это связано с графиком работы риелторов.

Больше всего объявлений опубликовано в декабре, возможно это связано с несколькими факторами:
- стремлением сделать подарок на НГ;
- продать "подороже", так как перед НГ спрос всегда увеличивается;
- продать квартиру перед началом календарного года, чтобы потом не платить налог.

Так же много объявлений подается в весенние месяцы, в этот период как правило спрос тоже увеличивается.

**расстояние до центра города в метрах; расстояние до ближайшего аэропорта; расстояние до ближайшего парка;**


```python
print('city_centers_nearest')
df['city_centers_nearest'].hist(figsize=(15,5), bins=100, range=(0, 70000))
plt.show()
print('airports_nearest')
df['airports_nearest'].hist(figsize=(15,5), bins=100, range=(0, 85000))
plt.show()
print('parks_nearest')
df['parks_nearest'].hist(figsize=(15,5), bins=100, range=(0, 2000))
plt.show()
```

    city_centers_nearest



    
![png](output_236_1.png)
    


    airports_nearest



    
![png](output_236_3.png)
    


    parks_nearest



    
![png](output_236_5.png)
    


Больше всего квартир продается на расстоянии от 12 до 15км.

Растояния до аэропорта приобладает в промежутке 15 - 40 км.

Растояние до парка от нескольких метров до 750. 

На всех трех графиках видны  отдельные пиковые значения. Из этого можно сделать вывод, что большое количество данных взято из одного населенного пункта.

Наблюдается слабая зависимость количества объявлений от растояния до парка. При этом обратная картина для расстояния до центра или аэропорта (чем больше расстояние, тем меньше объявлений).


#### Изучаем, как быстро продавались квартиры

[Содержание](#intro)


```python
des_column('days_exposition')
```


    
![png](output_239_0.png)
    



    count    23682.000000
    mean       455.035132
    std        725.440182
    min          1.000000
    25%         45.000000
    50%        124.000000
    75%        390.000000
    max       2222.000000
    Name: days_exposition, dtype: float64



    
![png](output_239_2.png)
    



```python
print('days_exposition')
df['days_exposition'].hist(figsize=(15,5), bins=100, range=(0, 400))
plt.show()
df['days_exposition'].hist(figsize=(15,5), bins=100, range=(401, 1400))
plt.show()
df['days_exposition'].hist(figsize=(15,5), bins=100, range=(600, 2300))
plt.show()
```

    days_exposition



    
![png](output_240_1.png)
    



    
![png](output_240_2.png)
    



    
![png](output_240_3.png)
    



```python
print('Медиана =', df['days_exposition'].median())
print('Средняя =', df['days_exposition'].mean())
```

    Медиана = 124.0
    Средняя = 455.0351321678912


На графиках видно, что больше всего квартир продается (примерно) за 45 и 60 дней (пиковые значения на первой и второй гистограммах). На 90 днях так же заметен скачек, возможно есть алгоритм закрывающий объявления в этот срок автоматически или запрашивающий подтверждение у владельца. 

Все что продается раньше 40 дней можно назвать быстрой продажей (находится до первого пика). Долгими продажами будем считать все после 100 дней.

К необычно долгим я бы отнес все после 200 дней - третий квартиль, отсечка 3 четвертей(75% значений)

На последней гистограмме мы видим большое количество обьявлений которые, скорее всего, на момент сбора данных были актуальны. 

#### Факторы больше всего влияющие на общую (полную) стоимость объекта

[Содержание](#intro)

Изучим, зависит ли цена от:
- общей площади;
- жилой площади;
- площади кухни;
- количества комнат;
- этажа, на котором расположена квартира (первый, последний, другой);
- даты размещения (день недели, месяц, год). 

**Зависимость цены от общей площади**


```python
def cor(column):
    print('Корелляция цены от', column, df['last_price'].corr(df[column]))
    df.plot(x=column, y='last_price', kind='scatter', grid=True, figsize=(15,5), alpha=0.3)

cor('total_area')
```

    Корелляция цены от total_area 0.6946030124715974



    
![png](output_246_1.png)
    


Очевидно, что зависимость цены от площади есть, но она не сильная. При возрастании площади возрастает цена.

**Зависимость цены от жилой площади**


```python
cor('living_area')
```

    Корелляция цены от living_area 0.6234437671821476



    
![png](output_249_1.png)
    


Аналогичная картина как и на прошлом графике. Зависимость цены от площади есть, но она не сильная. При возрастании площади возрастает цена. Но корреляция уже немного меньше.

**Зависимость цены от площади кухнии**


```python
cor('kitchen_area')
```

    Корелляция цены от kitchen_area 0.5341827697256121



    
![png](output_252_1.png)
    


Корреляция еще меньше. Можно сказать, что ависимость все еще прослеживается, но уже не всегда.

**Зависимость цены от количества комнат**


```python
cor('rooms')
```

    Корелляция цены от rooms 0.39435080252686583



    
![png](output_255_1.png)
    


Корелляция крайне мала, по графику можно сказать что она работает до 4 комнатных квартир. 

**Зависимость цены от этажа, на котором расположена квартира (первый, последний, другой)**


```python
floor_type = df.groupby('floor_type')['last_price'].mean()
floor_type.plot(x=floor_type.index, y=floor_type.values, kind='bar', grid=True, figsize=(6,5))
```




    <AxesSubplot:xlabel='floor_type'>




    
![png](output_258_1.png)
    


Судя по графику дороже всего продаются квартиры на последнем этаже. Квартиры на первом этаже предпочитают меньше всего.

**Зависимость цены от даты размещения (день недели, месяц, год)**


```python
cor('weekday')
cor('month')
cor('year')
```

    Корелляция цены от weekday -0.0023317528494038405
    Корелляция цены от month -0.0023624366068073234
    Корелляция цены от year -0.04864496571084729



    
![png](output_261_1.png)
    



    
![png](output_261_2.png)
    



    
![png](output_261_3.png)
    


Цена не зависит от дня недели, месяца и года размещения объявления. Но из исследования выше было видно, что колличество расмещений в будние дни выше.

**ВЫВОД: Больше всего на стоимость квартиры влияет её площадь(прямопропорцианальная зависимость). Среднее влияние оказывает жилая площадь и площадь кухни. Количество комнат и тип этажа вляют меньше всего. Дата размещения не оказывает влияния на цену. Стоит обратить внимание на тип этажа, есть существенная разница между первым и последним этажами.**

#### Средняя цена одного квадратного метра.

[Содержание](#intro)

Посчитем среднюю цену одного квадратного метра в 10 населённых пунктах с наибольшим числом объявлений. Выделим населённые пункты с самой высокой и низкой стоимостью квадратного метра.


```python
top10 = df.pivot_table(index = 'locality_name', values = 'price_metr', aggfunc=['count', 'mean'])
top10.columns = ['count', 'price_metr']
top10 = top10.sort_values(by='count', ascending=False).head(10)

display(top10)

print('Населённый пункт с cамой высокой стоимость квадратного метра:')
print(top10.loc[top10['price_metr'] == top10['price_metr'].max()])
print()
print('Населённый пункт c cамой низкой стоимость квадратного метра:')
print(top10.loc[top10['price_metr'] == top10['price_metr'].min()])
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
      <th>count</th>
      <th>price_metr</th>
    </tr>
    <tr>
      <th>locality_name</th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>санкт-петербург</th>
      <td>15757</td>
      <td>114624.802310</td>
    </tr>
    <tr>
      <th>мурино</th>
      <td>590</td>
      <td>86087.125424</td>
    </tr>
    <tr>
      <th>кудрово</th>
      <td>472</td>
      <td>95323.154661</td>
    </tr>
    <tr>
      <th>шушары</th>
      <td>440</td>
      <td>78677.525000</td>
    </tr>
    <tr>
      <th>всеволожск</th>
      <td>398</td>
      <td>68654.894472</td>
    </tr>
    <tr>
      <th>пушкин</th>
      <td>369</td>
      <td>103125.379404</td>
    </tr>
    <tr>
      <th>колпино</th>
      <td>338</td>
      <td>75423.905325</td>
    </tr>
    <tr>
      <th>парголово</th>
      <td>327</td>
      <td>90176.813456</td>
    </tr>
    <tr>
      <th>гатчина</th>
      <td>307</td>
      <td>68745.641694</td>
    </tr>
    <tr>
      <th>выборг</th>
      <td>237</td>
      <td>58141.345992</td>
    </tr>
  </tbody>
</table>
</div>


    Населённый пункт с cамой высокой стоимость квадратного метра:
                     count    price_metr
    locality_name                       
    санкт-петербург  15757  114624.80231
    
    Населённый пункт c cамой низкой стоимость квадратного метра:
                   count    price_metr
    locality_name                     
    выборг           237  58141.345992


#### Средняя цена километра в Санкт-Петербурге

[Содержание](#intro)

Вычислим среднюю цену каждого километра в Санкт-Петербурге.


```python
spb = df[df['locality_name'] == 'санкт-петербург']
price_km_spb = spb.pivot_table(index='city_centers_nearest_km', values='last_price', aggfunc='mean')
price_km_spb.head()
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
      <th>last_price</th>
    </tr>
    <tr>
      <th>city_centers_nearest_km</th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0.0</th>
      <td>3.144912e+07</td>
    </tr>
    <tr>
      <th>1.0</th>
      <td>2.115871e+07</td>
    </tr>
    <tr>
      <th>2.0</th>
      <td>1.780829e+07</td>
    </tr>
    <tr>
      <th>3.0</th>
      <td>1.110271e+07</td>
    </tr>
    <tr>
      <th>4.0</th>
      <td>1.219186e+07</td>
    </tr>
  </tbody>
</table>
</div>




```python
price_km_spb.plot(kind='bar', grid=True, figsize=(15,5))
```




    <AxesSubplot:xlabel='city_centers_nearest_km'>




    
![png](output_270_1.png)
    


По гистограмме видно, что наибольшая цена на расстоянии до 1 км от центра  (около 32 млн.р за квартиру) и сильно меняется уже после 1 км. от центра (17 млн.р за квартиру)

Радиус до 1 км от центра - это "самый центр".

Еще одна граница находится в районе 7 км от центра, после которой следует сильное падение цены (с 15 млн до 9 млн) и после начинает убывать постепенно. 

Сильный скачек в конце гистграммы связан с одной дорогой квартирой стоимостью 17 122 148 и площадью 178.3.


```python
display(spb.query('(city_centers_nearest_km > 40)').sort_values(by='last_price', ascending=False))

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
      <th>total_images</th>
      <th>last_price</th>
      <th>total_area</th>
      <th>first_day_exposition</th>
      <th>rooms</th>
      <th>ceiling_height</th>
      <th>floors_total</th>
      <th>living_area</th>
      <th>floor</th>
      <th>is_apartment</th>
      <th>...</th>
      <th>parks_nearest</th>
      <th>ponds_around3000</th>
      <th>ponds_nearest</th>
      <th>days_exposition</th>
      <th>price_metr</th>
      <th>weekday</th>
      <th>month</th>
      <th>year</th>
      <th>floor_type</th>
      <th>city_centers_nearest_km</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>21276</th>
      <td>0</td>
      <td>17122148</td>
      <td>178.3</td>
      <td>2017-02-10</td>
      <td>1</td>
      <td>2.6</td>
      <td>3</td>
      <td>101.2</td>
      <td>1</td>
      <td>False</td>
      <td>...</td>
      <td>NaN</td>
      <td>0.0</td>
      <td>NaN</td>
      <td>4</td>
      <td>96029</td>
      <td>4</td>
      <td>2</td>
      <td>2017</td>
      <td>первый</td>
      <td>41.0</td>
    </tr>
  </tbody>
</table>
<p>1 rows × 28 columns</p>
</div>


### Общий вывод

[Содержание](#intro)

Для анализа была дана база с информацией по продаже квартир. Состоял из 22 колонок и 23699 строк. 
В процессе предобработки было удалено 17 строк (осталось 23682 строк, удалено 0,07% данных) - это хорошо.

Далее:
- в стлобце `ceiling_height` пропуски заменены медианами. И здесь были обнаружены ошибочные значения, в которых небыло запятой - исправили;
- столбец `first_day_exposition` приведен к типу datetime64;
- в столбцах `living_area` и `kitchen_area` произведена замена медианами в зависимости от общей площади. Так же учтены квартиры студии - в строках для студии в столбце`kitchen_area` постевлено 0;
- в числовых столбцах заменены типы на int, кроме столбцов с площадями и расстояниями(округлено до 0,1);
- в столбце `days_exposition` пропуски заполненны медианами в зависимости от населенного пункта;
- обрезаны сильные выбросы в столбцах где они имели место;
- удалено 3 дубликата;

По заданию было добавлено 7 столбцов, в которых были подсчитаны дополнительные параметры:
- цена квадратного метра;
- день недели, месяц и год публикации объявления;
- тип этажа квартиры (значения — «первый», «последний», «другой»);
- соотношение жилой и общей площади, а также отношение площади кухни к общей.

Был проведен анализ и сделаны выводы:

Больше всего квартир с площадями 30-60 кв.м - это самые популярные 1 и 2-х комнатные. Следом идут 3 комнатные с площадью до 80 кв.м. Типичное расположение этих квартир с 1 по 10 этаж.
Так же можно заметить, что по мере роста площади квартир, их количество начинает убывать - обратная зависимость. Также квартиры меньше 30 кв.м тоже довольно редки.
Распределение более похоже на распределение Пуассона.
Видна чёткая зависимость жилой и кухонной площадей от общей. С ростом площади уменьшается количество квартир.
Наиболее популярна жилая площадь от 15 до 40 метров, кухня от 7 до 10 квадратных метра.
На графике с жилой площадью заметен провал в районе 25 кв.м., скорее всего, это связано с переходом от однокомнатных квартир к двухкомнатным (первый всплеск - количество 1 + 2 комнатных квартир, второй - только 2 комнатные). Проще говоря, однокомнатных квартир с жилой площадью 25 кв.м. мало, а двухкомнатных с такой площадью еще нет или их количество равнозначно мало. 
Больше всего квартир с высотой потолков 2.5 - 3 м из них подавляющее большинство с высотой 2,6 - 2,7 см. Остальное - нестандартный размер и чем выше - тем реже встречается. Ниже нормального диапазона - тоже большая редкость. Похоже на распределение Пуассона

Видно, что больше всего квартир продается в пяти и девятиэтажных домах, скорее всего это самые распространенные дома советской постройки в старых районах города. 

Заметно, что наибольше количество квартир продаются по цене в диапазоне 2 - 7 млн.руб. Основная масса продается в цене около 4 мнн. А в остальных случаях чем ниже или выше цена количество объявлений пропорционально падает. Распределение более похоже на распределение Пуассона.

Логично, что больше всего квартир продается в "середине дома", так как дома в основном многоэтажные. Количество объявлений с типом "последний" чуть больше, возможно из-за того, что квартиры на первых этажах сдаются в аренду как коммерческая недвижимость.

Чаще всего объявления публикуются в буднии дни, скорее всего это связано с графиком работы риелторов.

Больше всего объявлений опубликовано в декабре, возможно это связано с несколькими факторами:

- стремлением сделать подарок на НГ;
- продать "подороже", так как перед НГ спрос всегда увеличивается;
- продать квартиру перед началом календарного года, чтобы потом не платить налог.
Так же много объявлений подается в весенние месяцы, в этот период как правило спрос тоже увеличивается.

На графикав видно, что больше всего квартир продается на расстоянии от 12 до 15км.
Растояния до аэропорта приобладает в промежутке 15 - 40 км.
Растояние до парка от нескольких метров до 750.
На всех трех графиках видны отдельные пиковые значения. Из этого можно сделать вывод, что большое количество данных взято из одного населенного пункта.

Наблюдается слабая зависимость количества объявлений от растояния до парка. При этом обратная картина для расстояния до центра или аэропорта (чем больше расстояние, тем меньше объявлений).
На графиках видно, что больше всего квартир продается (примерно) за 45 и 60 дней (пиковые значения на первой и второй гистограммах). На 90 днях так же заметен скачек, возможно есть алгоритм закрывающий объявления в этот срок автоматически или запрашивающий подтверждение у владельца. 

Все что продается раньше 40 дней можно назвать быстрой продажей (находится до первого пика). Долгими продажами будем считать все после 100 дней.
К необычно долгим я бы отнес все после 200 дней - третий квартиль, отсечка 3 четвертей(75% значений)

На последней гистограмме мы видим большое количество обьявлений которые, скорее всего, на момент сбора данных были актуальны.

Факторы больше всего влияющие на стоимость квартиры: 

Больше всего на стоимость квартиры влияет её площадь(прямопропорцианальная зависимость). Среднее влияние оказывает жилая площадь и площадь кухни. Количество комнат и тип этажа вляют меньше всего. Дата размещения не оказывает влияния на цену. Стоит обратить внимание на тип этажа, есть существенная разница между первым и последним этажами

По гистограмме видно, что наибольшая цена на расстоянии до 1 км от центра (около 32 млн.р за квартиру) и сильно меняется уже после 1 км. от центра (17 млн.р за квартиру)
Радиус до 1 км от центра - это "самый центр".
Еще одна граница находится в районе 7 км от центра, после которой следует сильное падение цены (с 15 млн до 9 млн) и после начинает убывать постепенно.
Сильный скачек в конце гистграммы связан с одной дорогой квартирой стоимостью 17 122 148 и площадью 178.3.
