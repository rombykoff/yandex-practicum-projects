# Анализ взаимодействия пользователей с карточками Яндекс.Дзен
<a id='intro'></a>
Ответим на вопросы менеджеров, используя дашборд.

<h1>Содержание<span class="tocSkip"></span></h1>
<div class="toc"><ul class="toc-item"><li><span><a href="#Краткое-ТЗ:" data-toc-modified-id="Краткое-ТЗ:-1">Краткое ТЗ:</a></span></li><li><span><a href="#Запрос-к-базе-данных" data-toc-modified-id="Запрос-к-базе-данных-2">Запрос к базе данных</a></span></li><li><span><a href="#Предобработка-данных" data-toc-modified-id="Предобработка-данных-3">Предобработка данных</a></span></li><li><span><a href="#Экспорт-данных-в-файл" data-toc-modified-id="Экспорт-данных-в-файл-4">Экспорт данных в файл</a></span></li><li><span><a href="#Дашборд-и-презентация" data-toc-modified-id="Дашборд-и-презентация-5">Дашборд и презентация</a></span></li></ul></div>

## Краткое ТЗ:
[Содержание](#intro)

- Бизнес-задача: анализ взаимодействия пользователей с карточками Яндекс.Дзен;
- Насколько часто предполагается пользоваться дашбордом: не реже, чем раз в неделю;
- Кто будет основным пользователем дашборда: менеджеры по анализу контента;
- Состав данных для дашборда:  
История событий по темам карточек (два графика - абсолютные числа и процентное соотношение);  
Разбивка событий по темам источников;  
Таблица соответствия тем источников темам карточек;  
- По каким параметрам данные должны группироваться:  
Дата и время;  
Тема карточки;  
Тема источника;  
Возрастная группа;  
- Характер данных:  
История событий по темам карточек — абсолютные величины с разбивкой по минутам;  
Разбивка событий по темам источников — относительные величины (% событий);  
Соответствия тем источников темам карточек - абсолютные величины;  
- Важность: все графики имеют равную важность;
- Источники данных для дашборда: cырые данные о событиях взаимодействия пользователей с карточками;
- Частота обновления данных: один раз в сутки, в полночь по UTC;
- Какие графики должны отображаться и в каком порядке, какие элементы управления должны быть на дашборде (макет дашборда):

![Image.png](attachment:Image.png)

## Запрос к базе данных
[Содержание](#intro)

Сделаем запрос к базе данных и сохраним информацию в csv-файл


```python
# импортируем библиотеки
import pandas as pd
import matplotlib.pyplot as plt
import seaborn as sns
from sqlalchemy import create_engine
```


```python
db_config = {'user': 'praktikum_student', # имя пользователя
            'pwd': 'Sdf4$2;d-d30pp', # пароль
            'host': 'rc1b-wcoijxj3yxfsf3fs.mdb.yandexcloud.net',
            'port': 6432, # порт подключения
            'db': 'data-analyst-zen-project-db'} # название базы данных

connection_string = 'postgresql://{}:{}@{}:{}/{}'.format(db_config['user'],
                                                db_config['pwd'],
                                                db_config['host'],
                                                db_config['port'],
                                                db_config['db'])

engine = create_engine(connection_string)
```


```python
query = '''SELECT *
           FROM dash_visits
'''
```


```python
df = pd.io.sql.read_sql(query, con = engine)
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
      <th>record_id</th>
      <th>item_topic</th>
      <th>source_topic</th>
      <th>age_segment</th>
      <th>dt</th>
      <th>visits</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>1040597</td>
      <td>Деньги</td>
      <td>Авто</td>
      <td>18-25</td>
      <td>2019-09-24 18:32:00</td>
      <td>3</td>
    </tr>
    <tr>
      <th>1</th>
      <td>1040598</td>
      <td>Деньги</td>
      <td>Авто</td>
      <td>18-25</td>
      <td>2019-09-24 18:35:00</td>
      <td>1</td>
    </tr>
    <tr>
      <th>2</th>
      <td>1040599</td>
      <td>Деньги</td>
      <td>Авто</td>
      <td>18-25</td>
      <td>2019-09-24 18:54:00</td>
      <td>4</td>
    </tr>
    <tr>
      <th>3</th>
      <td>1040600</td>
      <td>Деньги</td>
      <td>Авто</td>
      <td>18-25</td>
      <td>2019-09-24 18:55:00</td>
      <td>17</td>
    </tr>
    <tr>
      <th>4</th>
      <td>1040601</td>
      <td>Деньги</td>
      <td>Авто</td>
      <td>18-25</td>
      <td>2019-09-24 18:56:00</td>
      <td>27</td>
    </tr>
  </tbody>
</table>
</div>



## Предобработка данных
[Содержание](#intro)

Посмотрим информацию о таблице, проверим наличие пропусков и дубликатов.


```python
df.info()
```

    <class 'pandas.core.frame.DataFrame'>
    RangeIndex: 30745 entries, 0 to 30744
    Data columns (total 6 columns):
     #   Column        Non-Null Count  Dtype         
    ---  ------        --------------  -----         
     0   record_id     30745 non-null  int64         
     1   item_topic    30745 non-null  object        
     2   source_topic  30745 non-null  object        
     3   age_segment   30745 non-null  object        
     4   dt            30745 non-null  datetime64[ns]
     5   visits        30745 non-null  int64         
    dtypes: datetime64[ns](1), int64(2), object(3)
    memory usage: 1.4+ MB


Пропусков в таблице нет, столбцы имеют нужный тип данных. Посмотрим на дубликаты.


```python
print('Количество явных строк-дубликатов:', df.duplicated().sum())
```

    Количество явных строк-дубликатов: 0


Посмотрим данные по столбцам.


```python
print(df['item_topic'].unique())
print()
print(df['source_topic'].unique())
print()
print(df['age_segment'].unique())
print()
print(df['dt'].unique())
```

    ['Деньги' 'Дети' 'Женская психология' 'Женщины' 'Здоровье' 'Знаменитости'
     'Интересные факты' 'Искусство' 'История' 'Красота' 'Культура' 'Наука'
     'Общество' 'Отношения' 'Подборки' 'Полезные советы' 'Психология'
     'Путешествия' 'Рассказы' 'Россия' 'Семья' 'Скандалы' 'Туризм' 'Шоу'
     'Юмор']
    
    ['Авто' 'Деньги' 'Дети' 'Еда' 'Здоровье' 'Знаменитости' 'Интерьеры'
     'Искусство' 'История' 'Кино' 'Музыка' 'Одежда' 'Полезные советы'
     'Политика' 'Психология' 'Путешествия' 'Ремонт' 'Россия' 'Сад и дача'
     'Сделай сам' 'Семейные отношения' 'Семья' 'Спорт' 'Строительство'
     'Технологии' 'Финансы']
    
    ['18-25' '26-30' '31-35' '36-40' '41-45' '45+']
    
    ['2019-09-24T18:32:00.000000000' '2019-09-24T18:35:00.000000000'
     '2019-09-24T18:54:00.000000000' '2019-09-24T18:55:00.000000000'
     '2019-09-24T18:56:00.000000000' '2019-09-24T18:57:00.000000000'
     '2019-09-24T18:58:00.000000000' '2019-09-24T18:59:00.000000000'
     '2019-09-24T19:00:00.000000000' '2019-09-24T18:29:00.000000000'
     '2019-09-24T18:30:00.000000000' '2019-09-24T18:31:00.000000000'
     '2019-09-24T18:52:00.000000000' '2019-09-24T18:33:00.000000000'
     '2019-09-24T18:53:00.000000000' '2019-09-24T18:28:00.000000000'
     '2019-09-24T18:34:00.000000000']


Посмотрим на распределение значений.


```python
df['visits'].describe()
```




    count    30745.000000
    mean        10.089673
    std         19.727601
    min          1.000000
    25%          1.000000
    50%          3.000000
    75%         10.000000
    max        371.000000
    Name: visits, dtype: float64




```python
plt.figure(figsize=(17, 12))
ax = sns.boxplot(data=df, x='visits', y='item_topic', palette="Set3")
plt.title('Ящик с усами распределения количества событий по темам карточек')
plt.xlabel('Количество событий')
plt.ylabel('Темы карточек')
plt.show()
```


    
![png](output_17_0.png)
    



```python
plt.figure(figsize=(17, 12))
ax = sns.boxplot(data=df, x='visits', y='item_topic', palette="Set3")
ax.set_xlim(-1,50)
plt.title('Ящик с усами распределения количества событий по темам карточек')
plt.xlabel('Количество событий')
plt.ylabel('Темы карточек')
plt.show()
```


    
![png](output_18_0.png)
    


Присутвуют выбросы, но большая часть значений расположено от 1 до 20.  
**Вывод:** Будем считать данные корректными, оставим все без изменения.

## Экспорт данных в файл
[Содержание](#intro)

Выгрузим в данные в csv-файл


```python
df.to_csv('dash_visits.csv', index = False, sep = '\t')
```

Проверим таблицу


```python
visits = pd.read_csv('dash_visits.csv', sep = '\t')
visits
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
      <th>record_id</th>
      <th>item_topic</th>
      <th>source_topic</th>
      <th>age_segment</th>
      <th>dt</th>
      <th>visits</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>1040597</td>
      <td>Деньги</td>
      <td>Авто</td>
      <td>18-25</td>
      <td>2019-09-24 18:32:00</td>
      <td>3</td>
    </tr>
    <tr>
      <th>1</th>
      <td>1040598</td>
      <td>Деньги</td>
      <td>Авто</td>
      <td>18-25</td>
      <td>2019-09-24 18:35:00</td>
      <td>1</td>
    </tr>
    <tr>
      <th>2</th>
      <td>1040599</td>
      <td>Деньги</td>
      <td>Авто</td>
      <td>18-25</td>
      <td>2019-09-24 18:54:00</td>
      <td>4</td>
    </tr>
    <tr>
      <th>3</th>
      <td>1040600</td>
      <td>Деньги</td>
      <td>Авто</td>
      <td>18-25</td>
      <td>2019-09-24 18:55:00</td>
      <td>17</td>
    </tr>
    <tr>
      <th>4</th>
      <td>1040601</td>
      <td>Деньги</td>
      <td>Авто</td>
      <td>18-25</td>
      <td>2019-09-24 18:56:00</td>
      <td>27</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>30740</th>
      <td>1071337</td>
      <td>Юмор</td>
      <td>Финансы</td>
      <td>36-40</td>
      <td>2019-09-24 18:57:00</td>
      <td>2</td>
    </tr>
    <tr>
      <th>30741</th>
      <td>1071338</td>
      <td>Юмор</td>
      <td>Финансы</td>
      <td>36-40</td>
      <td>2019-09-24 19:00:00</td>
      <td>1</td>
    </tr>
    <tr>
      <th>30742</th>
      <td>1071339</td>
      <td>Юмор</td>
      <td>Финансы</td>
      <td>41-45</td>
      <td>2019-09-24 18:54:00</td>
      <td>1</td>
    </tr>
    <tr>
      <th>30743</th>
      <td>1071340</td>
      <td>Юмор</td>
      <td>Финансы</td>
      <td>41-45</td>
      <td>2019-09-24 18:56:00</td>
      <td>1</td>
    </tr>
    <tr>
      <th>30744</th>
      <td>1071341</td>
      <td>Юмор</td>
      <td>Финансы</td>
      <td>41-45</td>
      <td>2019-09-24 19:00:00</td>
      <td>1</td>
    </tr>
  </tbody>
</table>
<p>30745 rows × 6 columns</p>
</div>



Экспорт прошел успешно.

## Дашборд и презентация
[Содержание](#intro)

Ссылка на дашборд - https://public.tableau.com/app/profile/roman.bykov/viz/da_practicum_project_dash/Dashboard1

Ссылка на презентацию - https://disk.yandex.ru/i/gbqUTtxLsqweoA
