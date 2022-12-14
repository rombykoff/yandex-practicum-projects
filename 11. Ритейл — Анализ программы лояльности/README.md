# Ритейл — Анализ программы лояльности

После внедрения программы лояльности важно понять действительно ли она показывает ожидаемый рост среднего чека и количества продуктов в покупательской корзине. Результаты исследования будут говорить о результатах внедрения программы. 

**Цель проекта -** оценить результаты внедрения программы лояльности, понять насколько сработала программа лояльности, стоит ли ее использовать дальше.

**Задача:**

Проанализировать программу лояльности магазина.

- Провести исследовательский анализ данных;
- Провести анализ программы лояльности;
- Сформулировать и проверить статистические гипотезы.


# Содержание

- Описание данных
- Шаг 1. Загрузка данных и изучение общей информации
- Шаг 2. Подготовка данных
    - Изменение названия столбцов
    - Изменение типов данных
    - Обработка пропусков
    - Обработка дубликатов
    - Объединение таблиц
- Шаг 3. Исследовательский анализ данных
    - Анализ количества покупок во времени
    - Анализ чеков и возвратов
    - Аномальные значения и выбросы
    - Анализ магазинов
    - Анализ покупателей
    - Вывод
- Шаг 4. Анализ программы лояльности
    - Доля клиентов, участвующих в программе лояльности
    - Анализ клиентов, участвующих в программе лояльности
        - Средний чек
        - Cредняя сумма заказа на одного клиента
        - Среднее количество уникальных товаров на одного покупателя
        - Сумма товаров на одного покупателя
        - Количество покупок на одного клиента
    - Распределение покупок по датам с разделением на группы
    - Топ-10 товаров по группам
    - Вывод
- Шаг 5. Проверка статистических гипотез
    - 95-й и 99-й перцентили стоимости заказов
    - 95-й и 99-й перцентили количества товаров в чеке
    - Гипотеза о средних чеках
    - Гипотеза о среднем количестве товаров в чеке
- Шаг 6. Вывод и рекомендации для заказчика
    - Материалы для коллег из коммерческого департамента
    
# Вывод и рекомендации для заказчика

В ходе предобработки данных:  
Названия столбцов приведены к нижнему регистру и переименованы на более удобные.  
Данные приведены к нужным типам.  
Пропуски обработаны и почти без потерь данных.  
Дубликаты удалены. Удалено 1033 строк, меньше 1% данных.  
Получили общий датафрейм со всеми интересующими нас данными.  

Для нас доступен период с 01.12.2016 по 28.02.2017.
По графику видим, что пик продаж пришелся на первую половину декабря и совсем не было продаж последнюю неделю декабря - первую неделю января. Вероятно, что магазин не работал в праздичные дни.

Количество чеков с возвратами: 1019  
Возвращено 950 видов товаров  
Возвращено всего: 119554 шт  
Число покупателей, оформивших возврат: 780  
Общая сумма возвратов за анализируемый период: 371954.855  
Доля возвратов составляет: 15.5 %  
Самый возвращаемый товар - `23166` 

Видим 3656 покупок.  
Большинство покупателей приобретает до 221 единиц товара, состоящих из 21 уникальных позиций.  
Чаще чек пробивался без учета программы лояльности.  
Сумма чека в 75% случаев - до 592.80 р.  

Имеем данные за 31 магазин, после корректировки данных остались данные по 30 магазинам.

Самый крупный из них по всем показателям - `Shop 0`. Выручка за изучаемый период в нем составила 1591049. В этом же магазине максимальное количество покупок по программе лояльности.

В остальных магазинах выручка гораздо ниже. По выручке выделяется `Shop 4`: 53748.87, здесь же было продано большее количество товаров (после `Shop 0`).

Так же выделяются `Shop 1`, `Shop 6`, `Shop 8`. В них выручка находится в пределах 17 - 48,6. Количество проданных товаров от 5 до 18,5 тыс.шт. Они же отличаются разнообразностью проданных товаров. Больше чеков и покупателей с ID в `Shop 4`, `Shop 1`.

Покупатели приобретали товары по программе лояльности в 4 магазинах: `Shop 0`, `Shop 19`, `Shop 8`, `Shop 28`.

Есть 8 магазинов, в которых была совершена всего 1 покупка за весь анализируемый период.

Имееем 1663 покупателя.

В 50% случаях у покупателей 1 чек за время наблюдения.

На 1 покупателя приходтся от 1 до 1906 видов товаров, медианное значение - 19 шт.

А по количеству, медианное значение - 177 шт.

Чаще всего покупки совершались в 1 магазине.

Медиана суммы выручки от 1 человека составляет 177р.

В программе лояльности участвует 558 человек (33,6%). 1/3 от общего числа.

Все расчеты производились с учетом дополнителной оплаты программы лояльности клиентами.

Средний чек у клиентов c картой лояльности в январе и феврале выше чем у клиентов без карты. Особенно большой отрыв виден в феврале. В декабре средние чеки примерно одинаковы.
Исходя из этих данных, если исключить новогодний ажиотаж, можно сказать, что лояльные клиенты в среднем платят больше.

Средняя сумма заказов на одного клиента без карты лояльности больше по всем месяцам. По графику видна положительная тенденция, в феврале показатели уже почти сравнялись.

Мы увидели, что среднее количество уникальных товаров на одного покупателя без карты меньше, чем у клиентов с картой. Это говорит нам о том, что покупки у лояльных клиентов гораздо разнообразнее (больше наименований в чеке). Благодаря этому улучшается оборачиваемость склада и так же это можно использовать для увелечения выруки.

Клиенты без карты лояльности в абсолютных значениях покупают больше товаров. На эти показатели могли повлиять большие оптовые заказы, на это косвенно также указывает прошлый график(малое разнообразие, а получаем большой объем). По графику видно, что лояльные клиенты видут себя стабильнее чем нелояльные. 

По графику мы увидели снижение покупательской активности двух групп. Скорее всего это связано с окончанием праздников. В процентном соотношении сильнее всего это произошло у лояльных клиентов, но позже ситуация стабилизировалась. 

График распределения по датам хорошо показывает, что клиенты без карт лояльности совершают покупки чаще (что вполне логично, так как их значительно больше. Но график медианы выручки показывает, что чек у клиентов с картой значительно выше. Этот график хорошо подтверждает сделанные ранее выводы.

Самый популярный товар во всех группах - 85123A
В целом по данным видно, что покупатели покупают примерно одинаковые товары.

После проверки гипотез сделаем вывод: различия в среднем чеке и количестве товаров получили статистическую значимость, это говорит нам о том, что есть основания предполагать, что они отличны (у тех кто участвует в системе лояльности и тех кто нет).

Средний чек у лояльных клиентов выше. (48.6% / 51.4%)  
Среднее количетво товаров в чеке у нелояльных клиентов выше (54.8% / 45.2%)  

**На основе всего этого**  
Можно сказать, что программа лояльности в данном виде эффективна и есть хороший потенциал. Клиенты с картами лояльности покупают больше позиций (в феврале разница составляет около 60% или 2 единцы товара) и приносят больше выручки(в феврале разница чека составила примерно 15% или 90р., средний чек в общей массе растет).  
Но на данном этапе, данных пока мало и к тому же они за праздничные дни, что может искажать общую картину.

**Рекомендации**  
Распространить программу лояльности на другие магазины сети. В данный момент участвует только 4.  
В продвижении карты лояльности попробовать сделать упор на популярные товары, которые хорошо продаются как с ней, так и без неё.  
Следует посмотреть данные за более продолжительный период, например за год, чтобы понять полную картину.  
Возможно стоит отдельно изучить оптовые заказы и влияние карты лояльности на них.  
Проанализировать финансовую деятельность всех магазинов. Сделать выводы, возможно ли их дальнейшее развитие или, возможно, часть точек можно сократить и больше заниматься развитием остальных.