# Проверка гипотез для увеличения выручки интернет-магазина. А/В тесты.

Вместе с отделом маркетинга подготовилен список гипотез для увеличения выручки. Необходимо приоритизировать гипотезы, запустить A/B-тест и проанализировать результаты.
<a id='intro'></a>

<h1>Содержание<span class="tocSkip"></span></h1>
<div class="toc"><ul class="toc-item"><li><span><a href="#Описание-данных:" data-toc-modified-id="Описание-данных:-1">Описание данных:</a></span></li><li><span><a href="#Часть-1.-Приоритизация-гипотез." data-toc-modified-id="Часть-1.-Приоритизация-гипотез.-2">Часть 1. Приоритизация гипотез.</a></span><ul class="toc-item"><li><span><a href="#Предобработка-данных" data-toc-modified-id="Предобработка-данных-2.1">Предобработка данных</a></span></li><li><span><a href="#Фреймворк-ICE" data-toc-modified-id="Фреймворк-ICE-2.2">Фреймворк ICE</a></span></li><li><span><a href="#Фреймворк-RICE" data-toc-modified-id="Фреймворк-RICE-2.3">Фреймворк RICE</a></span></li></ul></li><li><span><a href="#Часть-2.-Анализ-A/B-теста" data-toc-modified-id="Часть-2.-Анализ-A/B-теста-3">Часть 2. Анализ A/B-теста</a></span><ul class="toc-item"><li><span><a href="#Предобработка-данных" data-toc-modified-id="Предобработка-данных-3.1">Предобработка данных</a></span><ul class="toc-item"><li><span><a href="#Табллица-orders" data-toc-modified-id="Табллица-orders-3.1.1">Табллица <code>orders</code></a></span></li><li><span><a href="#Таблица-visitors" data-toc-modified-id="Таблица-visitors-3.1.2">Таблица <code>visitors</code></a></span></li></ul></li><li><span><a href="#Анализ-A/B-теста" data-toc-modified-id="Анализ-A/B-теста-3.2">Анализ A/B-теста</a></span><ul class="toc-item"><li><span><a href="#График-кумулятивной-выручки-по-группам" data-toc-modified-id="График-кумулятивной-выручки-по-группам-3.2.1">График кумулятивной выручки по группам</a></span></li><li><span><a href="#График-кумулятивного-среднего-чека-по-группам" data-toc-modified-id="График-кумулятивного-среднего-чека-по-группам-3.2.2">График кумулятивного среднего чека по группам</a></span></li><li><span><a href="#График-относительного-изменения-кумулятивного-среднего-чека-группы-B-к-группе-A" data-toc-modified-id="График-относительного-изменения-кумулятивного-среднего-чека-группы-B-к-группе-A-3.2.3">График относительного изменения кумулятивного среднего чека группы B к группе A</a></span></li><li><span><a href="#График-кумулятивной-конверсии-по-группам" data-toc-modified-id="График-кумулятивной-конверсии-по-группам-3.2.4">График кумулятивной конверсии по группам</a></span></li><li><span><a href="#График-относительного-изменения-кумулятивной-конверсии-группы-B-к-группе-A" data-toc-modified-id="График-относительного-изменения-кумулятивной-конверсии-группы-B-к-группе-A-3.2.5">График относительного изменения кумулятивной конверсии группы B к группе A</a></span></li><li><span><a href="#Точечный-график-количества-заказов-по-пользователям" data-toc-modified-id="Точечный-график-количества-заказов-по-пользователям-3.2.6">Точечный график количества заказов по пользователям</a></span></li><li><span><a href="#95-й-и-99-й-перцентили-количества-заказов-на-пользователя" data-toc-modified-id="95-й-и-99-й-перцентили-количества-заказов-на-пользователя-3.2.7">95-й и 99-й перцентили количества заказов на пользователя</a></span></li><li><span><a href="#Точечный-график-стоимостей-заказов" data-toc-modified-id="Точечный-график-стоимостей-заказов-3.2.8">Точечный график стоимостей заказов</a></span></li><li><span><a href="#95-й-и-99-й-перцентили-стоимости-заказов" data-toc-modified-id="95-й-и-99-й-перцентили-стоимости-заказов-3.2.9">95-й и 99-й перцентили стоимости заказов</a></span></li><li><span><a href="#Cтатистическая-значимость-различий-в-конверсии-между-группами-по-«сырым»-данным." data-toc-modified-id="Cтатистическая-значимость-различий-в-конверсии-между-группами-по-«сырым»-данным.-3.2.10">Cтатистическая значимость различий в конверсии между группами по «сырым» данным.</a></span></li><li><span><a href="#Статистическая-значимость-различий-в-среднем-чеке-заказа-между-группами-по-«сырым»-данным" data-toc-modified-id="Статистическая-значимость-различий-в-среднем-чеке-заказа-между-группами-по-«сырым»-данным-3.2.11">Статистическая значимость различий в среднем чеке заказа между группами по «сырым» данным</a></span></li><li><span><a href="#Cтатистическая-значимость-различий-в-конверсии-между-группами-по-«очищенным»-данным" data-toc-modified-id="Cтатистическая-значимость-различий-в-конверсии-между-группами-по-«очищенным»-данным-3.2.12">Cтатистическая значимость различий в конверсии между группами по «очищенным» данным</a></span></li><li><span><a href="#Cтатистическая-значимость-различий-в-среднем-чеке-заказа-между-группами-по-«очищенным»-данным" data-toc-modified-id="Cтатистическая-значимость-различий-в-среднем-чеке-заказа-между-группами-по-«очищенным»-данным-3.2.13">Cтатистическая значимость различий в среднем чеке заказа между группами по «очищенным» данным</a></span></li></ul></li><li><span><a href="#Решение-по-результатам-теста" data-toc-modified-id="Решение-по-результатам-теста-3.3">Решение по результатам теста</a></span></li></ul></li></ul></div>

## Описание данных:
[Содержание](#intro)

**Данные для первой части**  
**Файл `hypothesis.csv`**  
`Hypothesis` — краткое описание гипотезы;  
`Reach` — охват пользователей по 10-балльной шкале;  
`Impact` — влияние на пользователей по 10-балльной шкале;  
`Confidence` — уверенность в гипотезе по 10-балльной шкале;  
`Efforts` — затраты ресурсов на проверку гипотезы по 10-балльной шкале. Чем больше значение Efforts, тем дороже проверка гипотезы.  

**Данные для второй части**  
**Файл `orders.csv`**  
`transactionId` — идентификатор заказа;  
`visitorId` — идентификатор пользователя, совершившего заказ;  
`date` — дата, когда был совершён заказ;  
`revenue` — выручка заказа;  
`group` — группа A/B-теста, в которую попал заказ.  
**Файл `visitors.csv`**  
`date` — дата;  
`group` — группа A/B-теста;  
`visitors` — количество пользователей в указанную дату в указанной группе A/B-теста  


**Загрузим необходимы библиотеки:**


```python
import pandas as pd
import datetime as dt
import numpy as np
import matplotlib.pyplot as plt
from pandas.plotting import register_matplotlib_converters
import warnings
register_matplotlib_converters()
import scipy.stats as stats
pd.set_option('display.max_colwidth', 0)
```

## Часть 1. Приоритизация гипотез.

### Предобработка данных
[Содержание](#intro)

**Загрузим таблицу с гипотезами и посмотрим содержимое**


```python
data = pd.read_csv('/datasets/hypothesis.csv')
data
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
      <th>Hypothesis</th>
      <th>Reach</th>
      <th>Impact</th>
      <th>Confidence</th>
      <th>Efforts</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Добавить два новых канала привлечения трафика, что позволит привлекать на 30% больше пользователей</td>
      <td>3</td>
      <td>10</td>
      <td>8</td>
      <td>6</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Запустить собственную службу доставки, что сократит срок доставки заказов</td>
      <td>2</td>
      <td>5</td>
      <td>4</td>
      <td>10</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Добавить блоки рекомендаций товаров на сайт интернет магазина, чтобы повысить конверсию и средний чек заказа</td>
      <td>8</td>
      <td>3</td>
      <td>7</td>
      <td>3</td>
    </tr>
    <tr>
      <th>3</th>
      <td>Изменить структура категорий, что увеличит конверсию, т.к. пользователи быстрее найдут нужный товар</td>
      <td>8</td>
      <td>3</td>
      <td>3</td>
      <td>8</td>
    </tr>
    <tr>
      <th>4</th>
      <td>Изменить цвет фона главной страницы, чтобы увеличить вовлеченность пользователей</td>
      <td>3</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
    </tr>
    <tr>
      <th>5</th>
      <td>Добавить страницу отзывов клиентов о магазине, что позволит увеличить количество заказов</td>
      <td>3</td>
      <td>2</td>
      <td>2</td>
      <td>3</td>
    </tr>
    <tr>
      <th>6</th>
      <td>Показать на главной странице баннеры с актуальными акциями и распродажами, чтобы увеличить конверсию</td>
      <td>5</td>
      <td>3</td>
      <td>8</td>
      <td>3</td>
    </tr>
    <tr>
      <th>7</th>
      <td>Добавить форму подписки на все основные страницы, чтобы собрать базу клиентов для email-рассылок</td>
      <td>10</td>
      <td>7</td>
      <td>8</td>
      <td>5</td>
    </tr>
    <tr>
      <th>8</th>
      <td>Запустить акцию, дающую скидку на товар в день рождения</td>
      <td>1</td>
      <td>9</td>
      <td>9</td>
      <td>5</td>
    </tr>
  </tbody>
</table>
</div>



Приведем столбцы к нижнему регистру


```python
data.columns = ['hypothesis', 'reach', 'impact', 'confidence', 'efforts']
data
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
      <th>hypothesis</th>
      <th>reach</th>
      <th>impact</th>
      <th>confidence</th>
      <th>efforts</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Добавить два новых канала привлечения трафика, что позволит привлекать на 30% больше пользователей</td>
      <td>3</td>
      <td>10</td>
      <td>8</td>
      <td>6</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Запустить собственную службу доставки, что сократит срок доставки заказов</td>
      <td>2</td>
      <td>5</td>
      <td>4</td>
      <td>10</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Добавить блоки рекомендаций товаров на сайт интернет магазина, чтобы повысить конверсию и средний чек заказа</td>
      <td>8</td>
      <td>3</td>
      <td>7</td>
      <td>3</td>
    </tr>
    <tr>
      <th>3</th>
      <td>Изменить структура категорий, что увеличит конверсию, т.к. пользователи быстрее найдут нужный товар</td>
      <td>8</td>
      <td>3</td>
      <td>3</td>
      <td>8</td>
    </tr>
    <tr>
      <th>4</th>
      <td>Изменить цвет фона главной страницы, чтобы увеличить вовлеченность пользователей</td>
      <td>3</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
    </tr>
    <tr>
      <th>5</th>
      <td>Добавить страницу отзывов клиентов о магазине, что позволит увеличить количество заказов</td>
      <td>3</td>
      <td>2</td>
      <td>2</td>
      <td>3</td>
    </tr>
    <tr>
      <th>6</th>
      <td>Показать на главной странице баннеры с актуальными акциями и распродажами, чтобы увеличить конверсию</td>
      <td>5</td>
      <td>3</td>
      <td>8</td>
      <td>3</td>
    </tr>
    <tr>
      <th>7</th>
      <td>Добавить форму подписки на все основные страницы, чтобы собрать базу клиентов для email-рассылок</td>
      <td>10</td>
      <td>7</td>
      <td>8</td>
      <td>5</td>
    </tr>
    <tr>
      <th>8</th>
      <td>Запустить акцию, дающую скидку на товар в день рождения</td>
      <td>1</td>
      <td>9</td>
      <td>9</td>
      <td>5</td>
    </tr>
  </tbody>
</table>
</div>



### Фреймворк ICE
[Содержание](#intro)

**Применим фреймворк ICE для приоритизации гипотез. Отсортируем их по убыванию приоритета.**


```python
data['ICE'] = data['impact'] * data['confidence'] / data['efforts']
data_ice = data[['hypothesis','ICE']].sort_values('ICE', ascending=False).round(2)
data_ice
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
      <th>hypothesis</th>
      <th>ICE</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>8</th>
      <td>Запустить акцию, дающую скидку на товар в день рождения</td>
      <td>16.20</td>
    </tr>
    <tr>
      <th>0</th>
      <td>Добавить два новых канала привлечения трафика, что позволит привлекать на 30% больше пользователей</td>
      <td>13.33</td>
    </tr>
    <tr>
      <th>7</th>
      <td>Добавить форму подписки на все основные страницы, чтобы собрать базу клиентов для email-рассылок</td>
      <td>11.20</td>
    </tr>
    <tr>
      <th>6</th>
      <td>Показать на главной странице баннеры с актуальными акциями и распродажами, чтобы увеличить конверсию</td>
      <td>8.00</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Добавить блоки рекомендаций товаров на сайт интернет магазина, чтобы повысить конверсию и средний чек заказа</td>
      <td>7.00</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Запустить собственную службу доставки, что сократит срок доставки заказов</td>
      <td>2.00</td>
    </tr>
    <tr>
      <th>5</th>
      <td>Добавить страницу отзывов клиентов о магазине, что позволит увеличить количество заказов</td>
      <td>1.33</td>
    </tr>
    <tr>
      <th>3</th>
      <td>Изменить структура категорий, что увеличит конверсию, т.к. пользователи быстрее найдут нужный товар</td>
      <td>1.12</td>
    </tr>
    <tr>
      <th>4</th>
      <td>Изменить цвет фона главной страницы, чтобы увеличить вовлеченность пользователей</td>
      <td>1.00</td>
    </tr>
  </tbody>
</table>
</div>



Наиболее перспективные гипотезы: 8, 0, 7

### Фреймворк RICE
[Содержание](#intro)

**Применим фреймворк RICE для приоритизации гипотез. Отсортируем их по убыванию приоритета.**


```python
data['RICE'] = data['reach'] * data['impact'] * data['confidence'] / data['efforts']
data_rice = data[['hypothesis','RICE']].sort_values('RICE', ascending=False).round(2)
data_rice
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
      <th>hypothesis</th>
      <th>RICE</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>7</th>
      <td>Добавить форму подписки на все основные страницы, чтобы собрать базу клиентов для email-рассылок</td>
      <td>112.0</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Добавить блоки рекомендаций товаров на сайт интернет магазина, чтобы повысить конверсию и средний чек заказа</td>
      <td>56.0</td>
    </tr>
    <tr>
      <th>0</th>
      <td>Добавить два новых канала привлечения трафика, что позволит привлекать на 30% больше пользователей</td>
      <td>40.0</td>
    </tr>
    <tr>
      <th>6</th>
      <td>Показать на главной странице баннеры с актуальными акциями и распродажами, чтобы увеличить конверсию</td>
      <td>40.0</td>
    </tr>
    <tr>
      <th>8</th>
      <td>Запустить акцию, дающую скидку на товар в день рождения</td>
      <td>16.2</td>
    </tr>
    <tr>
      <th>3</th>
      <td>Изменить структура категорий, что увеличит конверсию, т.к. пользователи быстрее найдут нужный товар</td>
      <td>9.0</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Запустить собственную службу доставки, что сократит срок доставки заказов</td>
      <td>4.0</td>
    </tr>
    <tr>
      <th>5</th>
      <td>Добавить страницу отзывов клиентов о магазине, что позволит увеличить количество заказов</td>
      <td>4.0</td>
    </tr>
    <tr>
      <th>4</th>
      <td>Изменить цвет фона главной страницы, чтобы увеличить вовлеченность пользователей</td>
      <td>3.0</td>
    </tr>
  </tbody>
</table>
</div>



Здесь уже порядок изменился, наиболее перспективными уже будут: 7, 2, 0.

Это связано с тем, что при расчете RICE учитываем как много пользователей затронет гипотеза.


```python
data_hypothesis = data_ice.merge(data_rice, on='hypothesis')
data_hypothesis
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
      <th>hypothesis</th>
      <th>ICE</th>
      <th>RICE</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Запустить акцию, дающую скидку на товар в день рождения</td>
      <td>16.20</td>
      <td>16.2</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Добавить два новых канала привлечения трафика, что позволит привлекать на 30% больше пользователей</td>
      <td>13.33</td>
      <td>40.0</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Добавить форму подписки на все основные страницы, чтобы собрать базу клиентов для email-рассылок</td>
      <td>11.20</td>
      <td>112.0</td>
    </tr>
    <tr>
      <th>3</th>
      <td>Показать на главной странице баннеры с актуальными акциями и распродажами, чтобы увеличить конверсию</td>
      <td>8.00</td>
      <td>40.0</td>
    </tr>
    <tr>
      <th>4</th>
      <td>Добавить блоки рекомендаций товаров на сайт интернет магазина, чтобы повысить конверсию и средний чек заказа</td>
      <td>7.00</td>
      <td>56.0</td>
    </tr>
    <tr>
      <th>5</th>
      <td>Запустить собственную службу доставки, что сократит срок доставки заказов</td>
      <td>2.00</td>
      <td>4.0</td>
    </tr>
    <tr>
      <th>6</th>
      <td>Добавить страницу отзывов клиентов о магазине, что позволит увеличить количество заказов</td>
      <td>1.33</td>
      <td>4.0</td>
    </tr>
    <tr>
      <th>7</th>
      <td>Изменить структура категорий, что увеличит конверсию, т.к. пользователи быстрее найдут нужный товар</td>
      <td>1.12</td>
      <td>9.0</td>
    </tr>
    <tr>
      <th>8</th>
      <td>Изменить цвет фона главной страницы, чтобы увеличить вовлеченность пользователей</td>
      <td>1.00</td>
      <td>3.0</td>
    </tr>
  </tbody>
</table>
</div>



Параметр Reach изменил всю приоритетность, полученную фреймворком ICE.  
Гипотезы 6 и 4 остались на прежних местах.

## Часть 2. Анализ A/B-теста

### Предобработка данных
[Содержание](#intro)

Загрузим и посмотрим данные 
#### Табллица `orders`


```python
orders = pd.read_csv('/datasets/orders.csv')
display(orders.info())
display(orders.head())
print()
print('Число дубликатов:', orders.duplicated().sum())
```

    <class 'pandas.core.frame.DataFrame'>
    RangeIndex: 1197 entries, 0 to 1196
    Data columns (total 5 columns):
     #   Column         Non-Null Count  Dtype 
    ---  ------         --------------  ----- 
     0   transactionId  1197 non-null   int64 
     1   visitorId      1197 non-null   int64 
     2   date           1197 non-null   object
     3   revenue        1197 non-null   int64 
     4   group          1197 non-null   object
    dtypes: int64(3), object(2)
    memory usage: 46.9+ KB



    None



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
      <th>transactionId</th>
      <th>visitorId</th>
      <th>date</th>
      <th>revenue</th>
      <th>group</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>3667963787</td>
      <td>3312258926</td>
      <td>2019-08-15</td>
      <td>1650</td>
      <td>B</td>
    </tr>
    <tr>
      <th>1</th>
      <td>2804400009</td>
      <td>3642806036</td>
      <td>2019-08-15</td>
      <td>730</td>
      <td>B</td>
    </tr>
    <tr>
      <th>2</th>
      <td>2961555356</td>
      <td>4069496402</td>
      <td>2019-08-15</td>
      <td>400</td>
      <td>A</td>
    </tr>
    <tr>
      <th>3</th>
      <td>3797467345</td>
      <td>1196621759</td>
      <td>2019-08-15</td>
      <td>9759</td>
      <td>B</td>
    </tr>
    <tr>
      <th>4</th>
      <td>2282983706</td>
      <td>2322279887</td>
      <td>2019-08-15</td>
      <td>2308</td>
      <td>B</td>
    </tr>
  </tbody>
</table>
</div>


    
    Число дубликатов: 0


Пропусков не обнаружено.  
Дубликатов нет.  
В столбце с датами задан неверный тип данных - исправим.  
Исправим названия столбцов.


```python
orders.columns = ['transaction_id', 'visitor_id', 'date', 'revenue', 'group']
orders['date'] = orders['date'].map(
    lambda x: dt.datetime.strptime(x, '%Y-%m-%d')
)
display(orders.info())
display(orders.head())
```

    <class 'pandas.core.frame.DataFrame'>
    RangeIndex: 1197 entries, 0 to 1196
    Data columns (total 5 columns):
     #   Column          Non-Null Count  Dtype         
    ---  ------          --------------  -----         
     0   transaction_id  1197 non-null   int64         
     1   visitor_id      1197 non-null   int64         
     2   date            1197 non-null   datetime64[ns]
     3   revenue         1197 non-null   int64         
     4   group           1197 non-null   object        
    dtypes: datetime64[ns](1), int64(3), object(1)
    memory usage: 46.9+ KB



    None



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
      <th>transaction_id</th>
      <th>visitor_id</th>
      <th>date</th>
      <th>revenue</th>
      <th>group</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>3667963787</td>
      <td>3312258926</td>
      <td>2019-08-15</td>
      <td>1650</td>
      <td>B</td>
    </tr>
    <tr>
      <th>1</th>
      <td>2804400009</td>
      <td>3642806036</td>
      <td>2019-08-15</td>
      <td>730</td>
      <td>B</td>
    </tr>
    <tr>
      <th>2</th>
      <td>2961555356</td>
      <td>4069496402</td>
      <td>2019-08-15</td>
      <td>400</td>
      <td>A</td>
    </tr>
    <tr>
      <th>3</th>
      <td>3797467345</td>
      <td>1196621759</td>
      <td>2019-08-15</td>
      <td>9759</td>
      <td>B</td>
    </tr>
    <tr>
      <th>4</th>
      <td>2282983706</td>
      <td>2322279887</td>
      <td>2019-08-15</td>
      <td>2308</td>
      <td>B</td>
    </tr>
  </tbody>
</table>
</div>


#### Таблица `visitors`
[Содержание](#intro)


```python
visitors = pd.read_csv('/datasets/visitors.csv')
display(visitors.info())
display(visitors)
print()
print('Число дубликатов:', orders.duplicated().sum())
```

    <class 'pandas.core.frame.DataFrame'>
    RangeIndex: 62 entries, 0 to 61
    Data columns (total 3 columns):
     #   Column    Non-Null Count  Dtype 
    ---  ------    --------------  ----- 
     0   date      62 non-null     object
     1   group     62 non-null     object
     2   visitors  62 non-null     int64 
    dtypes: int64(1), object(2)
    memory usage: 1.6+ KB



    None



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
      <th>date</th>
      <th>group</th>
      <th>visitors</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>2019-08-01</td>
      <td>A</td>
      <td>719</td>
    </tr>
    <tr>
      <th>1</th>
      <td>2019-08-02</td>
      <td>A</td>
      <td>619</td>
    </tr>
    <tr>
      <th>2</th>
      <td>2019-08-03</td>
      <td>A</td>
      <td>507</td>
    </tr>
    <tr>
      <th>3</th>
      <td>2019-08-04</td>
      <td>A</td>
      <td>717</td>
    </tr>
    <tr>
      <th>4</th>
      <td>2019-08-05</td>
      <td>A</td>
      <td>756</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>57</th>
      <td>2019-08-27</td>
      <td>B</td>
      <td>720</td>
    </tr>
    <tr>
      <th>58</th>
      <td>2019-08-28</td>
      <td>B</td>
      <td>654</td>
    </tr>
    <tr>
      <th>59</th>
      <td>2019-08-29</td>
      <td>B</td>
      <td>531</td>
    </tr>
    <tr>
      <th>60</th>
      <td>2019-08-30</td>
      <td>B</td>
      <td>490</td>
    </tr>
    <tr>
      <th>61</th>
      <td>2019-08-31</td>
      <td>B</td>
      <td>718</td>
    </tr>
  </tbody>
</table>
<p>62 rows × 3 columns</p>
</div>


    
    Число дубликатов: 0


Пропусков не обнаружено.
Дубликатов нет.
Исправим тип данных в столбце с датами.


```python
visitors['date'] = visitors['date'].map(
    lambda x: dt.datetime.strptime(x, '%Y-%m-%d')
)
display(visitors.info())
```

    <class 'pandas.core.frame.DataFrame'>
    RangeIndex: 62 entries, 0 to 61
    Data columns (total 3 columns):
     #   Column    Non-Null Count  Dtype         
    ---  ------    --------------  -----         
     0   date      62 non-null     datetime64[ns]
     1   group     62 non-null     object        
     2   visitors  62 non-null     int64         
    dtypes: datetime64[ns](1), int64(1), object(1)
    memory usage: 1.6+ KB



    None


**Посмотрим на количество груп:**


```python
visitors['group'].value_counts()
```




    A    31
    B    31
    Name: group, dtype: int64



**Проверим группы на совпадения пользователей**


```python
g_a = orders[orders['group'] == 'A']['visitor_id']
g_b = orders[orders['group'] == 'B']['visitor_id']
orders_ab = orders.query('visitor_id in @g_a and visitor_id in @g_b')
display(orders_ab['visitor_id'].unique())
print('Количество пользователей в двух группах:', orders_ab['visitor_id'].nunique())
print('Всего пользователей в тесте:', orders['visitor_id'].nunique())
```


    array([4069496402,  963407295,  351125977, 3234906277,  199603092,
            237748145, 3803269165, 2038680547, 2378935119, 4256040402,
           2712142231,    8300375,  276558944,  457167155, 3062433592,
           1738359350, 2458001652, 2716752286, 3891541246, 1648269707,
           3656415546, 2686716486, 2954449915, 2927087541, 2579882178,
           3957174400, 2780786433, 3984495233,  818047933, 1668030113,
           3717692402, 2044997962, 1959144690, 1294878855, 1404934699,
           2587333274, 3202540741, 1333886533, 2600415354, 3951559397,
            393266494, 3972127743, 4120364173, 4266935830, 1230306981,
           1614305549,  477780734, 1602967004, 1801183820, 4186807279,
           3766097110, 3941795274,  471551937, 1316129916,  232979603,
           2654030115, 3963646447, 2949041841])


    Количество пользователей в двух группах: 58
    Всего пользователей в тесте: 1031



```python
orders_ab['revenue'].describe()
```




    count    181.000000  
    mean     8612.900552 
    std      14161.550845
    min      50.000000   
    25%      1530.000000 
    50%      3460.000000 
    75%      8439.000000 
    max      93940.000000
    Name: revenue, dtype: float64




```python
# таблица с пользователями и суммой их заказов
#orders_ab.groupby('visitor_id').agg({'transaction_id':'count', 'revenue':'sum'}).sort_values('revenue', ascending=False)
```

Имеем 58 пользователей который одновременно присутствуют в двух группах. Всего они сделали 181 заказ со средней суммой заказа 8612 (причем медианное значение 3460) и максимальной 93940.  
Всего в таблице 1031 пользоватей. Наши пользователи из 2 групп составляют 5,6% от общего числа.  
Так как их процент от общего числа не велик, очистим таблицу от этих пользователей, для корретного А/В теста.


```python
orders = orders.query('visitor_id not in @orders_ab["visitor_id"]')
print('Всего пользователей в тесте осталось:', orders['visitor_id'].nunique())
```

    Всего пользователей в тесте осталось: 973


**Вывод:**  
В таблицах был задан некорректный тип данных для столбца с датами - исправлено.  
Пропусков и явных дубликатов не обнаружено.  
Очистили таблицу от пользователей, которые были в двух группах одновременно.

### Анализ A/B-теста

#### График кумулятивной выручки по группам
[Содержание](#intro)

Соберем данные.  
1) Создим датафрейм `datesGroups` с уникальными парами значений 'date' и 'group', таблицы `orders`.   
2) Объявим переменную `ordersAggregated`, содержащую:  
дату;  
группу A/B-теста;  
число уникальных заказов в группе теста по указанную дату включительно;  
число уникальных пользователей, совершивших хотя бы 1 заказ в группе теста по указанную дату включительно;  
суммарную выручку заказов в группе теста по указанную дату включительно.  
3) Объявим переменную `visitorsAggregated`, содержащую:  
дату;  
группу A/B-теста;  
количество уникальных посетителей в группе теста по указанную дату включительно.  
4) `ordersAggregated` и `visitorsAggregated` отсортируем по столбцам 'date', 'group'.  
5) Определим переменную `cumulativeData`, объединив `ordersAggregated` и `visitorsAggregated` по колонкам 'date', 'group'  
6) Присвоим столбцам `cumulativeData` названия.


```python
datesGroups = orders[['date','group']].drop_duplicates()

ordersAggregated = datesGroups.apply(
    lambda x: orders[np.logical_and(orders['date'] <= x['date'], orders['group'] == x['group'])].agg({
'date' : 'max',
'group' : 'max',
'transaction_id' : pd.Series.nunique,
'visitor_id' : pd.Series.nunique,
'revenue' : 'sum'}), axis=1).sort_values(by=['date','group'])

visitorsAggregated = datesGroups.apply(
    lambda x: visitors[np.logical_and(visitors['date'] <= x['date'], visitors['group'] == x['group'])].agg({
'date' : 'max',
'group' : 'max',
'visitors' : 'sum'}), axis=1).sort_values(by=['date','group'])

cumulativeData = ordersAggregated.merge(visitorsAggregated, left_on=['date', 'group'], right_on=['date', 'group'])

cumulativeData.columns = ['date', 'group', 'orders', 'buyers', 'revenue', 'visitors']

display(cumulativeData.head())
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
      <th>date</th>
      <th>group</th>
      <th>orders</th>
      <th>buyers</th>
      <th>revenue</th>
      <th>visitors</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>2019-08-01</td>
      <td>A</td>
      <td>23</td>
      <td>19</td>
      <td>142779</td>
      <td>719</td>
    </tr>
    <tr>
      <th>1</th>
      <td>2019-08-01</td>
      <td>B</td>
      <td>17</td>
      <td>17</td>
      <td>59758</td>
      <td>713</td>
    </tr>
    <tr>
      <th>2</th>
      <td>2019-08-02</td>
      <td>A</td>
      <td>42</td>
      <td>36</td>
      <td>234381</td>
      <td>1338</td>
    </tr>
    <tr>
      <th>3</th>
      <td>2019-08-02</td>
      <td>B</td>
      <td>40</td>
      <td>39</td>
      <td>221801</td>
      <td>1294</td>
    </tr>
    <tr>
      <th>4</th>
      <td>2019-08-03</td>
      <td>A</td>
      <td>66</td>
      <td>60</td>
      <td>346854</td>
      <td>1845</td>
    </tr>
  </tbody>
</table>
</div>


Объявим переменные `cumulativeRevenueA` и `cumulativeRevenueB`, в которых сохраним данные о датах, выручке и числе заказов в группах A и B.
Построим график.


```python
cumulativeRevenueA = cumulativeData[cumulativeData['group']=='A'][['date','revenue', 'orders']]
cumulativeRevenueB = cumulativeData[cumulativeData['group']=='B'][['date','revenue', 'orders']]

plt.figure(figsize=(15,5))
plt.plot(cumulativeRevenueA['date'], cumulativeRevenueA['revenue'], label='A')
plt.plot(cumulativeRevenueB['date'], cumulativeRevenueB['revenue'], label='B')

plt.title('График кумулятивной выручки по группам')
plt.ylabel('Выручка')
plt.xticks(rotation=45)
plt.legend()
plt.grid()
plt.show()
```


    
![png](output_42_0.png)
    


**Вывод:**  
В первой половине месяца мы видим, что выручка в группе "B" растёт немного быстрее, чем в "A", а 13 августа они сравниваются.  
Во второй половине месяца мы видим у группы "В" резкий скачек в районе 18 числа, что может говорить о выбросах. Далее хорошо видно, что её выручка значительно больше.  
Скорее всего сильный всплеск выручки в группе "B" 18 августа может быть связан с крупным(-ми) заказом(-ами). Если отбросить его, то по графикам можно предположить, что выручки росли примерно одинаково, но у группы "В" она все равно выше.

#### График кумулятивного среднего чека по группам
[Содержание](#intro)

Для каждой группы построим графики кумулятивного среднего чека.


```python
plt.figure(figsize=(15,5))
plt.plot(cumulativeRevenueA['date'], cumulativeRevenueA['revenue']/cumulativeRevenueA['orders'], label='A')
plt.plot(cumulativeRevenueB['date'], cumulativeRevenueB['revenue']/cumulativeRevenueB['orders'], label='B')

plt.title('График кумулятивного среднего чека по группам')
plt.ylabel('Выручка')
plt.xticks(rotation=45)
plt.legend()
plt.grid()
plt.show()
```


    
![png](output_45_0.png)
    


**Вывод:**  
Кумулятивное значение среднего чека по группам колеблется.  
Заметен резкий скачок у группы В.  
Принимать решения по данной метрике рано и требуется анализ выбросов.

На графике видим что преимущественно лидирует группа "В", изредка чек группы "А" выше.  
18.08 происходит резкий рост группы "В" который доводит отрыв от группы "А" почти до 50%, далее начинается нисходящий тренд. В следствии которого отрыв у группы "В" уменьшается примерно до 25%.  
Причиной резкого роста среднего чека может быть та же, что и у кумулятивной выручк - это крупные заказы.  
Средний чек группы "В" еще не стабилизировался и в следствии этого можно ожидать дальнейшее падение.

#### График относительного изменения кумулятивного среднего чека группы B к группе A
[Содержание](#intro)

Объединим таблицы `cumulativeRevenueA` и `cumulativeRevenueB`. Построим график относительно различия кумулятивного среднего чека группы B к группе A.


```python
mergedCumulativeRevenue = cumulativeRevenueA.merge(
    cumulativeRevenueB, left_on='date', right_on='date', how='left', suffixes=['A', 'B']
)

plt.figure(figsize=(15,5))
plt.plot(mergedCumulativeRevenue['date'],
         (mergedCumulativeRevenue['revenueB']/mergedCumulativeRevenue['ordersB'])/
         (mergedCumulativeRevenue['revenueA']/mergedCumulativeRevenue['ordersA'])-1)

plt.axhline(y=0, color='black', linestyle='--') 

plt.title('График относительного изменения кумулятивного среднего чека группы B к группе A')
plt.xticks(rotation=45)
plt.grid()
plt.show()
```


    
![png](output_48_0.png)
    


**Вывод:**  
Результаты теста значительно и резко меняются в несколько дат. Видимо, именно тогда были совершены аномальные заказы.

#### График кумулятивной конверсии по группам
[Содержание](#intro)

Добавим в `cumulativeData` столбец `'conversion'` c отношением числа заказов к количеству пользователей в указанной группе в указанный день.
Объявим переменные `cumulativeDataA` и `cumulativeDataB`, в которых сохраните данные о заказах в сегментах A и B соответственно.
Построим графики кумулятивной конверсии по дням по группам.


```python
cumulativeData['conversion'] = cumulativeData['orders']/cumulativeData['visitors']


cumulativeDataA = cumulativeData[cumulativeData['group']=='A']
cumulativeDataB = cumulativeData[cumulativeData['group']=='B']

plt.figure(figsize=(15,5))
plt.plot(cumulativeDataA['date'], cumulativeDataA['conversion'], label='A')
plt.plot(cumulativeDataB['date'], cumulativeDataB['conversion'], label='B')
plt.title('График кумулятивной конверсии по группам')
plt.ylabel('сonversion')
plt.xticks(rotation=45)
plt.legend()
plt.grid()
plt.show()
```


    
![png](output_51_0.png)
    


**Вывод:**  
В начале теста группа А имела больщую конверсию, позже снизалась и зафиксировалась на одном уровне. У группы В на графике видна обратная ситуация и значения выше сегмента А.

#### График относительного изменения кумулятивной конверсии группы B к группе A
[Содержание](#intro)

Объединим таблицы `cumulativeDataA` и `cumulativeDataB`, cохраните в переменной `mergedCumulativeConversions`.
Построим график относительного различия кумулятивной конверсии группы B к группе A.


```python
mergedCumulativeConversions = cumulativeDataA[['date','conversion']].merge(
    cumulativeDataB[['date','conversion']], left_on='date', right_on='date', how='left', suffixes=['A', 'B']
)

plt.figure(figsize=(15,5))
plt.plot(
    mergedCumulativeConversions['date'], mergedCumulativeConversions['conversionB']/
    mergedCumulativeConversions['conversionA']-1, label="Прирост конверсии группы B к группе A")

plt.legend()
plt.axhline(y=0, color='black', linestyle='--')
plt.axhline(y=0.15, color='red', linestyle='--')
plt.title('График относительного изменения кумулятивной конверсии группы B к группе A')
plt.xticks(rotation=45)
plt.grid()
plt.show()
```


    
![png](output_54_0.png)
    


**Вывод:** Конверсия группы В показывает себя намного лучше, достигая прироста 15 - 20% относительно группы А.

#### Точечный график количества заказов по пользователям
[Содержание](#intro)

Построим диаграмму


```python
#создаем таблицу по заказам
orders_by_users = orders.groupby(
    'visitor_id', as_index=False).agg({'transaction_id':'nunique'}).rename(columns={'transaction_id':'orders'}
)
orders_by_users.head()
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
      <th>visitor_id</th>
      <th>orders</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>5114589</td>
      <td>1</td>
    </tr>
    <tr>
      <th>1</th>
      <td>6958315</td>
      <td>1</td>
    </tr>
    <tr>
      <th>2</th>
      <td>11685486</td>
      <td>1</td>
    </tr>
    <tr>
      <th>3</th>
      <td>39475350</td>
      <td>1</td>
    </tr>
    <tr>
      <th>4</th>
      <td>47206413</td>
      <td>1</td>
    </tr>
  </tbody>
</table>
</div>




```python
#строим график
x_values = pd.Series(range(0,len(orders_by_users['orders'])))

plt.figure(figsize=(10,5))
plt.scatter(x_values, orders_by_users['orders'], alpha=0.3)
plt.title('Точечный график количества заказов по пользователям')
plt.xlabel('Число пользователей')
plt.ylabel('Число заказов')
plt.grid()
plt.show()
```


    
![png](output_58_0.png)
    


**Вывод:**  
По графику видно, что большая часть людей совершает покупку один раз. Много пользователей, кто совершил 2-3 покупки.

#### 95-й и 99-й перцентили количества заказов на пользователя
[Содержание](#intro)

Посчитаем 95-й и 99-й выборочные перцентили количества заказов по пользователям. Выберем границу для определения аномальных пользователей.


```python
print('90 перцентиль:', (np.percentile(orders_by_users['orders'], [90])))
print('95 перцентиль:', (np.percentile(orders_by_users['orders'], [95])))
print('99 перцентиль:', (np.percentile(orders_by_users['orders'], [99])))
```

    90 перцентиль: [1.]
    95 перцентиль: [1.]
    99 перцентиль: [2.]


Не более 1% пользователей совершили больше 2 заказов.  
Посмотрим максимальное количество заказов.


```python
orders_by_users['orders'].max()
```




    3



Разумно выбрать 2 заказа на одного пользователя за верхнюю границу числа заказов.  
Посмотрим сколько таких пользователей за этой границей:


```python
len(orders_by_users[orders_by_users['orders'] > 2]['visitor_id'])
```




    7



**Вывод:**  
Всех пользователей сделавших более 2 заказов будем считать аномальными.

#### Точечный график стоимостей заказов
[Содержание](#intro)

Построим диаграмму


```python
x_values = pd.Series(range(0,len(orders['revenue'])))

plt.figure(figsize=(10,5))
plt.scatter(x_values, orders['revenue'], alpha=0.3)
plt.title('Точечный график стоимостей заказов')
plt.xlabel('Число пользователей')
plt.ylabel('Стоимость заказов')
plt.grid()
plt.show()
```


    
![png](output_68_0.png)
    



```python
plt.figure(figsize=(10,5))
plt.scatter(x_values, orders['revenue'], alpha=0.3)
plt.ylim(0, 210000)
plt.xlabel('Число пользователей')
plt.ylabel('Стоимость заказов')
plt.grid()
plt.show()

plt.figure(figsize=(10,5))
plt.scatter(x_values, orders['revenue'], alpha=0.3)
plt.ylim(0, 100000)
plt.xlabel('Число пользователей')
plt.ylabel('Стоимость заказов')
plt.grid()
plt.show()
```


    
![png](output_69_0.png)
    



    
![png](output_69_1.png)
    


На графике явно видно 2 выброса. Данные заказы с такой большой суммой заказа могут исказить результаты. Оставим все, что ниже 10000.

#### 95-й и 99-й перцентили стоимости заказов
[Содержание](#intro)

Посчитаем 95-й и 99-й перцентили стоимости заказов. Выберем границу для определения аномальных заказов.


```python
print('90 перцентиль:', (np.percentile(orders['revenue'], [90])))
print('95 перцентиль:', (np.percentile(orders['revenue'], [95])))
print('99 перцентиль:', (np.percentile(orders['revenue'], [99])))
```

    90 перцентиль: [17990.]
    95 перцентиль: [26785.]
    99 перцентиль: [53904.]


Не более 5% пользователей совершили заказ на сумму больше 26785.  
Не более 1% пользователей совершили заказ на сумму больше 53904.
Аномальными будем считать заказы на сумму больше этого значения.

#### Cтатистическая значимость различий в конверсии между группами по «сырым» данным. 
[Содержание](#intro)

Посчитаем статистическую значимость различий в конверсии между группами по «сырым» данным.


```python
visitorsADaily = visitors[visitors['group'] == 'A'][['date', 'visitors']]
visitorsADaily.columns = ['date', 'visitorsPerDateA']

visitorsACummulative = visitorsADaily.apply(
    lambda x: visitorsADaily[visitorsADaily['date'] <= x['date']].agg(
        {'date': 'max', 'visitorsPerDateA': 'sum'}
    ),
    axis=1,
)
visitorsACummulative.columns = ['date', 'visitorsCummulativeA']

visitorsBDaily = visitors[visitors['group'] == 'B'][['date', 'visitors']]
visitorsBDaily.columns = ['date', 'visitorsPerDateB']

visitorsBCummulative = visitorsBDaily.apply(
    lambda x: visitorsBDaily[visitorsBDaily['date'] <= x['date']].agg(
        {'date': 'max', 'visitorsPerDateB': 'sum'}
    ),
    axis=1,
)
visitorsBCummulative.columns = ['date', 'visitorsCummulativeB']

ordersADaily = (
    orders[orders['group'] == 'A'][['date', 'transaction_id', 'visitor_id', 'revenue']]
    .groupby('date', as_index=False)
    .agg({'transaction_id': pd.Series.nunique, 'revenue': 'sum'})
)
ordersADaily.columns = ['date', 'ordersPerDateA', 'revenuePerDateA']

ordersACummulative = ordersADaily.apply(
    lambda x: ordersADaily[ordersADaily['date'] <= x['date']].agg(
        {'date': 'max', 'ordersPerDateA': 'sum', 'revenuePerDateA': 'sum'}
    ),
    axis=1,
).sort_values(by=['date'])
ordersACummulative.columns = [
    'date',
    'ordersCummulativeA',
    'revenueCummulativeA',
]

ordersBDaily = (
    orders[orders['group'] == 'B'][['date', 'transaction_id', 'visitor_id', 'revenue']]
    .groupby('date', as_index=False)
    .agg({'transaction_id': pd.Series.nunique, 'revenue': 'sum'})
)
ordersBDaily.columns = ['date', 'ordersPerDateB', 'revenuePerDateB']

ordersBCummulative = ordersBDaily.apply(
    lambda x: ordersBDaily[ordersBDaily['date'] <= x['date']].agg(
        {'date': 'max', 'ordersPerDateB': 'sum', 'revenuePerDateB': 'sum'}
    ),
    axis=1,
).sort_values(by=['date'])
ordersBCummulative.columns = [
    'date',
    'ordersCummulativeB',
    'revenueCummulativeB',
]

cummulative = (
    ordersADaily.merge(
        ordersBDaily, left_on='date', right_on='date', how='left'
    )
    .merge(ordersACummulative, left_on='date', right_on='date', how='left')
    .merge(ordersBCummulative, left_on='date', right_on='date', how='left')
    .merge(visitorsADaily, left_on='date', right_on='date', how='left')
    .merge(visitorsBDaily, left_on='date', right_on='date', how='left')
    .merge(visitorsACummulative, left_on='date', right_on='date', how='left')
    .merge(visitorsBCummulative, left_on='date', right_on='date', how='left')
)

ordersByUsersA = (
    orders[orders['group'] == 'A']
    .groupby('visitor_id', as_index=False)
    .agg({'transaction_id': pd.Series.nunique})
)
ordersByUsersA.columns = ['userId', 'orders']

ordersByUsersB = (
    orders[orders['group'] == 'B']
    .groupby('visitor_id', as_index=False)
    .agg({'transaction_id': pd.Series.nunique})
)
ordersByUsersB.columns = ['userId', 'orders']

sampleA = pd.concat(
    [
        ordersByUsersA['orders'],
        pd.Series(
            0,
            index=np.arange(
                cummulative['visitorsPerDateA'].sum() - len(ordersByUsersA['orders'])
            ),
            name='orders',
        ),
    ],
    axis=0,
)

sampleB = pd.concat(
    [
        ordersByUsersB['orders'],
        pd.Series(
            0,
            index=np.arange(
                cummulative['visitorsPerDateB'].sum() - len(ordersByUsersB['orders'])
            ),
            name='orders',
        ),
    ],
    axis=0,
)
```

Сформулируем гипотезы:  
Нулевая гипотеза: Конверсия в группе A равна группе B (статистическая значимость не значительна и сделать вывод о различии нельзя)  
Альтернативная гипотеза: Конверсия в группе A не равна группе B (между выборками имеется статистическая значимость)   
Применим критерием Манна-Уитни. Порогом статистической значимости установим alpha = 0.05


```python
print('{0:.3f}'.format(stats.mannwhitneyu(sampleA, sampleB)[1]))
print('{0:.3f}'.format(sampleB.mean() / sampleA.mean() - 1))
```

    0.011
    0.160


P-value значительно меньше 0.05, не получилось подтвердить нулевую гипотезу. Конверсия в группе A не равна группе B (между выборками имеется статистическая значимость).
Относительный прирост среднего группы В к конверсии группы А равен 16%.

#### Статистическая значимость различий в среднем чеке заказа между группами по «сырым» данным
[Содержание](#intro)

Посчитаем  
Сформулируем гипотезы:  
Нулевая гипотеза: Средний чек в группе A равен группе B (статистическая значимость не значительна и сделать вывод о различии нельзя)  
Альтернативная гипотеза: Средний чек в группе A не равен группе B (между выборками имеется статистическая значимость)   
Применим критерием Манна-Уитни. Порогом статистической значимости установим alpha = 0.05


```python
print('{0:.3f}'.format(stats.mannwhitneyu(orders[orders['group']=='A']['revenue'], orders[orders['group']=='B']['revenue'])[1]))
print('{0:.3f}'.format(orders[orders['group']=='B']['revenue'].mean()/orders[orders['group']=='A']['revenue'].mean()-1)) 
```

    0.829
    0.287


P-value значительно больше 0.05, не получилось отвергнуть нулевую гипотезу. Средний чек в группе A равен группе B (статистическая значимость не значительна и сделать вывод о различии нельзя).  
Относительный прирост среднего чека группы В к группе А равен 28,7%. Что было заметно на графиках ранее.

#### Cтатистическая значимость различий в конверсии между группами по «очищенным» данным
[Содержание](#intro)

Приступаем к подготовке очищенных от аномалий данных.  
Напомним, что 95-й и 99-й перцентили стоимости заказов равны 26785 и 53904 рублям. 
А 95-й и 99-й перцентили числа заказов на одного пользователя равны 1 и 2 заказам на пользователя.
Примем за аномальных пользователей тех, кто совершил более 2 заказов или совершил заказ на сумму свыше 53904 рублей. Так мы уберём 1% пользователей с наибольшим числом заказов и с наибольшей стоимостью.


```python
usersWithManyOrders = pd.concat(
    [
        #ordersByUsersA[ordersByUsersA['orders'] > 2]['userId'],
        #ordersByUsersB[ordersByUsersB['orders'] > 2]['userId'],
        ordersByUsersA[ordersByUsersA['orders'] > int(np.percentile(orders_by_users['orders'], [99]))]['userId'],
        ordersByUsersB[ordersByUsersB['orders'] > int(np.percentile(orders_by_users['orders'], [99]))]['userId'],
    ],
    axis=0,
)

usersWithExpensiveOrders = orders[orders['revenue'] > int(np.percentile(orders['revenue'], [99]))]['visitor_id']
#usersWithExpensiveOrders = orders[orders['revenue'] > 53904]['visitor_id']
abnormalUsers = (
    pd.concat([usersWithManyOrders, usersWithExpensiveOrders], axis=0)
    .drop_duplicates()
    .sort_values()
)
sampleAFiltered = pd.concat(
    [
        ordersByUsersA[
            np.logical_not(ordersByUsersA['userId'].isin(abnormalUsers))
        ]['orders'],
        pd.Series(
            0,
            index=np.arange(
                cummulative['visitorsPerDateA'].sum() - len(ordersByUsersA['orders'])
            ),
            name='orders',
        ),
    ],
    axis=0,
)

sampleBFiltered = pd.concat(
    [
        ordersByUsersB[
            np.logical_not(ordersByUsersB['userId'].isin(abnormalUsers))
        ]['orders'],
        pd.Series(
            0,
            index=np.arange(
                cummulative['visitorsPerDateB'].sum() - len(ordersByUsersB['orders'])
            ),
            name='orders',
        ),
    ],
    axis=0,
) 
```

Сформулируем гипотезы:  
Нулевая гипотеза: Конверсия в группе A равна группе B (статистическая значимость не значительна и сделать вывод о различии нельзя)  
Альтернативная гипотеза: Конверсия в группе A не равна группе B (между выборками имеется статистическая значимость)  
Применим критерием Манна-Уитни. Порогом статистической значимости установим alpha = 0.05


```python
print('{0:.3f}'.format(stats.mannwhitneyu(sampleAFiltered, sampleBFiltered)[1]))
print('{0:.3f}'.format(sampleBFiltered.mean()/sampleAFiltered.mean()-1)) 
```

    0.007
    0.189


P-value значительно меньше 0.05, не получилось подтвердить нулевую гипотезу. Конверсия в группе A не равна группе B (между выборками имеется статистическая значимость).  
Относительный прирост среднего группы В к конверсии группы А равен 18,9%. Изменение по сравнению с прошлыми результатами почти 3%.  
Подтвердилась та же гипотеза.

#### Cтатистическая значимость различий в среднем чеке заказа между группами по «очищенным» данным
[Содержание](#intro)

Сформулируем гипотезы:  
Нулевая гипотеза: Средний чек в группе A равен группе B (статистическая значимость не значительна и сделать вывод о различии нельзя)  
Альтернативная гипотеза: Средний чек в группе A не равен группе B (между выборками имеется статистическая значимость)   
Применим критерием Манна-Уитни. Порогом статистической значимости установим alpha = 0.05


```python
print(
    '{0:.3f}'.format(
        stats.mannwhitneyu(
            orders[
                np.logical_and(
                    orders['group'] == 'A',
                    np.logical_not(orders['visitor_id'].isin(abnormalUsers)),
                )
            ]['revenue'],
            orders[
                np.logical_and(
                    orders['group'] == 'B',
                    np.logical_not(orders['visitor_id'].isin(abnormalUsers)),
                )
            ]['revenue'],
        )[1]
    )
)

print(
    '{0:.3f}'.format(
        orders[
            np.logical_and(
                orders['group'] == 'B',
                np.logical_not(orders['visitor_id'].isin(abnormalUsers)),
            )
        ]['revenue'].mean()
        / orders[
            np.logical_and(
                orders['group'] == 'A',
                np.logical_not(orders['visitor_id'].isin(abnormalUsers)),
            )
        ]['revenue'].mean()
        - 1
    )
)
```

    0.788
    -0.032


P-value значительно больше 0.05, не получилось отвергнуть нулевую гипотезу. Средний чек в группе A равен группе B (статистическая значимость не значительна и сделать вывод о различии нельзя).  
Относительное снижение среднего чека группы В к группе А примерно равен 3%. Изменение по сравнению с прошлыми результатами почти 30%.  
Подтвердилась та же гипотеза.

### Решение по результатам теста
[Содержание](#intro)

Тест можно остановить и считать успешно пройденным.  

Конверсия в группе A не равна группе B (между выборками имеется статистическая значимость) по сырым и по очищенным данным.  
Средний чек в группе A равен группе B (статистическая значимость не значительна и сделать вывод о различии нельзя) по сырым и по очищенным данным.  

После анализа видно, что группа В показывает лучше результат в конверсии.  
Среднемий чек не имеет значимых различий между группами.
