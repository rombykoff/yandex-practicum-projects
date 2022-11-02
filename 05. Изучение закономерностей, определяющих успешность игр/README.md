# Изучение закономерностей, определяющих успешность игр

Интернет-магазин «Стримчик» продаёт по всему миру компьютерные игры. Из открытых источников доступны исторические данные о продажах игр, оценки пользователей и экспертов, жанры и платформы (например, Xbox или PlayStation). Нужно выявить определяющие успешность игры закономерности. Это позволит сделать ставку на потенциально популярный продукт и спланировать рекламные кампании.  
Перед нами данные до 2016 года. Представим, что сейчас декабрь 2016 г., и мы планируем кампанию на 2017-й. Нужно отработать принцип работы с данными.  
В наборе данных попадается аббревиатура ESRB (Entertainment Software Rating Board) — это ассоциация, определяющая возрастной рейтинг компьютерных игр. ESRB оценивает игровой контент и присваивает ему подходящую возрастную категорию, например, «Для взрослых», «Для детей младшего возраста» или «Для подростков».

**Цель проекта:**  Выявить определяющие успешность игры закономерности. Это позволит сделать ставку на потенциально популярный продукт и спланировать рекламные кампании.

**Описание данных:**

`Name` — название игры  
`Platform` — платформа  
`Year_of_Release` — год выпуска  
`Genre` — жанр игры  
`NA_sales` — продажи в Северной Америке (миллионы проданных копий)  
`EU_sales` — продажи в Европе (миллионы проданных копий)  
`JP_sales` — продажи в Японии (миллионы проданных копий)  
`Other_sales` — продажи в других странах (миллионы проданных копий)  
`Critic_Score` — оценка критиков (максимум 100)  
`User_Score` — оценка пользователей (максимум 10)  
`Rating` — рейтинг от организации ESRB (англ. Entertainment Software Rating Board). Эта ассоциация определяет рейтинг компьютерных игр и присваивает им подходящую возрастную категорию.

Данные за 2016 год могут быть неполными.

**Ход исследования:**

Шаг 1. Открыть файл с данными и изучить общую информацию  
Шаг 2. Подготовить данные  
Шаг 3. Провести исследовательский анализ данных  
Шаг 4. Составить портрет пользователя каждого региона  
Шаг 5. Проверить гипотезы:
- Средние пользовательские рейтинги платформ Xbox One и PC одинаковые;
- Средние пользовательские рейтинги жанров Action (англ. «действие», экшен-игры) и Sports (англ. «спортивные соревнования») разные.

<a id='intro'></a>

<h1>Содержание<span class="tocSkip"></span></h1>
<div class="toc"><ul class="toc-item"><li><span><a href="#Шаг-1.-Файл-с-данными-и-общая-информация" data-toc-modified-id="Шаг-1.-Файл-с-данными-и-общая-информация-1">Шаг 1. Файл с данными и общая информация</a></span></li><li><span><a href="#Шаг-2.-Подготовка-данных" data-toc-modified-id="Шаг-2.-Подготовка-данных-2">Шаг 2. Подготовка данных</a></span><ul class="toc-item"><li><span><a href="#Замена-названия-столбцов-и-содержимого" data-toc-modified-id="Замена-названия-столбцов-и-содержимого-2.1">Замена названия столбцов и содержимого</a></span></li><li><span><a href="#Преобразование-данных-в-нужные-типы" data-toc-modified-id="Преобразование-данных-в-нужные-типы-2.2">Преобразование данных в нужные типы</a></span></li><li><span><a href="#Обработка-пропусков" data-toc-modified-id="Обработка-пропусков-2.3">Обработка пропусков</a></span></li><li><span><a href="#Рассчет-суммарных-продаж-во-всех-регионах" data-toc-modified-id="Рассчет-суммарных-продаж-во-всех-регионах-2.4">Рассчет суммарных продаж во всех регионах</a></span></li></ul></li><li><span><a href="#Шаг-3.-Исследовательский-анализ-данных" data-toc-modified-id="Шаг-3.-Исследовательский-анализ-данных-3">Шаг 3. Исследовательский анализ данных</a></span><ul class="toc-item"><li><span><a href="#Количество-игр-в-разные-годы" data-toc-modified-id="Количество-игр-в-разные-годы-3.1">Количество игр в разные годы</a></span></li><li><span><a href="#Платформы" data-toc-modified-id="Платформы-3.2">Платформы</a></span></li><li><span><a href="#Актуальный-период-и-потенциально-прибыльные-платформы" data-toc-modified-id="Актуальный-период-и-потенциально-прибыльные-платформы-3.3">Актуальный период и потенциально прибыльные платформы</a></span></li><li><span><a href="#Зависимость-между-объемом-продаж-и-отзывами-пользователей-и-критиков" data-toc-modified-id="Зависимость-между-объемом-продаж-и-отзывами-пользователей-и-критиков-3.4">Зависимость между объемом продаж и отзывами пользователей и критиков</a></span></li><li><span><a href="#Общее-распределение-игр-по-жанрам" data-toc-modified-id="Общее-распределение-игр-по-жанрам-3.5">Общее распределение игр по жанрам</a></span></li><li><span><a href="#Вывод" data-toc-modified-id="Вывод-3.6">Вывод</a></span></li></ul></li><li><span><a href="#Шаг-4.-Портрет-пользователя-каждого-региона" data-toc-modified-id="Шаг-4.-Портрет-пользователя-каждого-региона-4">Шаг 4. Портрет пользователя каждого региона</a></span><ul class="toc-item"><li><span><a href="#Популярность-платформ" data-toc-modified-id="Популярность-платформ-4.1">Популярность платформ</a></span></li><li><span><a href="#Популярные-жанры" data-toc-modified-id="Популярные-жанры-4.2">Популярные жанры</a></span></li><li><span><a href="#Влияние-рейтинга-ESRB-на-продажи-в-отдельных-регионах" data-toc-modified-id="Влияние-рейтинга-ESRB-на-продажи-в-отдельных-регионах-4.3">Влияние рейтинга ESRB на продажи в отдельных регионах</a></span></li><li><span><a href="#Вывод" data-toc-modified-id="Вывод-4.4">Вывод</a></span></li></ul></li><li><span><a href="#Шаг-5.-Проверка-гипотез" data-toc-modified-id="Шаг-5.-Проверка-гипотез-5">Шаг 5. Проверка гипотез</a></span><ul class="toc-item"><li><span><a href="#Гипотеза-1" data-toc-modified-id="Гипотеза-1-5.1">Гипотеза 1</a></span></li><li><span><a href="#Гипотеза-2" data-toc-modified-id="Гипотеза-2-5.2">Гипотеза 2</a></span></li></ul></li><li><span><a href="#Общий-вывод" data-toc-modified-id="Общий-вывод-6">Общий вывод</a></span></li></ul></div>

## Шаг 1. Файл с данными и общая информация

[Содержание](#intro)


```python
import pandas as pd
import matplotlib.pyplot as plt
import seaborn as sns
import numpy as np
from scipy import stats as st
```


```python
try:
    df = pd.read_csv('/datasets/games.csv')
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
      <th>Name</th>
      <th>Platform</th>
      <th>Year_of_Release</th>
      <th>Genre</th>
      <th>NA_sales</th>
      <th>EU_sales</th>
      <th>JP_sales</th>
      <th>Other_sales</th>
      <th>Critic_Score</th>
      <th>User_Score</th>
      <th>Rating</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Wii Sports</td>
      <td>Wii</td>
      <td>2006.0</td>
      <td>Sports</td>
      <td>41.36</td>
      <td>28.96</td>
      <td>3.77</td>
      <td>8.45</td>
      <td>76.0</td>
      <td>8</td>
      <td>E</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Super Mario Bros.</td>
      <td>NES</td>
      <td>1985.0</td>
      <td>Platform</td>
      <td>29.08</td>
      <td>3.58</td>
      <td>6.81</td>
      <td>0.77</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Mario Kart Wii</td>
      <td>Wii</td>
      <td>2008.0</td>
      <td>Racing</td>
      <td>15.68</td>
      <td>12.76</td>
      <td>3.79</td>
      <td>3.29</td>
      <td>82.0</td>
      <td>8.3</td>
      <td>E</td>
    </tr>
    <tr>
      <th>3</th>
      <td>Wii Sports Resort</td>
      <td>Wii</td>
      <td>2009.0</td>
      <td>Sports</td>
      <td>15.61</td>
      <td>10.93</td>
      <td>3.28</td>
      <td>2.95</td>
      <td>80.0</td>
      <td>8</td>
      <td>E</td>
    </tr>
    <tr>
      <th>4</th>
      <td>Pokemon Red/Pokemon Blue</td>
      <td>GB</td>
      <td>1996.0</td>
      <td>Role-Playing</td>
      <td>11.27</td>
      <td>8.89</td>
      <td>10.22</td>
      <td>1.00</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
  </tbody>
</table>
</div>




```python
df.info()
```

    <class 'pandas.core.frame.DataFrame'>
    RangeIndex: 16715 entries, 0 to 16714
    Data columns (total 11 columns):
     #   Column           Non-Null Count  Dtype  
    ---  ------           --------------  -----  
     0   Name             16713 non-null  object 
     1   Platform         16715 non-null  object 
     2   Year_of_Release  16446 non-null  float64
     3   Genre            16713 non-null  object 
     4   NA_sales         16715 non-null  float64
     5   EU_sales         16715 non-null  float64
     6   JP_sales         16715 non-null  float64
     7   Other_sales      16715 non-null  float64
     8   Critic_Score     8137 non-null   float64
     9   User_Score       10014 non-null  object 
     10  Rating           9949 non-null   object 
    dtypes: float64(6), object(5)
    memory usage: 1.4+ MB



```python
df['User_Score'].value_counts()
```




    tbd    2424
    7.8     324
    8       290
    8.2     282
    8.3     254
           ... 
    1.1       2
    0.3       2
    0.9       2
    9.7       1
    0         1
    Name: User_Score, Length: 96, dtype: int64



**Видим столбец `Year_of_Release` с некорректным типом данных, переведем все в *int*. Что делать со столбцом `User_Score` решим позже.**


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
      <th>Year_of_Release</th>
      <th>NA_sales</th>
      <th>EU_sales</th>
      <th>JP_sales</th>
      <th>Other_sales</th>
      <th>Critic_Score</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>count</th>
      <td>16446.000000</td>
      <td>16715.000000</td>
      <td>16715.000000</td>
      <td>16715.000000</td>
      <td>16715.000000</td>
      <td>8137.000000</td>
    </tr>
    <tr>
      <th>mean</th>
      <td>2006.484616</td>
      <td>0.263377</td>
      <td>0.145060</td>
      <td>0.077617</td>
      <td>0.047342</td>
      <td>68.967679</td>
    </tr>
    <tr>
      <th>std</th>
      <td>5.877050</td>
      <td>0.813604</td>
      <td>0.503339</td>
      <td>0.308853</td>
      <td>0.186731</td>
      <td>13.938165</td>
    </tr>
    <tr>
      <th>min</th>
      <td>1980.000000</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>13.000000</td>
    </tr>
    <tr>
      <th>25%</th>
      <td>2003.000000</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>60.000000</td>
    </tr>
    <tr>
      <th>50%</th>
      <td>2007.000000</td>
      <td>0.080000</td>
      <td>0.020000</td>
      <td>0.000000</td>
      <td>0.010000</td>
      <td>71.000000</td>
    </tr>
    <tr>
      <th>75%</th>
      <td>2010.000000</td>
      <td>0.240000</td>
      <td>0.110000</td>
      <td>0.040000</td>
      <td>0.030000</td>
      <td>79.000000</td>
    </tr>
    <tr>
      <th>max</th>
      <td>2016.000000</td>
      <td>41.360000</td>
      <td>28.960000</td>
      <td>10.220000</td>
      <td>10.570000</td>
      <td>98.000000</td>
    </tr>
  </tbody>
</table>
</div>



**Посмотрим количество пропущенных значений**


```python
df.isna().sum()
```




    Name                  2
    Platform              0
    Year_of_Release     269
    Genre                 2
    NA_sales              0
    EU_sales              0
    JP_sales              0
    Other_sales           0
    Critic_Score       8578
    User_Score         6701
    Rating             6766
    dtype: int64



**Названия всех столбцов**


```python
df.columns
```




    Index(['Name', 'Platform', 'Year_of_Release', 'Genre', 'NA_sales', 'EU_sales',
           'JP_sales', 'Other_sales', 'Critic_Score', 'User_Score', 'Rating'],
          dtype='object')



**Проверим наличие дубликатов**


```python
df['Platform'].value_counts()
```




    PS2     2161
    DS      2151
    PS3     1331
    Wii     1320
    X360    1262
    PSP     1209
    PS      1197
    PC       974
    XB       824
    GBA      822
    GC       556
    3DS      520
    PSV      430
    PS4      392
    N64      319
    XOne     247
    SNES     239
    SAT      173
    WiiU     147
    2600     133
    NES       98
    GB        98
    DC        52
    GEN       29
    NG        12
    SCD        6
    WS         6
    3DO        3
    TG16       2
    GG         1
    PCFX       1
    Name: Platform, dtype: int64




```python
df['Genre'].value_counts()
```




    Action          3369
    Sports          2348
    Misc            1750
    Role-Playing    1498
    Shooter         1323
    Adventure       1303
    Racing          1249
    Platform         888
    Simulation       873
    Fighting         849
    Strategy         683
    Puzzle           580
    Name: Genre, dtype: int64



**Количество явных строк-дубликатов:**


```python
df.duplicated().sum()
```




    0



**Вывод:**  
Необходимо поменять тип данных в столбеце `Year of Release`, так как года исчисляются целыми числами.  
Нужно привести к нижнему регистру названия столбцов и их содержание.  
Имеются пропуски значений в столбцах `Year_of_Release`, `Critic_Score`, `User_Score`, `Rating`. Что с ними делать решим в процессе обработки.  
Дубликатов нет.

## Шаг 2. Подготовка данных

### Замена названия столбцов и содержимого

[Содержание](#intro)

Меняем названия столбцов:


```python
df.columns = df.columns.str.lower()
df.columns
```




    Index(['name', 'platform', 'year_of_release', 'genre', 'na_sales', 'eu_sales',
           'jp_sales', 'other_sales', 'critic_score', 'user_score', 'rating'],
          dtype='object')



Приводим к нижнему ресгистру содержание столбцов:


```python
for column in df[['name', 'platform', 'genre', 'rating']]:
    df[column] = df[column].str.lower()
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
      <th>name</th>
      <th>platform</th>
      <th>year_of_release</th>
      <th>genre</th>
      <th>na_sales</th>
      <th>eu_sales</th>
      <th>jp_sales</th>
      <th>other_sales</th>
      <th>critic_score</th>
      <th>user_score</th>
      <th>rating</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>wii sports</td>
      <td>wii</td>
      <td>2006.0</td>
      <td>sports</td>
      <td>41.36</td>
      <td>28.96</td>
      <td>3.77</td>
      <td>8.45</td>
      <td>76.0</td>
      <td>8</td>
      <td>e</td>
    </tr>
    <tr>
      <th>1</th>
      <td>super mario bros.</td>
      <td>nes</td>
      <td>1985.0</td>
      <td>platform</td>
      <td>29.08</td>
      <td>3.58</td>
      <td>6.81</td>
      <td>0.77</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>2</th>
      <td>mario kart wii</td>
      <td>wii</td>
      <td>2008.0</td>
      <td>racing</td>
      <td>15.68</td>
      <td>12.76</td>
      <td>3.79</td>
      <td>3.29</td>
      <td>82.0</td>
      <td>8.3</td>
      <td>e</td>
    </tr>
    <tr>
      <th>3</th>
      <td>wii sports resort</td>
      <td>wii</td>
      <td>2009.0</td>
      <td>sports</td>
      <td>15.61</td>
      <td>10.93</td>
      <td>3.28</td>
      <td>2.95</td>
      <td>80.0</td>
      <td>8</td>
      <td>e</td>
    </tr>
    <tr>
      <th>4</th>
      <td>pokemon red/pokemon blue</td>
      <td>gb</td>
      <td>1996.0</td>
      <td>role-playing</td>
      <td>11.27</td>
      <td>8.89</td>
      <td>10.22</td>
      <td>1.00</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
  </tbody>
</table>
</div>



### Преобразование данных в нужные типы

[Содержание](#intro)

Необходимо поменять тип данных в столбеце `year_of_release`, так как года исчисляются целыми числами.


```python
df['year_of_release'] = df['year_of_release'].astype('Int64')
df.info()
df.head()
```

    <class 'pandas.core.frame.DataFrame'>
    RangeIndex: 16715 entries, 0 to 16714
    Data columns (total 11 columns):
     #   Column           Non-Null Count  Dtype  
    ---  ------           --------------  -----  
     0   name             16713 non-null  object 
     1   platform         16715 non-null  object 
     2   year_of_release  16446 non-null  Int64  
     3   genre            16713 non-null  object 
     4   na_sales         16715 non-null  float64
     5   eu_sales         16715 non-null  float64
     6   jp_sales         16715 non-null  float64
     7   other_sales      16715 non-null  float64
     8   critic_score     8137 non-null   float64
     9   user_score       10014 non-null  object 
     10  rating           9949 non-null   object 
    dtypes: Int64(1), float64(5), object(5)
    memory usage: 1.4+ MB





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
      <th>name</th>
      <th>platform</th>
      <th>year_of_release</th>
      <th>genre</th>
      <th>na_sales</th>
      <th>eu_sales</th>
      <th>jp_sales</th>
      <th>other_sales</th>
      <th>critic_score</th>
      <th>user_score</th>
      <th>rating</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>wii sports</td>
      <td>wii</td>
      <td>2006</td>
      <td>sports</td>
      <td>41.36</td>
      <td>28.96</td>
      <td>3.77</td>
      <td>8.45</td>
      <td>76.0</td>
      <td>8</td>
      <td>e</td>
    </tr>
    <tr>
      <th>1</th>
      <td>super mario bros.</td>
      <td>nes</td>
      <td>1985</td>
      <td>platform</td>
      <td>29.08</td>
      <td>3.58</td>
      <td>6.81</td>
      <td>0.77</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>2</th>
      <td>mario kart wii</td>
      <td>wii</td>
      <td>2008</td>
      <td>racing</td>
      <td>15.68</td>
      <td>12.76</td>
      <td>3.79</td>
      <td>3.29</td>
      <td>82.0</td>
      <td>8.3</td>
      <td>e</td>
    </tr>
    <tr>
      <th>3</th>
      <td>wii sports resort</td>
      <td>wii</td>
      <td>2009</td>
      <td>sports</td>
      <td>15.61</td>
      <td>10.93</td>
      <td>3.28</td>
      <td>2.95</td>
      <td>80.0</td>
      <td>8</td>
      <td>e</td>
    </tr>
    <tr>
      <th>4</th>
      <td>pokemon red/pokemon blue</td>
      <td>gb</td>
      <td>1996</td>
      <td>role-playing</td>
      <td>11.27</td>
      <td>8.89</td>
      <td>10.22</td>
      <td>1.00</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
  </tbody>
</table>
</div>



В столбце `user_score` содержатся оценки, поэтому заменим на float64.  
Но сразу это сделать не получится, так как кроме пропусков и значений содержится значения "tbd".  
Эта аббревиатура обозначает "Будет определено", значит эти значения можно заменить на NaN.


```python
df.loc[df['user_score'] == 'tbd', 'user_score'] = np.nan
df['user_score'] = df['user_score'].astype('float')
df.info()
```

    <class 'pandas.core.frame.DataFrame'>
    RangeIndex: 16715 entries, 0 to 16714
    Data columns (total 11 columns):
     #   Column           Non-Null Count  Dtype  
    ---  ------           --------------  -----  
     0   name             16713 non-null  object 
     1   platform         16715 non-null  object 
     2   year_of_release  16446 non-null  Int64  
     3   genre            16713 non-null  object 
     4   na_sales         16715 non-null  float64
     5   eu_sales         16715 non-null  float64
     6   jp_sales         16715 non-null  float64
     7   other_sales      16715 non-null  float64
     8   critic_score     8137 non-null   float64
     9   user_score       7590 non-null   float64
     10  rating           9949 non-null   object 
    dtypes: Int64(1), float64(6), object(4)
    memory usage: 1.4+ MB


### Обработка пропусков

[Содержание](#intro)

**Столбец `year_of_release`**  
Пропусков мало, посмотрим данные.


```python
display(df.isna().sum())
df[df['year_of_release'].isna()].head(10).sort_values('name')
```


    name                  2
    platform              0
    year_of_release     269
    genre                 2
    na_sales              0
    eu_sales              0
    jp_sales              0
    other_sales           0
    critic_score       8578
    user_score         9125
    rating             6766
    dtype: int64





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
      <th>name</th>
      <th>platform</th>
      <th>year_of_release</th>
      <th>genre</th>
      <th>na_sales</th>
      <th>eu_sales</th>
      <th>jp_sales</th>
      <th>other_sales</th>
      <th>critic_score</th>
      <th>user_score</th>
      <th>rating</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>719</th>
      <td>call of duty 3</td>
      <td>wii</td>
      <td>&lt;NA&gt;</td>
      <td>shooter</td>
      <td>1.17</td>
      <td>0.84</td>
      <td>0.00</td>
      <td>0.23</td>
      <td>69.0</td>
      <td>6.7</td>
      <td>t</td>
    </tr>
    <tr>
      <th>377</th>
      <td>fifa soccer 2004</td>
      <td>ps2</td>
      <td>&lt;NA&gt;</td>
      <td>sports</td>
      <td>0.59</td>
      <td>2.36</td>
      <td>0.04</td>
      <td>0.51</td>
      <td>84.0</td>
      <td>6.4</td>
      <td>e</td>
    </tr>
    <tr>
      <th>657</th>
      <td>frogger's adventures: temple of the frog</td>
      <td>gba</td>
      <td>&lt;NA&gt;</td>
      <td>adventure</td>
      <td>2.15</td>
      <td>0.18</td>
      <td>0.00</td>
      <td>0.07</td>
      <td>73.0</td>
      <td>NaN</td>
      <td>e</td>
    </tr>
    <tr>
      <th>456</th>
      <td>lego batman: the videogame</td>
      <td>wii</td>
      <td>&lt;NA&gt;</td>
      <td>action</td>
      <td>1.80</td>
      <td>0.97</td>
      <td>0.00</td>
      <td>0.29</td>
      <td>74.0</td>
      <td>7.9</td>
      <td>e10+</td>
    </tr>
    <tr>
      <th>678</th>
      <td>lego indiana jones: the original adventures</td>
      <td>wii</td>
      <td>&lt;NA&gt;</td>
      <td>action</td>
      <td>1.51</td>
      <td>0.61</td>
      <td>0.00</td>
      <td>0.21</td>
      <td>78.0</td>
      <td>6.6</td>
      <td>e10+</td>
    </tr>
    <tr>
      <th>183</th>
      <td>madden nfl 2004</td>
      <td>ps2</td>
      <td>&lt;NA&gt;</td>
      <td>sports</td>
      <td>4.26</td>
      <td>0.26</td>
      <td>0.01</td>
      <td>0.71</td>
      <td>94.0</td>
      <td>8.5</td>
      <td>e</td>
    </tr>
    <tr>
      <th>627</th>
      <td>rock band</td>
      <td>x360</td>
      <td>&lt;NA&gt;</td>
      <td>misc</td>
      <td>1.93</td>
      <td>0.33</td>
      <td>0.00</td>
      <td>0.21</td>
      <td>92.0</td>
      <td>8.2</td>
      <td>t</td>
    </tr>
    <tr>
      <th>805</th>
      <td>rock band</td>
      <td>wii</td>
      <td>&lt;NA&gt;</td>
      <td>misc</td>
      <td>1.33</td>
      <td>0.56</td>
      <td>0.00</td>
      <td>0.20</td>
      <td>80.0</td>
      <td>6.3</td>
      <td>t</td>
    </tr>
    <tr>
      <th>609</th>
      <td>space invaders</td>
      <td>2600</td>
      <td>&lt;NA&gt;</td>
      <td>shooter</td>
      <td>2.36</td>
      <td>0.14</td>
      <td>0.00</td>
      <td>0.03</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>475</th>
      <td>wwe smackdown vs. raw 2006</td>
      <td>ps2</td>
      <td>&lt;NA&gt;</td>
      <td>fighting</td>
      <td>1.57</td>
      <td>1.02</td>
      <td>0.00</td>
      <td>0.41</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
  </tbody>
</table>
</div>




```python
df[df['name'] == 'call of duty 3']
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
      <th>name</th>
      <th>platform</th>
      <th>year_of_release</th>
      <th>genre</th>
      <th>na_sales</th>
      <th>eu_sales</th>
      <th>jp_sales</th>
      <th>other_sales</th>
      <th>critic_score</th>
      <th>user_score</th>
      <th>rating</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>557</th>
      <td>call of duty 3</td>
      <td>x360</td>
      <td>2006</td>
      <td>shooter</td>
      <td>1.49</td>
      <td>0.92</td>
      <td>0.02</td>
      <td>0.27</td>
      <td>82.0</td>
      <td>6.5</td>
      <td>t</td>
    </tr>
    <tr>
      <th>719</th>
      <td>call of duty 3</td>
      <td>wii</td>
      <td>&lt;NA&gt;</td>
      <td>shooter</td>
      <td>1.17</td>
      <td>0.84</td>
      <td>0.00</td>
      <td>0.23</td>
      <td>69.0</td>
      <td>6.7</td>
      <td>t</td>
    </tr>
    <tr>
      <th>1255</th>
      <td>call of duty 3</td>
      <td>ps3</td>
      <td>2006</td>
      <td>shooter</td>
      <td>0.60</td>
      <td>0.62</td>
      <td>0.03</td>
      <td>0.26</td>
      <td>80.0</td>
      <td>6.9</td>
      <td>t</td>
    </tr>
    <tr>
      <th>1915</th>
      <td>call of duty 3</td>
      <td>ps2</td>
      <td>2006</td>
      <td>shooter</td>
      <td>0.89</td>
      <td>0.03</td>
      <td>0.00</td>
      <td>0.15</td>
      <td>82.0</td>
      <td>7.4</td>
      <td>t</td>
    </tr>
  </tbody>
</table>
</div>




```python
df[df['name'] == 'lego batman: the videogame']
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
      <th>name</th>
      <th>platform</th>
      <th>year_of_release</th>
      <th>genre</th>
      <th>na_sales</th>
      <th>eu_sales</th>
      <th>jp_sales</th>
      <th>other_sales</th>
      <th>critic_score</th>
      <th>user_score</th>
      <th>rating</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>397</th>
      <td>lego batman: the videogame</td>
      <td>x360</td>
      <td>2008</td>
      <td>action</td>
      <td>2.04</td>
      <td>1.02</td>
      <td>0.0</td>
      <td>0.32</td>
      <td>76.0</td>
      <td>7.9</td>
      <td>e10+</td>
    </tr>
    <tr>
      <th>456</th>
      <td>lego batman: the videogame</td>
      <td>wii</td>
      <td>&lt;NA&gt;</td>
      <td>action</td>
      <td>1.80</td>
      <td>0.97</td>
      <td>0.0</td>
      <td>0.29</td>
      <td>74.0</td>
      <td>7.9</td>
      <td>e10+</td>
    </tr>
    <tr>
      <th>460</th>
      <td>lego batman: the videogame</td>
      <td>ds</td>
      <td>2008</td>
      <td>action</td>
      <td>1.75</td>
      <td>1.01</td>
      <td>0.0</td>
      <td>0.29</td>
      <td>72.0</td>
      <td>8.0</td>
      <td>e10+</td>
    </tr>
    <tr>
      <th>1519</th>
      <td>lego batman: the videogame</td>
      <td>ps3</td>
      <td>2008</td>
      <td>action</td>
      <td>0.72</td>
      <td>0.39</td>
      <td>0.0</td>
      <td>0.19</td>
      <td>75.0</td>
      <td>7.7</td>
      <td>e10+</td>
    </tr>
    <tr>
      <th>1538</th>
      <td>lego batman: the videogame</td>
      <td>psp</td>
      <td>&lt;NA&gt;</td>
      <td>action</td>
      <td>0.57</td>
      <td>0.44</td>
      <td>0.0</td>
      <td>0.27</td>
      <td>73.0</td>
      <td>7.4</td>
      <td>e10+</td>
    </tr>
    <tr>
      <th>1553</th>
      <td>lego batman: the videogame</td>
      <td>ps2</td>
      <td>2008</td>
      <td>action</td>
      <td>0.72</td>
      <td>0.03</td>
      <td>0.0</td>
      <td>0.52</td>
      <td>77.0</td>
      <td>8.9</td>
      <td>e10+</td>
    </tr>
    <tr>
      <th>12465</th>
      <td>lego batman: the videogame</td>
      <td>pc</td>
      <td>2008</td>
      <td>action</td>
      <td>0.02</td>
      <td>0.03</td>
      <td>0.0</td>
      <td>0.01</td>
      <td>80.0</td>
      <td>7.8</td>
      <td>e10+</td>
    </tr>
  </tbody>
</table>
</div>



По таблицам видно, что пропуски можно заполнить по совпадающим названиям игр.  
Создадим переменную с играми без пропусков, создадим функцию и заменим пропуски.


```python
loc_year = df.groupby('name')['year_of_release'].max().dropna()
display(loc_year.head(10))

def year_rel(name):
    if name in loc_year:
        return loc_year[name]
```


    name
     beyblade burst                            2016
     fire emblem fates                         2015
     frozen: olaf's quest                      2013
     haikyu!! cross team match!                2016
     tales of xillia 2                         2012
    '98 koshien                                1998
    .hack//g.u. vol.1//rebirth                 2006
    .hack//g.u. vol.2//reminisce               2006
    .hack//g.u. vol.2//reminisce (jp sales)    2006
    .hack//g.u. vol.3//redemption              2007
    Name: year_of_release, dtype: Int64



```python
df.loc[df['year_of_release'].isna(), 'year_of_release'] = df['name'].apply(year_rel)
```


```python
print('Пропусков осталось:', df['year_of_release'].isna().sum())
```

    Пропусков осталось: 146



```python
#проверим замену
df[df['name'] == 'call of duty 3']
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
      <th>name</th>
      <th>platform</th>
      <th>year_of_release</th>
      <th>genre</th>
      <th>na_sales</th>
      <th>eu_sales</th>
      <th>jp_sales</th>
      <th>other_sales</th>
      <th>critic_score</th>
      <th>user_score</th>
      <th>rating</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>557</th>
      <td>call of duty 3</td>
      <td>x360</td>
      <td>2006</td>
      <td>shooter</td>
      <td>1.49</td>
      <td>0.92</td>
      <td>0.02</td>
      <td>0.27</td>
      <td>82.0</td>
      <td>6.5</td>
      <td>t</td>
    </tr>
    <tr>
      <th>719</th>
      <td>call of duty 3</td>
      <td>wii</td>
      <td>2006</td>
      <td>shooter</td>
      <td>1.17</td>
      <td>0.84</td>
      <td>0.00</td>
      <td>0.23</td>
      <td>69.0</td>
      <td>6.7</td>
      <td>t</td>
    </tr>
    <tr>
      <th>1255</th>
      <td>call of duty 3</td>
      <td>ps3</td>
      <td>2006</td>
      <td>shooter</td>
      <td>0.60</td>
      <td>0.62</td>
      <td>0.03</td>
      <td>0.26</td>
      <td>80.0</td>
      <td>6.9</td>
      <td>t</td>
    </tr>
    <tr>
      <th>1915</th>
      <td>call of duty 3</td>
      <td>ps2</td>
      <td>2006</td>
      <td>shooter</td>
      <td>0.89</td>
      <td>0.03</td>
      <td>0.00</td>
      <td>0.15</td>
      <td>82.0</td>
      <td>7.4</td>
      <td>t</td>
    </tr>
  </tbody>
</table>
</div>



Посмотрим, что осталось


```python
df[df['year_of_release'].isna()].head(10).sort_values('name')
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
      <th>name</th>
      <th>platform</th>
      <th>year_of_release</th>
      <th>genre</th>
      <th>na_sales</th>
      <th>eu_sales</th>
      <th>jp_sales</th>
      <th>other_sales</th>
      <th>critic_score</th>
      <th>user_score</th>
      <th>rating</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>1506</th>
      <td>adventure</td>
      <td>2600</td>
      <td>&lt;NA&gt;</td>
      <td>adventure</td>
      <td>1.21</td>
      <td>0.08</td>
      <td>0.0</td>
      <td>0.01</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>1585</th>
      <td>combat</td>
      <td>2600</td>
      <td>&lt;NA&gt;</td>
      <td>action</td>
      <td>1.17</td>
      <td>0.07</td>
      <td>0.0</td>
      <td>0.01</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>657</th>
      <td>frogger's adventures: temple of the frog</td>
      <td>gba</td>
      <td>&lt;NA&gt;</td>
      <td>adventure</td>
      <td>2.15</td>
      <td>0.18</td>
      <td>0.0</td>
      <td>0.07</td>
      <td>73.0</td>
      <td>NaN</td>
      <td>e</td>
    </tr>
    <tr>
      <th>1984</th>
      <td>legacy of kain: soul reaver</td>
      <td>ps</td>
      <td>&lt;NA&gt;</td>
      <td>action</td>
      <td>0.58</td>
      <td>0.40</td>
      <td>0.0</td>
      <td>0.07</td>
      <td>91.0</td>
      <td>9.0</td>
      <td>t</td>
    </tr>
    <tr>
      <th>627</th>
      <td>rock band</td>
      <td>x360</td>
      <td>&lt;NA&gt;</td>
      <td>misc</td>
      <td>1.93</td>
      <td>0.33</td>
      <td>0.0</td>
      <td>0.21</td>
      <td>92.0</td>
      <td>8.2</td>
      <td>t</td>
    </tr>
    <tr>
      <th>805</th>
      <td>rock band</td>
      <td>wii</td>
      <td>&lt;NA&gt;</td>
      <td>misc</td>
      <td>1.33</td>
      <td>0.56</td>
      <td>0.0</td>
      <td>0.20</td>
      <td>80.0</td>
      <td>6.3</td>
      <td>t</td>
    </tr>
    <tr>
      <th>1142</th>
      <td>rock band</td>
      <td>ps3</td>
      <td>&lt;NA&gt;</td>
      <td>misc</td>
      <td>0.99</td>
      <td>0.41</td>
      <td>0.0</td>
      <td>0.22</td>
      <td>92.0</td>
      <td>8.4</td>
      <td>t</td>
    </tr>
    <tr>
      <th>1840</th>
      <td>rock band</td>
      <td>ps2</td>
      <td>&lt;NA&gt;</td>
      <td>misc</td>
      <td>0.71</td>
      <td>0.06</td>
      <td>0.0</td>
      <td>0.35</td>
      <td>82.0</td>
      <td>6.8</td>
      <td>t</td>
    </tr>
    <tr>
      <th>1301</th>
      <td>triple play 99</td>
      <td>ps</td>
      <td>&lt;NA&gt;</td>
      <td>sports</td>
      <td>0.81</td>
      <td>0.55</td>
      <td>0.0</td>
      <td>0.10</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>475</th>
      <td>wwe smackdown vs. raw 2006</td>
      <td>ps2</td>
      <td>&lt;NA&gt;</td>
      <td>fighting</td>
      <td>1.57</td>
      <td>1.02</td>
      <td>0.0</td>
      <td>0.41</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
  </tbody>
</table>
</div>




```python
display(df.loc[df['name'] == 'rock band'])
display(df.loc[df['name'] == 'adventure'])
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
      <th>name</th>
      <th>platform</th>
      <th>year_of_release</th>
      <th>genre</th>
      <th>na_sales</th>
      <th>eu_sales</th>
      <th>jp_sales</th>
      <th>other_sales</th>
      <th>critic_score</th>
      <th>user_score</th>
      <th>rating</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>627</th>
      <td>rock band</td>
      <td>x360</td>
      <td>&lt;NA&gt;</td>
      <td>misc</td>
      <td>1.93</td>
      <td>0.33</td>
      <td>0.0</td>
      <td>0.21</td>
      <td>92.0</td>
      <td>8.2</td>
      <td>t</td>
    </tr>
    <tr>
      <th>805</th>
      <td>rock band</td>
      <td>wii</td>
      <td>&lt;NA&gt;</td>
      <td>misc</td>
      <td>1.33</td>
      <td>0.56</td>
      <td>0.0</td>
      <td>0.20</td>
      <td>80.0</td>
      <td>6.3</td>
      <td>t</td>
    </tr>
    <tr>
      <th>1142</th>
      <td>rock band</td>
      <td>ps3</td>
      <td>&lt;NA&gt;</td>
      <td>misc</td>
      <td>0.99</td>
      <td>0.41</td>
      <td>0.0</td>
      <td>0.22</td>
      <td>92.0</td>
      <td>8.4</td>
      <td>t</td>
    </tr>
    <tr>
      <th>1840</th>
      <td>rock band</td>
      <td>ps2</td>
      <td>&lt;NA&gt;</td>
      <td>misc</td>
      <td>0.71</td>
      <td>0.06</td>
      <td>0.0</td>
      <td>0.35</td>
      <td>82.0</td>
      <td>6.8</td>
      <td>t</td>
    </tr>
  </tbody>
</table>
</div>



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
      <th>name</th>
      <th>platform</th>
      <th>year_of_release</th>
      <th>genre</th>
      <th>na_sales</th>
      <th>eu_sales</th>
      <th>jp_sales</th>
      <th>other_sales</th>
      <th>critic_score</th>
      <th>user_score</th>
      <th>rating</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>1506</th>
      <td>adventure</td>
      <td>2600</td>
      <td>&lt;NA&gt;</td>
      <td>adventure</td>
      <td>1.21</td>
      <td>0.08</td>
      <td>0.0</td>
      <td>0.01</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
  </tbody>
</table>
</div>


Остальные пропуски заполнить нет возможности. Пропусков в годе выпуска = 146, они принадлежат старым платформам. Так как их количество минимально, то их можно удалить.

**Столбцы `name` и `genre`**

Всего по 2 пропуска - оценим их визуально.


```python
df = df.dropna(subset=['year_of_release'])
df.isna().sum()
```




    name                  2
    platform              0
    year_of_release       0
    genre                 2
    na_sales              0
    eu_sales              0
    jp_sales              0
    other_sales           0
    critic_score       8494
    user_score         9029
    rating             6701
    dtype: int64




```python
display(df[df['name'].isna()])
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
      <th>name</th>
      <th>platform</th>
      <th>year_of_release</th>
      <th>genre</th>
      <th>na_sales</th>
      <th>eu_sales</th>
      <th>jp_sales</th>
      <th>other_sales</th>
      <th>critic_score</th>
      <th>user_score</th>
      <th>rating</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>659</th>
      <td>NaN</td>
      <td>gen</td>
      <td>1993</td>
      <td>NaN</td>
      <td>1.78</td>
      <td>0.53</td>
      <td>0.00</td>
      <td>0.08</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>14244</th>
      <td>NaN</td>
      <td>gen</td>
      <td>1993</td>
      <td>NaN</td>
      <td>0.00</td>
      <td>0.00</td>
      <td>0.03</td>
      <td>0.00</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
  </tbody>
</table>
</div>


Две игры с большим количеством пропусков. Для заполнения пропущенных значений, данных очень мало - удалим их.


```python
df = df.dropna(subset=['name'])
df.isna().sum()
```




    name                  0
    platform              0
    year_of_release       0
    genre                 0
    na_sales              0
    eu_sales              0
    jp_sales              0
    other_sales           0
    critic_score       8492
    user_score         9027
    rating             6699
    dtype: int64



**Столбец `rating`**

Посмотрим на категории в этом столбце


```python
display(df['rating'].value_counts())
```


    e       3958
    t       2930
    m       1554
    e10+    1412
    ec         8
    k-a        3
    rp         2
    ao         1
    Name: rating, dtype: int64


Расшифровка категорий:  
«EC» («Early childhood») — «Для детей младшего возраста»  
«E» («Everyone») — «Для всех»  
«K-A» («Kids to Adults»)— «Для детей и взрослых»  
«E10+» («Everyone 10 and older») — «Для всех от 10 лет и старше»  
«T» («Teen») — «Подросткам»  
«M» («Mature») — «Для взрослых»  
«AO» («Adults Only 18+») — «Только для взрослых»  
«RP» («Rating Pending») — «Рейтинг ожидается»  

Так как значний в `ec`, `k-a`, `rp` и `ao` очень мало, сократим количество категорий.  
Объединим:  
`ec` - `e`  
`k-a` - `e`  
`ao` - `m`  
`rp` - значит «Рейтинг ожидается», заменим на NaN


```python
df['rating'] = df['rating'].replace('ec', 'e')
df['rating'] = df['rating'].replace('k-a', 'e')
df['rating'] = df['rating'].replace('ao', 'm')
df.loc[df['rating'] == 'rp', 'rating'] = np.nan
display(df['rating'].value_counts())
```


    e       3969
    t       2930
    m       1555
    e10+    1412
    Name: rating, dtype: int64


Визуально оценим пропуски


```python
display(df[df['rating'].isna()])
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
      <th>name</th>
      <th>platform</th>
      <th>year_of_release</th>
      <th>genre</th>
      <th>na_sales</th>
      <th>eu_sales</th>
      <th>jp_sales</th>
      <th>other_sales</th>
      <th>critic_score</th>
      <th>user_score</th>
      <th>rating</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>1</th>
      <td>super mario bros.</td>
      <td>nes</td>
      <td>1985</td>
      <td>platform</td>
      <td>29.08</td>
      <td>3.58</td>
      <td>6.81</td>
      <td>0.77</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>4</th>
      <td>pokemon red/pokemon blue</td>
      <td>gb</td>
      <td>1996</td>
      <td>role-playing</td>
      <td>11.27</td>
      <td>8.89</td>
      <td>10.22</td>
      <td>1.00</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>5</th>
      <td>tetris</td>
      <td>gb</td>
      <td>1989</td>
      <td>puzzle</td>
      <td>23.20</td>
      <td>2.26</td>
      <td>4.22</td>
      <td>0.58</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>9</th>
      <td>duck hunt</td>
      <td>nes</td>
      <td>1984</td>
      <td>shooter</td>
      <td>26.93</td>
      <td>0.63</td>
      <td>0.28</td>
      <td>0.47</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>10</th>
      <td>nintendogs</td>
      <td>ds</td>
      <td>2005</td>
      <td>simulation</td>
      <td>9.05</td>
      <td>10.95</td>
      <td>1.93</td>
      <td>2.74</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>16710</th>
      <td>samurai warriors: sanada maru</td>
      <td>ps3</td>
      <td>2016</td>
      <td>action</td>
      <td>0.00</td>
      <td>0.00</td>
      <td>0.01</td>
      <td>0.00</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>16711</th>
      <td>lma manager 2007</td>
      <td>x360</td>
      <td>2006</td>
      <td>sports</td>
      <td>0.00</td>
      <td>0.01</td>
      <td>0.00</td>
      <td>0.00</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>16712</th>
      <td>haitaka no psychedelica</td>
      <td>psv</td>
      <td>2016</td>
      <td>adventure</td>
      <td>0.00</td>
      <td>0.00</td>
      <td>0.01</td>
      <td>0.00</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>16713</th>
      <td>spirits &amp; spells</td>
      <td>gba</td>
      <td>2003</td>
      <td>platform</td>
      <td>0.01</td>
      <td>0.00</td>
      <td>0.00</td>
      <td>0.00</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>16714</th>
      <td>winning post 8 2016</td>
      <td>psv</td>
      <td>2016</td>
      <td>simulation</td>
      <td>0.00</td>
      <td>0.00</td>
      <td>0.01</td>
      <td>0.00</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
  </tbody>
</table>
<p>6701 rows × 11 columns</p>
</div>



```python
display(df[df['name'] == 'tetris'])
display(df[df['name'] == 'nintendogs'])
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
      <th>name</th>
      <th>platform</th>
      <th>year_of_release</th>
      <th>genre</th>
      <th>na_sales</th>
      <th>eu_sales</th>
      <th>jp_sales</th>
      <th>other_sales</th>
      <th>critic_score</th>
      <th>user_score</th>
      <th>rating</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>5</th>
      <td>tetris</td>
      <td>gb</td>
      <td>1989</td>
      <td>puzzle</td>
      <td>23.20</td>
      <td>2.26</td>
      <td>4.22</td>
      <td>0.58</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>157</th>
      <td>tetris</td>
      <td>nes</td>
      <td>1988</td>
      <td>puzzle</td>
      <td>2.97</td>
      <td>0.69</td>
      <td>1.81</td>
      <td>0.11</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
  </tbody>
</table>
</div>



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
      <th>name</th>
      <th>platform</th>
      <th>year_of_release</th>
      <th>genre</th>
      <th>na_sales</th>
      <th>eu_sales</th>
      <th>jp_sales</th>
      <th>other_sales</th>
      <th>critic_score</th>
      <th>user_score</th>
      <th>rating</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>10</th>
      <td>nintendogs</td>
      <td>ds</td>
      <td>2005</td>
      <td>simulation</td>
      <td>9.05</td>
      <td>10.95</td>
      <td>1.93</td>
      <td>2.74</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
  </tbody>
</table>
</div>


Заменим пропуски по совпадающим названиям игр, для всех остальных заменим на маркер 'undef'.


```python
df_drop_rating = df[['name', 'rating']].dropna(subset=['rating']).drop_duplicates(subset=['name'])
display(df_drop_rating.head())

display(df_drop_rating[df_drop_rating[['name', 'rating']].duplicated(keep=False)].head())
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
      <th>name</th>
      <th>rating</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>wii sports</td>
      <td>e</td>
    </tr>
    <tr>
      <th>2</th>
      <td>mario kart wii</td>
      <td>e</td>
    </tr>
    <tr>
      <th>3</th>
      <td>wii sports resort</td>
      <td>e</td>
    </tr>
    <tr>
      <th>6</th>
      <td>new super mario bros.</td>
      <td>e</td>
    </tr>
    <tr>
      <th>7</th>
      <td>wii play</td>
      <td>e</td>
    </tr>
  </tbody>
</table>
</div>



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
      <th>name</th>
      <th>rating</th>
    </tr>
  </thead>
  <tbody>
  </tbody>
</table>
</div>



```python
def rating_game(row):
    name = row['name']
    if name in df_drop_rating['name']:
        return df_drop_rating['rating']
    return 'undef'


df.loc[df['rating'].isna(), 'rating'] = df.loc[df['rating'].isna()].apply(rating_game, axis = 1)

display(df.isna().sum())
display(df.head())
```


    name                  0
    platform              0
    year_of_release       0
    genre                 0
    na_sales              0
    eu_sales              0
    jp_sales              0
    other_sales           0
    critic_score       8492
    user_score         9027
    rating                0
    dtype: int64



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
      <th>name</th>
      <th>platform</th>
      <th>year_of_release</th>
      <th>genre</th>
      <th>na_sales</th>
      <th>eu_sales</th>
      <th>jp_sales</th>
      <th>other_sales</th>
      <th>critic_score</th>
      <th>user_score</th>
      <th>rating</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>wii sports</td>
      <td>wii</td>
      <td>2006</td>
      <td>sports</td>
      <td>41.36</td>
      <td>28.96</td>
      <td>3.77</td>
      <td>8.45</td>
      <td>76.0</td>
      <td>8.0</td>
      <td>e</td>
    </tr>
    <tr>
      <th>1</th>
      <td>super mario bros.</td>
      <td>nes</td>
      <td>1985</td>
      <td>platform</td>
      <td>29.08</td>
      <td>3.58</td>
      <td>6.81</td>
      <td>0.77</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>undef</td>
    </tr>
    <tr>
      <th>2</th>
      <td>mario kart wii</td>
      <td>wii</td>
      <td>2008</td>
      <td>racing</td>
      <td>15.68</td>
      <td>12.76</td>
      <td>3.79</td>
      <td>3.29</td>
      <td>82.0</td>
      <td>8.3</td>
      <td>e</td>
    </tr>
    <tr>
      <th>3</th>
      <td>wii sports resort</td>
      <td>wii</td>
      <td>2009</td>
      <td>sports</td>
      <td>15.61</td>
      <td>10.93</td>
      <td>3.28</td>
      <td>2.95</td>
      <td>80.0</td>
      <td>8.0</td>
      <td>e</td>
    </tr>
    <tr>
      <th>4</th>
      <td>pokemon red/pokemon blue</td>
      <td>gb</td>
      <td>1996</td>
      <td>role-playing</td>
      <td>11.27</td>
      <td>8.89</td>
      <td>10.22</td>
      <td>1.00</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>undef</td>
    </tr>
  </tbody>
</table>
</div>


`rating`  
Пропуски заменены.  
`critic_score`	`user_score`  
Отсутсвие значений в этих столбцах скорее всего с тем, что оценки у этой игры просто нет. 

### Рассчет суммарных продаж во всех регионах 

[Содержание](#intro)


```python
df['total_sales'] = df[['na_sales', 'eu_sales', 'jp_sales', 'other_sales']].sum(axis = 1)
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
      <th>name</th>
      <th>platform</th>
      <th>year_of_release</th>
      <th>genre</th>
      <th>na_sales</th>
      <th>eu_sales</th>
      <th>jp_sales</th>
      <th>other_sales</th>
      <th>critic_score</th>
      <th>user_score</th>
      <th>rating</th>
      <th>total_sales</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>wii sports</td>
      <td>wii</td>
      <td>2006</td>
      <td>sports</td>
      <td>41.36</td>
      <td>28.96</td>
      <td>3.77</td>
      <td>8.45</td>
      <td>76.0</td>
      <td>8.0</td>
      <td>e</td>
      <td>82.54</td>
    </tr>
    <tr>
      <th>1</th>
      <td>super mario bros.</td>
      <td>nes</td>
      <td>1985</td>
      <td>platform</td>
      <td>29.08</td>
      <td>3.58</td>
      <td>6.81</td>
      <td>0.77</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>undef</td>
      <td>40.24</td>
    </tr>
    <tr>
      <th>2</th>
      <td>mario kart wii</td>
      <td>wii</td>
      <td>2008</td>
      <td>racing</td>
      <td>15.68</td>
      <td>12.76</td>
      <td>3.79</td>
      <td>3.29</td>
      <td>82.0</td>
      <td>8.3</td>
      <td>e</td>
      <td>35.52</td>
    </tr>
    <tr>
      <th>3</th>
      <td>wii sports resort</td>
      <td>wii</td>
      <td>2009</td>
      <td>sports</td>
      <td>15.61</td>
      <td>10.93</td>
      <td>3.28</td>
      <td>2.95</td>
      <td>80.0</td>
      <td>8.0</td>
      <td>e</td>
      <td>32.77</td>
    </tr>
    <tr>
      <th>4</th>
      <td>pokemon red/pokemon blue</td>
      <td>gb</td>
      <td>1996</td>
      <td>role-playing</td>
      <td>11.27</td>
      <td>8.89</td>
      <td>10.22</td>
      <td>1.00</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>undef</td>
      <td>31.38</td>
    </tr>
  </tbody>
</table>
</div>



**Вывод:**  
Все названия столбцов и значения приведены к нижнему регистру.  
В столбцах `year_of_release` и `user_score` заменены типы данных.  
В `year_of_release` пропуски заполнены по совпадающим названиям игр.  
Удалено 148 строк.  
`rating` - Пропуски заменены.
`critic_score` `user_score` - отсутсвие значений в этих столбцах скорее всего с тем, что оценки у этой игры просто нет.  
Отсутсвие значений в этих столбцах скорее всего с тем, что оценки или рейтинга у этой игры просто нет.


```python
# check
df.info()
```

    <class 'pandas.core.frame.DataFrame'>
    Int64Index: 16567 entries, 0 to 16714
    Data columns (total 12 columns):
     #   Column           Non-Null Count  Dtype  
    ---  ------           --------------  -----  
     0   name             16567 non-null  object 
     1   platform         16567 non-null  object 
     2   year_of_release  16567 non-null  Int64  
     3   genre            16567 non-null  object 
     4   na_sales         16567 non-null  float64
     5   eu_sales         16567 non-null  float64
     6   jp_sales         16567 non-null  float64
     7   other_sales      16567 non-null  float64
     8   critic_score     8075 non-null   float64
     9   user_score       7540 non-null   float64
     10  rating           16567 non-null  object 
     11  total_sales      16567 non-null  float64
    dtypes: Int64(1), float64(7), object(4)
    memory usage: 1.7+ MB


## Шаг 3. Исследовательский анализ данных

[Содержание](#intro)

### Количество игр в разные годы

**Посмотрим сколько выпускалось игр в разные годы:**


```python
year_release = df.pivot_table(index='year_of_release', values='name', aggfunc='count')
plt.figure(figsize=(15, 5))
sns.lineplot(data=year_release, legend=False)
plt.title('Количество игр выпускаемых в разные годы')
plt.xlabel('Год')
plt.ylabel('Количество выпущенных игр')
plt.grid()
plt.show()
```


    
![png](output_70_0.png)
    


На графике видно, что быстрый рост выпуска игр начинается с 1990. Пик на консольные и компьютерный игры приходится на 2008-2009 года, далее начинается спад.  
Уменьшение количества игр на консоли и на ПК может быть связан с увелечением времени разработки игр (с каждым годом графика и функционал улучшается, геймеры становятся более требовательны), так же в это время начинает расширяться рынок мобильных игр (выходит Айфон, увеличиваются экраны телефонов и их мощность).

### Платформы

[Содержание](#intro)

**Далее посмотрим, как менялись продажи по платформам. Выберем платформы с наибольшими суммарными продажами и построим распределение по годам. Посмотрим за какой характерный срок появляются новые и исчезают старые платформы.**


```python
platform_release = df.pivot_table(index='platform', values='total_sales', aggfunc='sum').sort_values(by='total_sales', ascending=False)
plt.figure(figsize=(15, 5))
sns.barplot(x=platform_release.index, y=platform_release['total_sales'])
plt.title('Продажи по платформам')
plt.xlabel('Название платформы')
plt.ylabel('Продажи(миллионы копий)')
plt.show()
```


    
![png](output_74_0.png)
    


Из графика видно, что самые популярные игровые платформы: PS, DS, WII, PS3, X360, PS2.  



```python
platform_pivot_years = df.pivot_table(index='year_of_release', columns='platform', values = 'total_sales', aggfunc = 'sum')
platform_pivot_years.head()
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
      <th>platform</th>
      <th>2600</th>
      <th>3do</th>
      <th>3ds</th>
      <th>dc</th>
      <th>ds</th>
      <th>gb</th>
      <th>gba</th>
      <th>gc</th>
      <th>gen</th>
      <th>gg</th>
      <th>...</th>
      <th>sat</th>
      <th>scd</th>
      <th>snes</th>
      <th>tg16</th>
      <th>wii</th>
      <th>wiiu</th>
      <th>ws</th>
      <th>x360</th>
      <th>xb</th>
      <th>xone</th>
    </tr>
    <tr>
      <th>year_of_release</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>1980</th>
      <td>11.38</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
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
      <th>1981</th>
      <td>35.68</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
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
      <th>1982</th>
      <td>28.88</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
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
      <th>1983</th>
      <td>5.84</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
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
      <th>1984</th>
      <td>0.27</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
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
<p>5 rows × 31 columns</p>
</div>




```python
platform_pivot_years.plot(figsize=(15,5))
plt.show()
```


    
![png](output_77_0.png)
    



```python
plt.figure(figsize=(20, 15))
sns.heatmap(platform_pivot_years, annot=True, cmap="Blues", fmt='.1f')
```




    <AxesSubplot:xlabel='platform', ylabel='year_of_release'>




    
![png](output_78_1.png)
    


На графике видны странные выбросы у приставок 2600, dc, ds и pc.  
Согласно Wikipedia поддержка платформ была:  
для `2600` с 1977 по 1992, удалим все после 1992  
для `ds` с 2004 по 2013, удалим все что не входит в этот промежуток  
для `pc` выход игр начался еще раньше 1985 года, оставим все как есть  
для `dc` все сложнее, она аппаратный клон приставки третьего поколения Famicom японской компании Nintendo, продавалась в основном в России с 1992 по 1998. Поддержка оригинальной приставки закончилась в 2003. Удалим все, что после 2003.


```python
drop_2600 = df.query('(platform == "2600") and (year_of_release > 1992)').index
display(drop_2600)
drop_ds = df.query('(platform == "ds") and (2004 > year_of_release)').index
display(drop_ds)
drop_dc = df.query('(platform == "dc") and (year_of_release >= 2003)').index
display(drop_dc)
```


    Int64Index([609], dtype='int64')



    Int64Index([15957], dtype='int64')



    Int64Index([14006, 15997], dtype='int64')



```python
df = df.drop([609, 15957, 14006, 15997], axis=0)
```


```python
platform_pivot_years = df.pivot_table(index='year_of_release', columns='platform', values = 'total_sales', aggfunc = 'sum')
plt.figure(figsize=(20, 15))
sns.heatmap(platform_pivot_years, annot=True, cmap="Blues", fmt='.1f')
```




    <AxesSubplot:xlabel='platform', ylabel='year_of_release'>




    
![png](output_82_1.png)
    



```python
platform_totalsales = df.pivot_table(index='platform', values='total_sales', aggfunc='sum').nlargest(6, 'total_sales').index
df.query('platform in @platform_totalsales').pivot_table(index = 'year_of_release', columns = 'platform', values= 'total_sales', aggfunc = 'sum').plot(figsize = (15,7))
plt.grid()
plt.show()
```


    
![png](output_83_0.png)
    


Практически все игровые платформы в среднем существуют около 10 лет.  
Самый пик наступает примерно в середине жизни приставки.  
PC самая долгоживущая платформа.

### Актуальный период и потенциально прибыльные платформы

[Содержание](#intro)

**Возьмем данные за актуальный период**

Актуальные период для прогноза на 2017 возьмем с 2014 по 2016 года. Данный период выбран в связи с тем, что уже остались только актуальные платформы и за 2016 год данные неполные. Если сделать период меньше, то часть данных будет урезана.


```python
actual_df = df.query('2014 <= year_of_release <= 2016') 
actual_df.head()
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
      <th>name</th>
      <th>platform</th>
      <th>year_of_release</th>
      <th>genre</th>
      <th>na_sales</th>
      <th>eu_sales</th>
      <th>jp_sales</th>
      <th>other_sales</th>
      <th>critic_score</th>
      <th>user_score</th>
      <th>rating</th>
      <th>total_sales</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>31</th>
      <td>call of duty: black ops 3</td>
      <td>ps4</td>
      <td>2015</td>
      <td>shooter</td>
      <td>6.03</td>
      <td>5.86</td>
      <td>0.36</td>
      <td>2.38</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>undef</td>
      <td>14.63</td>
    </tr>
    <tr>
      <th>42</th>
      <td>grand theft auto v</td>
      <td>ps4</td>
      <td>2014</td>
      <td>action</td>
      <td>3.96</td>
      <td>6.31</td>
      <td>0.38</td>
      <td>1.97</td>
      <td>97.0</td>
      <td>8.3</td>
      <td>m</td>
      <td>12.62</td>
    </tr>
    <tr>
      <th>47</th>
      <td>pokemon omega ruby/pokemon alpha sapphire</td>
      <td>3ds</td>
      <td>2014</td>
      <td>role-playing</td>
      <td>4.35</td>
      <td>3.49</td>
      <td>3.10</td>
      <td>0.74</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>undef</td>
      <td>11.68</td>
    </tr>
    <tr>
      <th>77</th>
      <td>fifa 16</td>
      <td>ps4</td>
      <td>2015</td>
      <td>sports</td>
      <td>1.12</td>
      <td>6.12</td>
      <td>0.06</td>
      <td>1.28</td>
      <td>82.0</td>
      <td>4.3</td>
      <td>e</td>
      <td>8.58</td>
    </tr>
    <tr>
      <th>87</th>
      <td>star wars battlefront (2015)</td>
      <td>ps4</td>
      <td>2015</td>
      <td>shooter</td>
      <td>2.99</td>
      <td>3.49</td>
      <td>0.22</td>
      <td>1.28</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>undef</td>
      <td>7.98</td>
    </tr>
  </tbody>
</table>
</div>



**Посмотрим какие платформы лидируют по продажам, растут или падают. Выберемнесколько потенциально прибыльных платформ.**


```python
actual_df_pivot = actual_df.pivot_table(index='platform', columns='year_of_release', values = 'total_sales', aggfunc = 'sum')
sns.heatmap(actual_df_pivot, annot=True, cmap="Blues", fmt='.2f')
```




    <AxesSubplot:xlabel='year_of_release', ylabel='platform'>




    
![png](output_89_1.png)
    


Из полученных данных мы видим, что в топе продаж приставки PS4 и XONE. По данным видно, что практически все платформы находятся в стадии снижения показателей.  
Потенциально прибыльными платформами можем считать: PS4, XONE, 3DS и PC как самую распространенную и долгоживущую платформу.  
У PS3 и X360, тоже высокие показатели, но мы их использовать не будем, так как они уже не актуальны.

**Построим график «ящик с усами» по глобальным продажам игр в разбивке по платформам.**


```python
#оставим в таблице только актуальные платформы
#actual_platform = ['ps4', 'xone', '3ds', 'pc']
#actual_df = actual_df.query('platform == @actual_platform')
#actual_df.head()
```


```python
# построим общую диаграмму размаха
actual_df.boxplot(column='total_sales', by='platform', figsize=(15, 10))
plt.show()
actual_df.boxplot(column='total_sales', by='platform', figsize=(15, 10))
plt.ylim(0, 5)
plt.show()
actual_df.boxplot(column='total_sales', by='platform', figsize=(15, 10))
plt.ylim(0, 1)
plt.show()
```


    
![png](output_93_0.png)
    



    
![png](output_93_1.png)
    



    
![png](output_93_2.png)
    



```python
actual_df.groupby('platform')['total_sales'].describe()
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
      <th>mean</th>
      <th>std</th>
      <th>min</th>
      <th>25%</th>
      <th>50%</th>
      <th>75%</th>
      <th>max</th>
    </tr>
    <tr>
      <th>platform</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>3ds</th>
      <td>212.0</td>
      <td>0.408868</td>
      <td>1.188368</td>
      <td>0.01</td>
      <td>0.0300</td>
      <td>0.090</td>
      <td>0.2500</td>
      <td>11.68</td>
    </tr>
    <tr>
      <th>pc</th>
      <td>152.0</td>
      <td>0.180263</td>
      <td>0.328559</td>
      <td>0.01</td>
      <td>0.0200</td>
      <td>0.060</td>
      <td>0.2050</td>
      <td>3.05</td>
    </tr>
    <tr>
      <th>ps3</th>
      <td>219.0</td>
      <td>0.311324</td>
      <td>0.633059</td>
      <td>0.01</td>
      <td>0.0400</td>
      <td>0.110</td>
      <td>0.3250</td>
      <td>5.27</td>
    </tr>
    <tr>
      <th>ps4</th>
      <td>376.0</td>
      <td>0.766356</td>
      <td>1.614969</td>
      <td>0.01</td>
      <td>0.0575</td>
      <td>0.185</td>
      <td>0.6900</td>
      <td>14.63</td>
    </tr>
    <tr>
      <th>psp</th>
      <td>13.0</td>
      <td>0.027692</td>
      <td>0.027735</td>
      <td>0.01</td>
      <td>0.0100</td>
      <td>0.020</td>
      <td>0.0200</td>
      <td>0.09</td>
    </tr>
    <tr>
      <th>psv</th>
      <td>295.0</td>
      <td>0.075932</td>
      <td>0.141591</td>
      <td>0.01</td>
      <td>0.0200</td>
      <td>0.040</td>
      <td>0.0900</td>
      <td>1.96</td>
    </tr>
    <tr>
      <th>wii</th>
      <td>11.0</td>
      <td>0.460909</td>
      <td>0.625451</td>
      <td>0.01</td>
      <td>0.0350</td>
      <td>0.180</td>
      <td>0.7550</td>
      <td>2.01</td>
    </tr>
    <tr>
      <th>wiiu</th>
      <td>73.0</td>
      <td>0.588767</td>
      <td>1.161467</td>
      <td>0.01</td>
      <td>0.0500</td>
      <td>0.190</td>
      <td>0.5700</td>
      <td>7.09</td>
    </tr>
    <tr>
      <th>x360</th>
      <td>111.0</td>
      <td>0.434414</td>
      <td>0.628967</td>
      <td>0.01</td>
      <td>0.0700</td>
      <td>0.180</td>
      <td>0.5050</td>
      <td>4.28</td>
    </tr>
    <tr>
      <th>xone</th>
      <td>228.0</td>
      <td>0.615614</td>
      <td>1.046513</td>
      <td>0.01</td>
      <td>0.0500</td>
      <td>0.205</td>
      <td>0.6325</td>
      <td>7.39</td>
    </tr>
  </tbody>
</table>
</div>



На графике видим выбросы. Данные выбросы оставим, так как на каждой платформе есть свои эксклюзивные игры. Или игры которы продаются на этой платформе лучше.  
Заметно, что практически у всех платформ большая часть значений больше медианного. У платформ XONE, X360, WIIU, WII, PS4 медиана находится в районе значения 0.2.  
Наиболее длинный ряд упешно продающихся игр у PS4 и XONE, следом WIIU.

### Зависимость между объемом продаж и отзывами пользователей и критиков

[Содержание](#intro)

**Посмотрим, как влияют на продажи внутри одной популярной платформы отзывы пользователей и критиков. Построим диаграмму рассеяния и посчитаем корреляцию между отзывами и продажами.**


```python
print('Диаграмма рассеяния для PS4')
actual_df[actual_df['platform']=='ps4'].plot(x='user_score', y='total_sales', kind='scatter', alpha=0.3, figsize=(15,5), grid=True)
plt.show()
actual_df[actual_df['platform']=='ps4'].plot(x='critic_score', y='total_sales', kind='scatter', alpha=0.3, figsize=(15,5), grid=True)
plt.show()
```

    Диаграмма рассеяния для PS4



    
![png](output_98_1.png)
    



    
![png](output_98_2.png)
    


Посмотрим корреляцию


```python
plt.figure(figsize=(8,6))
sns.heatmap(actual_df[actual_df['platform']=='ps4'].corr(), annot=True, cmap='Blues', fmt='.2f')
```




    <AxesSubplot:>




    
![png](output_100_1.png)
    


По графикам видно, что большую часть составляют оценки от 6 до 8.  
По данным тепловой карты у нас :
- слабо отрицательная (можно сказать отсутствует) корреляция -0.04 по отзывам покупателей
- слабо положительная корреляция 0.4 по отзывам критиков  

**Соотнесем выводы с продажами игр на других платформах.**


```python
print('Диаграмма рассеяния для XONE')
actual_df[actual_df['platform']=='xone'].plot(x='user_score', y='total_sales', kind='scatter', alpha=0.3, figsize=(15,5), grid=True)
plt.show()
actual_df[actual_df['platform']=='xone'].plot(x='critic_score', y='total_sales', kind='scatter', alpha=0.3, figsize=(15,5), grid=True)
plt.show()
print('Корреляция с user_score:', actual_df[actual_df['platform']=='xone']['user_score'].corr(actual_df[actual_df['platform']=='xone']['total_sales']))
print('Корреляция с critic_score:', actual_df[actual_df['platform']=='xone']['critic_score'].corr(actual_df[actual_df['platform']=='xone']['total_sales']))
```

    Диаграмма рассеяния для XONE



    
![png](output_103_1.png)
    



    
![png](output_103_2.png)
    


    Корреляция с user_score: -0.0703839280647581
    Корреляция с critic_score: 0.42867694370333226



```python
print('Диаграмма рассеяния для 3DS')
actual_df[actual_df['platform']=='3ds'].plot(x='user_score', y='total_sales', kind='scatter', alpha=0.3, figsize=(15,5), grid=True)
plt.show()
actual_df[actual_df['platform']=='3ds'].plot(x='critic_score', y='total_sales', kind='scatter', alpha=0.3, figsize=(15,5), grid=True)
plt.show()
print('Корреляция с user_score:', actual_df[actual_df['platform']=='3ds']['user_score'].corr(actual_df[actual_df['platform']=='3ds']['total_sales']))
print('Корреляция с critic_score:', actual_df[actual_df['platform']=='3ds']['critic_score'].corr(actual_df[actual_df['platform']=='3ds']['total_sales']))

```

    Диаграмма рассеяния для 3DS



    
![png](output_104_1.png)
    



    
![png](output_104_2.png)
    


    Корреляция с user_score: 0.2151932718527028
    Корреляция с critic_score: 0.314117492869051



```python
print('Диаграмма рассеяния для PC')
actual_df[actual_df['platform']=='pc'].plot(x='user_score', y='total_sales', kind='scatter', alpha=0.3, figsize=(15,5), grid=True)
plt.show()
actual_df[actual_df['platform']=='pc'].plot(x='critic_score', y='total_sales', kind='scatter', alpha=0.3, figsize=(15,5), grid=True)
plt.show()
print('Корреляция с user_score:', actual_df[actual_df['platform']=='pc']['user_score'].corr(actual_df[actual_df['platform']=='pc']['total_sales']))
print('Корреляция с critic_score:', actual_df[actual_df['platform']=='pc']['critic_score'].corr(actual_df[actual_df['platform']=='pc']['total_sales']))

```

    Диаграмма рассеяния для PC



    
![png](output_105_1.png)
    



    
![png](output_105_2.png)
    


    Корреляция с user_score: -0.0664171785890673
    Корреляция с critic_score: 0.17727193487623283


Из полученных данных видно, что с другими платформами аналогичная ситуация:
- слабо отрицательная (можно сказать отсутствует) корреляция по отзывам покупателей
- слабо положительная корреляция по отзывам критиков  

Исключение платформа 3DS, здесь корреляция одинаково низкая для всех оценок. Но корреляция с user_score немного выше чем у остальных.

### Общее распределение игр по жанрам

[Содержание](#intro)

**Посмотрим на общее распределение игр по жанрам.**


```python
actual_df.plot(x='genre', y='total_sales', kind='scatter', grid=True, figsize=(15, 5), alpha=0.5)
plt.show()
```


    
![png](output_109_0.png)
    



```python
genre_sales = actual_df.groupby('genre')['total_sales'].sum().to_frame().sort_values('total_sales', ascending=False).reset_index()
genre_sales.head()
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
      <th>genre</th>
      <th>total_sales</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>action</td>
      <td>199.71</td>
    </tr>
    <tr>
      <th>1</th>
      <td>shooter</td>
      <td>170.94</td>
    </tr>
    <tr>
      <th>2</th>
      <td>sports</td>
      <td>109.48</td>
    </tr>
    <tr>
      <th>3</th>
      <td>role-playing</td>
      <td>101.44</td>
    </tr>
    <tr>
      <th>4</th>
      <td>misc</td>
      <td>37.55</td>
    </tr>
  </tbody>
</table>
</div>




```python
plt.figure(figsize=(15,6))
sns.barplot(data=genre_sales, x='genre', y='total_sales')
plt.xlabel('Жанры',fontsize=12)
plt.ylabel('Продажи',fontsize=12)
```




    Text(0, 0.5, 'Продажи')




    
![png](output_111_1.png)
    



```python
# построим общую диаграмму размаха
actual_df.boxplot(column='total_sales', by='genre', figsize=(15, 10))
plt.show()
actual_df.boxplot(column='total_sales', by='genre', figsize=(15, 10))
plt.ylim(0, 5)
plt.show()
actual_df.boxplot(column='total_sales', by='genre', figsize=(15, 10))
plt.ylim(0, 2)
plt.show()
```


    
![png](output_112_0.png)
    



    
![png](output_112_1.png)
    



    
![png](output_112_2.png)
    



```python
actual_df.groupby('genre')['total_sales'].describe()
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
      <th>mean</th>
      <th>std</th>
      <th>min</th>
      <th>25%</th>
      <th>50%</th>
      <th>75%</th>
      <th>max</th>
    </tr>
    <tr>
      <th>genre</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>action</th>
      <td>620.0</td>
      <td>0.322113</td>
      <td>0.797537</td>
      <td>0.01</td>
      <td>0.0300</td>
      <td>0.090</td>
      <td>0.2800</td>
      <td>12.62</td>
    </tr>
    <tr>
      <th>adventure</th>
      <td>185.0</td>
      <td>0.094865</td>
      <td>0.203147</td>
      <td>0.01</td>
      <td>0.0200</td>
      <td>0.030</td>
      <td>0.0800</td>
      <td>1.66</td>
    </tr>
    <tr>
      <th>fighting</th>
      <td>60.0</td>
      <td>0.470333</td>
      <td>1.188053</td>
      <td>0.01</td>
      <td>0.0600</td>
      <td>0.125</td>
      <td>0.3200</td>
      <td>7.55</td>
    </tr>
    <tr>
      <th>misc</th>
      <td>113.0</td>
      <td>0.332301</td>
      <td>0.736999</td>
      <td>0.01</td>
      <td>0.0300</td>
      <td>0.090</td>
      <td>0.3200</td>
      <td>5.27</td>
    </tr>
    <tr>
      <th>platform</th>
      <td>38.0</td>
      <td>0.476053</td>
      <td>0.722561</td>
      <td>0.01</td>
      <td>0.0625</td>
      <td>0.140</td>
      <td>0.4675</td>
      <td>3.21</td>
    </tr>
    <tr>
      <th>puzzle</th>
      <td>14.0</td>
      <td>0.157857</td>
      <td>0.320629</td>
      <td>0.01</td>
      <td>0.0200</td>
      <td>0.045</td>
      <td>0.1000</td>
      <td>1.19</td>
    </tr>
    <tr>
      <th>racing</th>
      <td>69.0</td>
      <td>0.398841</td>
      <td>0.963716</td>
      <td>0.01</td>
      <td>0.0300</td>
      <td>0.090</td>
      <td>0.2500</td>
      <td>7.09</td>
    </tr>
    <tr>
      <th>role-playing</th>
      <td>221.0</td>
      <td>0.459005</td>
      <td>1.177284</td>
      <td>0.01</td>
      <td>0.0500</td>
      <td>0.110</td>
      <td>0.3600</td>
      <td>11.68</td>
    </tr>
    <tr>
      <th>shooter</th>
      <td>128.0</td>
      <td>1.335469</td>
      <td>2.050567</td>
      <td>0.01</td>
      <td>0.1725</td>
      <td>0.515</td>
      <td>1.6175</td>
      <td>14.63</td>
    </tr>
    <tr>
      <th>simulation</th>
      <td>44.0</td>
      <td>0.298409</td>
      <td>0.646925</td>
      <td>0.01</td>
      <td>0.0200</td>
      <td>0.100</td>
      <td>0.3275</td>
      <td>3.05</td>
    </tr>
    <tr>
      <th>sports</th>
      <td>161.0</td>
      <td>0.680000</td>
      <td>1.239736</td>
      <td>0.01</td>
      <td>0.0600</td>
      <td>0.180</td>
      <td>0.6400</td>
      <td>8.58</td>
    </tr>
    <tr>
      <th>strategy</th>
      <td>37.0</td>
      <td>0.107027</td>
      <td>0.118412</td>
      <td>0.01</td>
      <td>0.0300</td>
      <td>0.060</td>
      <td>0.1400</td>
      <td>0.52</td>
    </tr>
  </tbody>
</table>
</div>



Выделяются жанры с высокими и низкими продажами. 
Заметно, что практически у всех жанров большая часть значений находится больше медианного. Медианы большинства жанров находятся в промежутке от 0.06 до 0.15.
Наиболее длинный ряд упешно продающихся игр в жанре `shooter`, следом `sports`  
Больше всего игр в жанре `action`   
Если рассматривать только самые прибыльные жанры, то можно заметить, что в них сочетаются два параметра, у них большое количество игр или высокое медианное значение.

### Вывод

[Содержание](#intro)


Быстрый рост выпуска игр начинается с 1990. Пик на консольные и компьютерный игры приходится на 2008-2009 года, далее начинается спад.  
Уменьшение количества игр на консоли и на ПК может быть связан с увелечением времени разработки игр (с каждым годом графика и функционал улучшается, геймеры становятся более требовательны), так же в это время начинает расширяться рынок мобильных игр (выходит Айфон, увеличиваются экраны телефонов и их мощность).  
Самые популярные игровые платформы за все время: PS, DS, WII, PS3, X360, PS2.  
Практически все игровые платформы в среднем существуют около 10 лет.
Самый пик выхода игр наступает примерно в середине жизни приставки.
PC самая долгоживущая платформа.
Актуальные период для прогноза на 2017 взяли с 2014 по 2016 года. Данный период выбран в связи с тем, что уже остались только актуальные платформы и за 2016 год данные неполные. Если сделать период меньше, то часть данных будет урезана.  
Из полученных данных мы видим, что в топе продаж приставки PS4 и XONE. По данным видно, что практически все платформы находятся в стадии снижения показателей.
Потенциально прибыльными платформами будем считать: PS4, XONE, 3DS и PC как самую распространенную и долгоживущую платформу.  
На графике видны выбросы. Данные выбросы оставим, так как на каждой платформе есть свои эксклюзивные игры. Или игры которы продаются на этой платформе лучше. Заметно, что практически у всех платформ большая часть значений больше медианного. У платформ XONE, X360, WIIU, WII, PS4 медиана находится в районе значения 0.2.  
Наиболее длинный ряд упешно продающихся игр у PS4 и XONE, следом WIIU.  
По графикам видно, что большую часть составляют оценки от 6 до 8
Из полученных данных видно, что с другими платформами аналогичная ситуация:
слабо отрицательная (можно сказать отсутствует) корреляция по отзывам покупателей;
слабо положительная корреляция по отзывам критиков;
Исключение платформа 3DS, здесь корреляция одинаково низкая для всех оценок. Но корреляция с `user_score` немного выше чем у остальных.  
Выделяются жанры с высокими и низкими продажами. Заметно, что практически у всех жанров большая часть значений находится больше медианного. Медианы большинства жанров находятся в промежутке от 0.06 до 0.15. Наиболее длинный ряд упешно продающихся игр в жанре `shooter`, следом `sports`  
Больше всего игр в жанре `action`.  
Если рассматривать только самые прибыльные жанры, то можно заметить, что в них сочетаются два параметра, у них большое количество игр или высокое медианное значение.

## Шаг 4. Портрет пользователя каждого региона

[Содержание](#intro)

Определим для пользователя каждого региона (NA, EU, JP):  
- Самые популярные платформы (топ-5).
- Самые популярные жанры (топ-5).
- Посмотрим влияет ли рейтинг ESRB на продажи в отдельном регионе.

### Популярность платформ

[Содержание](#intro)

**Посмотрим на топ-5 платформ за 2014-2016 год.**


```python
top_platforms = actual_df.groupby('platform')[['na_sales', 'eu_sales', 'jp_sales']].sum()

plt.figure(figsize=(6,5))
plt.suptitle('Платформы за 2014-2016 год', fontsize=15)
sns.heatmap(top_platforms, annot=True, cmap='Blues', fmt='.2f')
plt.show()
print()

columns = top_platforms.columns

#plt.figure(figsize=(15,4))
#i = 1
#for j in columns:
#    plt.subplot (1, 3, i)
#    a = actual_df.groupby('platform')[j].sum().to_frame().nlargest(5, j)
#    sns.barplot(data=a, x=a.index, y=j)
#    i += 1

land = ['Северной Америке', 'Европе', 'Японии']

fig , ax = plt.subplots(nrows = 1, ncols = 3, figsize=(20, 6))
fig.suptitle('Топ-5 платформ за 2014-2016 год', fontsize=20)
for i in [0, 1, 2]:
    a = actual_df.groupby('platform')[columns[i]].sum().to_frame().sort_values(by = columns[i], ascending=False).reset_index()
    top_5 = a.nlargest(5, columns[i])
    other_value = (a[columns[i]].sum()) - (top_5[columns[i]].sum())
    new_row = {'platform':'other', columns[i]:other_value}
    top_5 = top_5.append(new_row, ignore_index = True)
    sns.barplot(data = top_5, x ='platform', y = columns[i], ax = ax[i])
    ax[i].set_title('Какие платформы популярны в ' + land[i])
plt.show()

fig , ax = plt.subplots(nrows = 1, ncols = 3, figsize=(18,5))
for j in [0, 1, 2]:
    a = actual_df.groupby('platform')[columns[j]].sum().to_frame().sort_values(by = columns[j], ascending=False).reset_index()
    top_5 = a.nlargest(5, columns[j])
    other_value = (a[columns[j]].sum()) - (top_5[columns[j]].sum())
    new_row = {'platform':'other', columns[j]:other_value}
    top_5 = top_5.append(new_row, ignore_index = True)
    ax[j].pie(top_5[columns[j]], labels=top_5['platform'], autopct='%1.1f%%')
plt.show()
```


    
![png](output_120_0.png)
    


    



    
![png](output_120_2.png)
    



    
![png](output_120_3.png)
    


На графиках видно, что PS4 самая популярная платформа в Северной Америке и Европе (доли внутри рынка 34.7% и 48% соответственно), следом идет XONE (28.6% и 17.1%).  
В Японии наиболее популярна 3DS(доля от продаж в Японии 47.5%), следом PS4 (16.1%).

**Теперь посмотрим топ-5 самых популярных платформ за все время в каждом регионе**


```python
#display(df.head())
#display(columns)
i = 1
plt.figure(figsize=(15,4))
plt.suptitle('Продажи топ-5 платформ за все время', fontsize=15)
for j in columns:
    plt.subplot (1, 3, i)
    a = df.groupby('platform')[j].sum().to_frame().nlargest(5, j)
    a['ratio'] = a[j] / df.groupby('platform')['total_sales'].sum()
    #display(a)
    sns.heatmap(a, annot=True, cmap='Blues', fmt='.2f')
    i += 1
print()

#i = 1
#plt.figure(figsize=(15,5))
#for j in columns:
#    plt.subplot (1, 3, i)
#    a = df.groupby('platform')[j].sum().to_frame().nlargest(5, j)
#    sns.barplot(data=a, x=a.index, y=j)
#    i += 1


fig , ax = plt.subplots(nrows = 1, ncols = 3, figsize=(20, 6))
fig.suptitle('Топ-5 платформ за все время', fontsize=20)
for i in [0, 1, 2]:
    a = df.groupby('platform')[columns[i]].sum().to_frame().sort_values(by = columns[i], ascending=False).reset_index()
    top_5 = a.nlargest(5, columns[i])
    other_value = (a[columns[i]].sum()) - (top_5[columns[i]].sum())
    new_row = {'platform':'other', columns[i]:other_value}
    top_5 = top_5.append(new_row, ignore_index = True)
    sns.barplot(data = top_5, x ='platform', y = columns[i], ax = ax[i])
    ax[i].set_title('Какие платформы популярны в ' + land[i])
plt.show()

fig , ax = plt.subplots(nrows = 1, ncols = 3, figsize=(18,5))
for j in [0, 1, 2]:
    a = df.groupby('platform')[columns[j]].sum().to_frame().sort_values(by = columns[j], ascending=False).reset_index()
    top_5 = a.nlargest(5, columns[j])
    other_value = (a[columns[j]].sum()) - (top_5[columns[j]].sum())
    new_row = {'platform':'other', columns[j]:other_value}
    top_5 = top_5.append(new_row, ignore_index = True)
    ax[j].pie(top_5[columns[j]], labels=top_5['platform'], autopct='%1.1f%%')
plt.show()
```

    



    
![png](output_122_1.png)
    



    
![png](output_122_2.png)
    



    
![png](output_122_3.png)
    


На североамериканском рынке наибольшим спросом пользовались игры на консолях X360. Доля занимает 62% от объема общемировых продаж по этой консоли.  
Внутри рынка у этой консоли доля в 13.7% от всех.
Вообще на рынке Америки самые большие доли продаж.

На рынке Европы наибольшим спросом пользовались игры на PS2 и PS3. Доля этих консолей 27% и 35% от общего количества.  
Внутри рынка доля составляет 14% и 13.6% соответственно.

В Японии наибольшим спросом пользовались игры на приставках DS. 22% от всех продаж по этой консоли и 13.5% внутри региона.

### Популярные жанры

[Содержание](#intro)

**Посмотрим на топ-5 популярных жанров в 2014-2016 годах.**


```python
top_genre = actual_df.groupby('genre')[['na_sales', 'eu_sales', 'jp_sales']].sum()

plt.figure(figsize=(6,5))
plt.suptitle('Жанры за 2014-2016 год', fontsize=15)
sns.heatmap(top_genre, annot=True, cmap='Blues', fmt='.2f')
plt.show()
print()

#for j in columns:
#    plt.figure(figsize=(15,4))
#    sns.barplot(data=top_genre, x=top_genre.index, y=j)
    
fig , ax = plt.subplots(nrows = 1, ncols = 3, figsize=(20, 6))
fig.suptitle('Топ-5 популярных жанров в 2014-2016 годах', fontsize=20)
for i in [0, 1, 2]:
    a = actual_df.groupby('genre')[columns[i]].sum().to_frame().sort_values(by = columns[i], ascending=False).reset_index()
    top_5 = a.nlargest(5, columns[i])
    other_value = (a[columns[i]].sum()) - (top_5[columns[i]].sum())
    new_row = {'genre':'other', columns[i]:other_value}
    top_5 = top_5.append(new_row, ignore_index = True)
    sns.barplot(data = top_5, x ='genre', y = columns[i], ax = ax[i])
    ax[i].set_title('Какие жанры популярны в ' + land[i])
plt.show()

fig , ax = plt.subplots(nrows = 1, ncols = 3, figsize=(18,5))
for j in [0, 1, 2]:
    a = actual_df.groupby('genre')[columns[j]].sum().to_frame().sort_values(by = columns[j], ascending=False).reset_index()
    top_5 = a.nlargest(5, columns[j])
    other_value = (a[columns[j]].sum()) - (top_5[columns[j]].sum())
    new_row = {'genre':'other', columns[j]:other_value}
    top_5 = top_5.append(new_row, ignore_index = True)
    ax[j].pie(top_5[columns[j]], labels=top_5['genre'], autopct='%1.1f%%')
plt.show()
```


    
![png](output_125_0.png)
    


    



    
![png](output_125_2.png)
    



    
![png](output_125_3.png)
    


Наиболее популярный жанр на рынке Америки "shooter" и "action"(доля внутри рынка 27.8% и 25.5%)  
В Европе похожая картина, но на первом месте "action" а на втором "shooter" (27.7% и 24.2%). Вообще на этих рынках картина с предпочтениями жанра схожа.  
В Японии больше популярны жанры "action" и "role-playing" (31.8% и 33.5% соответственно).

**Взглянем на топ-5 за все время**.


```python
i = 1
plt.figure(figsize=(22,4))
plt.suptitle('Продажи топ-5 за все время', fontsize=20)
for j in columns:
    plt.subplot (1, 3, i)
    a = df.groupby('genre')[j].sum().to_frame().nlargest(5, j)
    #display(a)
    sns.heatmap(a, annot=True, cmap='Blues', fmt='.2f')
    i += 1
print()

#i = 1
#plt.figure(figsize=(18,5))
#for j in columns:
#    plt.subplot (1, 3, i)
#    a = df.groupby('genre')[j].sum().to_frame().nlargest(5, j)
#    sns.barplot(data=a, x=a.index, y=j)
#    i += 1
    
land = ['Северной Америке', 'Европе', 'Японии']
fig , ax = plt.subplots(nrows = 1, ncols = 3, figsize=(20, 6))
fig.suptitle('Топ-5 популярных жанров за все время', fontsize=20)
for i in [0, 1, 2]:
    a = df.groupby('genre')[columns[i]].sum().to_frame().sort_values(by = columns[i], ascending=False).reset_index()
    top_5 = a.nlargest(5, columns[i])
    other_value = (a[columns[i]].sum()) - (top_5[columns[i]].sum())
    new_row = {'genre':'other', columns[i]:other_value}
    top_5 = top_5.append(new_row, ignore_index = True)
    sns.barplot(data = top_5, x ='genre', y = columns[i], ax = ax[i])
    ax[i].set_title('Какие жанры популярны в ' + land[i])
plt.show()

fig , ax = plt.subplots(nrows = 1, ncols = 3, figsize=(18,5))
for j in [0, 1, 2]:
    a = df.groupby('genre')[columns[j]].sum().to_frame().sort_values(by = columns[j], ascending=False).reset_index()
    top_5 = a.nlargest(5, columns[j])
    other_value = (a[columns[j]].sum()) - (top_5[columns[j]].sum())
    new_row = {'genre':'other', columns[j]:other_value}
    top_5 = top_5.append(new_row, ignore_index = True)
    ax[j].pie(top_5[columns[j]], labels=top_5['genre'], autopct='%1.1f%%')
plt.show()
```

    



    
![png](output_127_1.png)
    



    
![png](output_127_2.png)
    



    
![png](output_127_3.png)
    


На территории Северной Америки и Европы за все время самым популярным жанром был "action" (доля внутри рынка 20% и 21.4% соответственно), далее спортивные игры (доля внутри рынка 15.6% и 15.5%).  
В Японии предпочтения почти не меняются. На первом месте "role_playing" (доля внутри рынка 27%) и на втором "action" (12.4%).  
По полученным данным можно сказать, что игры в жанре "action" стабильно хорошо востребованы во все временые промежутки и на разных рынках.

### Влияние рейтинга ESRB на продажи в отдельных регионах

[Содержание](#intro)

**Посмотрим влияет ли рейтинг ESRB на продажи в отдельном регионе.**  
Возьмем данные за актуальный период.


```python
df_esrb = actual_df.groupby('rating')[['na_sales', 'eu_sales', 'jp_sales']].sum()
plt.figure(figsize=(15,5))
sns.heatmap(df_esrb, annot=True, cmap='Blues', fmt='.2f')
plt.show()
```


    
![png](output_130_0.png)
    


Вспомним категорий рейтинга:  
**«E»** («Everyone») — «Для всех»  
**«E10+»** («Everyone 10 and older») — «Для всех от 10 лет и старше»  
**«T»** («Teen») — «Подросткам»  
**«M»** («Mature») — «Для взрослых»  

В Европе и Америке наиболее восстребован жанр **«M»** («Mature») — «Для взрослых», на втором месте **«E»** («Everyone») — «Для всех».  
В Японии на первом месте **«T»** («Teen») — «Подросткам»  и на втором **«E»** («Everyone») — «Для всех»

### Вывод

[Содержание](#intro)

На графиках видно, что PS4 самая популярная платформа в Северной Америке и Европе (доли внутри рынка 34.7% и 48% соответственно), следом идет XONE (28.6% и 17.1%).  
В Японии наиболее популярна 3DS(доля от продаж в Японии 47.5%), следом PS4 (16.1%).  
За все время на североамериканском рынке наибольшим спросом пользовались игры на консолях X360. Доля занимает 62% от объема общемировых продаж по этой консоли.  
Внутри рынка у этой консоли доля в 13.7% от всех.
Вообще на рынке Америки самые большие доли продаж.  
На рынке Европы наибольшим спросом пользовались игры на PS2 и PS3. Доля этих консолей 27% и 35% от общего количества.  
Внутри рынка доля составляет 14% и 13.6% соответственно.  
В Японии наибольшим спросом пользовались игры на приставках DS. 22% от всех продаж по этой консоли и 13.5% внутри региона.  
Наиболее популярный жанр на рынке Америки "shooter" и "action"(доля внутри рынка 27.8% и 25.5%)  
В Европе похожая картина, но на первом месте "action" а на втором "shooter" (27.7% и 24.2%). Вообще на этих рынках картина с предпочтениями жанра схожа.  
В Японии больше популярны жанры "action" и "role-playing" (31.8% и 33.5% соответственно).
За все время на территории Северной Америки и Европы за все время самым популярным жанром был "action" (доля внутри рынка 20% и 21.4% соответственно), далее спортивные игры (доля внутри рынка 15.6% и 15.5%).  
В Японии предпочтения почти не меняются. На первом месте "role_playing" (доля внутри рынка 27%) и на втором "action" (12.4%).  
По полученным данным можно сказать, что игры в жанре "action" стабильно хорошо востребованы во все временые промежутки и на разных рынках.  
В Европе и Америке наиболее восстребован жанр **«M»** («Mature») — «Для взрослых», на втором месте **«E»** («Everyone») — «Для всех».  
В Японии на первом месте **«T»** («Teen») — «Подросткам»  и на втором **«E»** («Everyone») — «Для всех»

## Шаг 5. Проверка гипотез

Проверять будем на выборках из всей таблицы.  
Будем использовать метод `st.ttest_ind()`, так как он позваляет сравнивать средние двух генеральных совокупностей между собой.

### Гипотеза 1

[Содержание](#intro)

Нулевая гипотеза: Средние пользовательские рейтинги платформ Xbox One и PC одинаковые  
Альтернативная гипотеза : Средние пользовательские рейтинги платформ Xbox One и PC отличаются

Сделаем две выборки по названиям платформ


```python
xone_hyp = df[(df['platform']=='xone') & (df['year_of_release']>=2013)]['user_score']#XONE появился в 2013
pc_hyp = df[(df['platform']=='pc') & (df['year_of_release']>=2013)]['user_score']

print('Средняя оценка для XONE:', xone_hyp.mean())
print('Средняя оценка для PC:', pc_hyp.mean())
```

    Средняя оценка для XONE: 6.521428571428572
    Средняя оценка для PC: 6.280379746835442



```python
alpha = .05

results = st.ttest_ind(xone_hyp.dropna(), pc_hyp.dropna(), equal_var=False)

print('p-значение:', results.pvalue)

if results.pvalue < alpha:
    print("Отвергаем нулевую гипотезу")
else:
    print("Не получилось отвергнуть нулевую гипотезу")
```

    p-значение: 0.16174359801784308
    Не получилось отвергнуть нулевую гипотезу


Значение p-value больше 16%. Опровергнуть нулевую гипотезу не получилось.  
Вероятность случайно получить такое или большее различие - 16%.  
Можем сделать вывод, что средние пользовательские рейтинги платформ Xbox One и PC похожи.

### Гипотеза 2

[Содержание](#intro)

Нулевая гипотеза: Средние пользовательские рейтинги жанров Action и Sports одинаковые  
Альтернативная гипотеза : Средние пользовательские рейтинги жанров Action и Sports отличаются

Сделаем две выборки по жанрам


```python
action_hyp = df[df['genre'] == 'action']['user_score']
sports_hyp = df[df['genre'] == 'sports']['user_score']
print('Средняя оценка для action:', action_hyp.mean())
print('Средняя оценка для sports:', sports_hyp.mean())
```

    Средняя оценка для action: 7.0564835164835165
    Средняя оценка для sports: 6.956375227686704



```python
alpha = .05

result = st.ttest_ind(action_hyp.dropna(), sports_hyp.dropna(), equal_var=False)

print('p-значение:', result.pvalue)

if result.pvalue < alpha:
    print("Отвергаем нулевую гипотезу")
else:
    print("Не получилось отвергнуть нулевую гипотезу")
```

    p-значение: 0.08991887133875968
    Не получилось отвергнуть нулевую гипотезу


Значение p-value больше 8.9%. Опровергнуть нулевую гипотезу не получилось.  
Вероятность случайно получить такое или большее различие почти 9%.  
Можем сделать вывод, что средние пользовательские рейтинги жанров Action и Sports похожи.

## Общий вывод

[Содержание](#intro)

Перед анализом данных, мы провели предобработку данных: привели к правильным данным столбцы, привели к нижнему регистру значения таблицы и сами названия колонок и привели к правильным типам данных необходимые столбцы.

Определили, что аббревиатура TBD значит to be determined, to be done. То есть данные были нарочно не заполнены. Поэтому заменили tbd на nan для проведения дальнейшего анализа.

Проведя анализ, мы выявили, что быстрый рост выпуска игр начинается с 1990. Пик на консольные и компьютерный игры приходится на 2008-2009 года, далее начинается спад.
Уменьшение количества игр на консоли и на ПК может быть связан с увелечением времени разработки игр (с каждым годом графика и функционал улучшается, геймеры становятся более требовательны), так же в это время начинает расширяться рынок мобильных игр (выходит Айфон, увеличиваются экраны телефонов и их мощность).

За весь период консольных приставок самые популярные оказались: PS, DS, WII, PS3, X360, PS2.
Также мы выявили, что gрактически все игровые платформы в среднем существуют около 10 лет.
Самый пик наступает примерно в середине жизни приставки.
PC самая долгоживущая платформа.

Проведя анализ оценок пользователей и критиков. Мы выявили, что оценки критиков взаимосвязаны с продажами самих игр. То есть чем больше оценка критиков или пользователей, тем лучше продажа игры, но зависимость слабая.
Так же определили, что самый популярный жанр за все время это "action".

После мы составили портреты пользователей каждого региона.
Выяснили, что PS4 самая популярная платформа в Северной Америке и Европе (доли внутри рынка 34.7% и 48% соответственно), следом идет XONE (28.6% и 17.1%).
В Японии наиболее популярна 3DS(доля от продаж в Японии 47.5%), следом PS4 (16.1%).

Наиболее популярный жанр на рынке Америки "shooter" и "action"(доля внутри рынка 27.8% и 25.5%)  
В Европе похожая картина, но на первом месте "action" а на втором "shooter" (27.7% и 24.2%). Вообще на этих рынках картина с предпочтениями жанра схожа.  
В Японии больше популярны жанры "action" и "role-playing" (31.8% и 33.5% соответственно).  
По полученным данным можно сказать, что игры в жанре "action" стабильно хорошо востребованы во все временые промежутки и на разных рынках.  
В Европе и Америке наиболее восстребован жанр **«M»** («Mature») — «Для взрослых», на втором месте **«E»** («Everyone») — «Для всех».  
В Японии на первом месте **«T»** («Teen») — «Подросткам»  и на втором **«E»** («Everyone») — «Для всех»

**Исходя из всех данных предполагаю, что лучше всего продавать игры для таких приставок как Sony Playstation 4.  
Жанр необходимо выбирать "action" или "shooter".  
И выбирать игры с рейтингом «E»(«Everyone») — «Для всех» или «M» («Mature») — «Для взрослых».  
Тогда продажи будут значительно больше.**
