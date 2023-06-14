![image](https://github.com/dmamontova/andan-project/assets/121117316/8052d70c-b2cf-43fb-bf55-9c0a444957f8)
## Привет, мой единственный читатель! 
### Над этим проектом кровью и потом трудились:
- Старощук Богдан
- Фоменко Андрей
- Мамонтова Дарья

Проект посвящен анализу влияния различных видов насилия против гражданского населения и политических конфликтов на экономику страны, для этого мы будем использовать крупные датасеты с классификацией и краткой характеристикой событий отсюда: https://acleddata.com/about-acled/. 
Эти данные мы дополнили индексом человеческого развития (HDI) и классификацией страны (country classifications by income level).

Основная цель работы - проверка предположения о том, что ненасильственные политические столкновения свидетельствуют о высоком развитии страны, а также применение методов машинного обучения для предсказания класса страны и уровня HDI.

Для более удобного восприятия, мы разделили работу над проектом на несколько осмысленных частей.


# 1. [Парсинг](https://github.com/dmamontova/andan-project/tree/all-work_main/parsing)

С помощью парсинга мы получили информацию о следующих признаках:
- HDI или индекс человеческого развития. Подробная информация о том, как происходил парсинг этого индекса, содержится в [тетрадке](https://github.com/dmamontova/andan-project/blob/project_main/parsing/index_parse.ipynb). Стоит отметить, что данные были получены со следующего [сайта](https://countryeconomy.com/hdi?year=1997). На выходе мы получили таблицу с HDI по странам.
- Индекс экономической классификации. Опять же вся техническая часть залита в [ноутбук](https://github.com/dmamontova/andan-project/blob/project_main/parsing/parsing_income.ipynb). Также на этом этапе мы объединили полученные данные с тем, что получили касаемо HDI, в общую таблицу.

И, наконец, последний этап парсинга.

[Предобработка данных](https://github.com/dmamontova/andan-project/blob/project_main/parsing/final_predobr.ipynb). Немного поработали с тем, чтобы привести все полученные данные в аккуратный вид. На выходе мы получили [таблицу](https://drive.google.com/file/d/1O3jwPG2JOHn5F90vUD4X7JsYtyZNSIrM/view?usp=share_link), с которой будем работать на дальнейших этапах. 
> Она достаточная большая, поэтому по ссылке залита на диск

И теперь сперва, неплохо было бы покапаться в наших данных и понять, что они из себя представляют.


# 2. [EDA](https://github.com/dmamontova/andan-project/tree/all-work_main/EDA)


На этом этапе мы разделили всю работу на несколько частей.

Теперь подробнее:

[EDA I](https://github.com/dmamontova/andan-project/blob/all-work_main/EDA/EDA%20I.ipynb). Для начала, мы провели первичную предобработку данных: описание переменных, анализ таблицы на пропуски и их заполнение, определение типов переменных, содержащихся в данных. 

После того как данные были подготовлены к дальнейшему анализу, мы решили рассмотреть категориальные и числовые переменные в отдельности, поискать взаимосвязи и построить понятные визуализации, поэтому:

[EDA II.1](https://github.com/dmamontova/andan-project/blob/all-work_main/EDA/EDA%20II.1.ipynb) Здесь мы рассмотрели определённые категориальные переменные в отдельности, а именно: Disorder_type, Event_type, Sub_event_type. Были выбраны именно они, так как, на наш взгляд, именно эти признаки обладают важной информацией, и понимание того, что они из себя представляют, поможет нам на этапе машинного обучения. Для лучшей интерпертируемости результатов, были построены гистограммы и диаграммы.

Очевидно, что рассмотрение категориальных признаков в отдельности недостаточно, хотелось бы посмотреть на них в комбинации с другими переменными, что мы и делали далее:

[EDA II.2](https://github.com/dmamontova/andan-project/blob/all-work_main/EDA/EDA%20II.2.ipynb) В этой части были рассмотрены внутренние связи между категориальными переменными. Кроме того, так как планируемой на этапе машинного обучения является задача классификации стран/регионов по экономическому индексу, мы рассмотрели целевую переменную в разрезе признаков, выделенных на предыдущем этапе. Так как мы использовали именно внутренние связи, самым удобным способом визуализации стали круговые диаграммы.

Ещё на первом этапе ED'ы мы обнаружили, что числовых признаков в наших данных не так уж и много, тем не менее, их без внимания мы тоже не оставили:

[EDA III](https://github.com/dmamontova/andan-project/blob/all-work_main/EDA/EDA%20III.ipynb) Завершающий этап с рассмотрением числовых переменых. В ходе работы мы выяснили, на какие года пришлось наибольшее количество конфликтов и, как следствие, выявили скорее возрастающий тренд в их количестве. Кроме того, используя в качестве визуализации интерактивную карту мира и данные из таблицы о координатах места события, выяснили, какие регионы наиболее сильно подвержены столкновениям. А также, конечно же, ввиду того, что ещё одной из целевых переменных является HDI, посмотрели на корреляцию числовых признаков с ней.

Теперь, когда мы потрогали данные, можно выдвинуть определённые гипотезы, проверка которых пригодится на следующих этапах.

# 4. [Проверка гипотез](https://github.com/dmamontova/andan-project/blob/all-work_main/Hypotheses.ipynb)


В первую очередь нас интересовали гипотезы, которые как-либо связанными с целевыми переменным: Index и Classification. Мы рассмотрели 5 основных гипотез, на основании которых сделали определённые выводы, и в зависимости от отвержения/ не отвержения $H_0$ сделали выводы о влиянии независимых признаков на целевые, а также о внутренних взаимосвязях.

После чего, мы приступили к самому сладкому этапу - создание новых признаков и машинное обучение.

# 5. Создание признаков и машинное обучение


При помощи моделей градиентого бустинга попытаемся решить задачу классификации стран на группы (Lower middle income, Upper middle income, Low income, High income), а также задачу регрессии для предсказания индекса HDI. Мы будем применять методы анализа текстов для работы с признаком "notes", который состоит из кратких новостных сводок.



