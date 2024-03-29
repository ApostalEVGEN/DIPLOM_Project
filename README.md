# Определение уязвимых групп населения
Итоговый проект первого года обучения Skillfactory Data Science Pro
# 

## Оглавление  
[1. Описание проекта](.README.md#Описание-проекта)  
[2. Какой кейс решаем?](.README.md#Задачи)    
[3. Краткая информация о данных](.README.md#Результат)   
[4. Этапы работы над проектом](.README.md#Результат)   
[5. Результат](.README.md#Результат)   


### Описание проекта    
Предоставлены данные государственных органов о доходах, заболеваемости, социально незащищенных слоях населения России и другие экономические и демографические данные. Задача проекта - кластеризовать регионы России и определить, какие из них наиболее остро нуждаются в помощи малообеспеченным/неблагополучным слоям населения:
-   описать группы населения, сталкивающиеся с бедностью;
-   определить следующее:
  -  влияет ли число детей, пенсионеров и других социально уязвимых групп на уровень бедности в регионе; 
  -  связаны ли уровень бедности/социального неблагополучия с производством и потреблением в регионе;
  -  какие еще зависимости можно наблюдать относительно социально незащищенных слоев населения.

:arrow_up:[к оглавлению](_)


### Какой кейс решаем?  

БКомпиляция данных (data engineering). Разработка ML-модели, кластерный анализ. Разработка сервиса: uWSGI, uWSGI+NGINX, Docker.

:arrow_up:[к оглавлению](.README.md#Оглавление)


### Краткая информация о данных

Различные статистические, экономические и демографические данные госорганов по субъектам Российской Федерации за разные годы, полученные из открытых источников. Полный набор файлов содержится в папке social_russia_data/. Более подробное описание дается при компиляции данных в части 1 настоящего проекта: p1_dataset_compilation.ipynb

Финальный датасет, russia_regions_2020.csv, используемый для реализации проекта, представляет из себя срез данных 2020 г., по которому имеется наиболее полная статистика.

:arrow_up:[к оглавлению](.README.md#Оглавление)


### Этапы работы над проектом

компиляция данных: p1_dataset_compilation.ipynb
построение ML-модели кластеризации: p2_modeling.ipynb
описание кластеров, анализ зависимостей: p3_analysis.ipynb
разработка сервиса для построения графика распределений выбранного признака в зависимости от кластера: dplot_service/

:arrow_up:[к оглавлению](.README.md#Оглавление)

### Результат:  
Проведено считывание экономических и демографических данных из предоставленных файлов различного формата и из дополнительных источников. На основе полученных датасетов сформирована финальная таблица по срезу 2020 г. Помимо этого, структура ноутбука позволяет с помощью минимальных изменений кода оперативно комбинировать различные наборы характеристик для возможного анализа финасово-экономических, демографических, медицинских, криминальных показателей по российским регионам.

В качестве критериев для группировки регионов в кластеры с близкими параметрами выбраны признаки, отражающие социально-экономическое благополучие субъектов федерации. Построены рейтинги регионов по избранным показателям. Исследованы распределения выбранных признаков и корреляции между ними, произведен отбор и выполнены необходимые преобразования. Итоговый модельный датасет для проведения кластеризации включает в себя 5 независимых характеристик:

номинальную заработную плату, нормированную на прожиточный минимум в регионе;
валовый региональный продукт в логарифмической шкале;
объем розничной торговли на человека, нормированный на прожиточный минимум в регионе;
жилую площадь на человека;
процент населения за чертой бедности.
С помощью различных внутренних метрик кластеризации оценено оптимальное количество кластеров (четыре). Построена базовая модель (k-means). Выполнено PCA-понижение размерности (3 главных компоненты, объясняющие 90% дисперсии). Протестированы различные алгоритмы кластеризации, из них на основе метрик и визуализации признакового пространства выбран оптимальный ("гауссова смесь на PCA-компонентах"), регионам присвоены соответствующие метки. Продемонстрирована устойчивость общей структуры кластеров при использовании различных алгоритмов кластеризации. При необходимости в качестве альтернативы можно также использовать k-means на PCA-компонентах, дающий похожие значения метрик и несколько большее число финансово неблагополучных регионов.

Дано описание характерных особенностей полученных кластеров:

бедные регионы;
среднестатистические регионы;
регионы "комфорт"
бизнес-регионы.
Проведен анализ социально-демографических групп населения, показывающий, что наиболее уязвимой группой населения по финасовым показателям являются семьи с детьми. При этом в кластере наиболее бедных регионов наблюдается аномально высокая рождаемость, необеспеченная доходами родителей, а доля детей среди всего населения на 10% больше, чем в остальных регионах. В остальных группах рождаемость слабо растет с ростом финансового благополучия. При этом процент детского населения падает от кластера бедных регионов к кластерам более богатых. В бедных регионах отмечается существенно более низкий процент населения пенсионного возраста (на 10%).

Проведен анализ различия других характеристик датасета по кластерам. Подробное описание имеющихся корреляций и отсутствия таковых дано в части 3, p3_analysis.ipynb.

Разработан Flask-сервис, позволяющий строить боксплоты распределений выбранного признака внутри кластеров согласно имеющейся модели. Сервис работает на основе клиент-сервисной архитектуры. Сервер работает в вариантах uWSGI и uWSGI+NGINX.
