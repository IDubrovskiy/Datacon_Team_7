## Datacon_Team_7
Предсказание токсичности наночастиц

Выполнение проекта включало в себя следующие этапы:
### 1. Первичная обработка данных (создание единого датафрейма, дополнение базы данных новыми дескрипторами)
    Это происходит в db_preparation.ipynb
    В результате некоторая часть дескрипторов была удалена и было оставлено только 16, 
    включающих в себя названия частиц и их различные характеристики, а также дескрипторы, 
    которые были созданы с использованием других библиотек. Такие дескрипторы включают в 
    себя молярную массу частицы (бралась из библиотеки pymatgen), электроотрицательность 
    (бралась из библиотеки pymatgen) и плотность заряда, которая является средним отношением 
    ионного радиуса к степени окисления для каждого элемента.
    Некоторые дескрипторы содержали в себе большое количество значений, которые были объединены 
    в группы, например все возможные поверхностные модификации были выделены в 8 групп 
    (0 - отсутствие модификации, 1 - модификация амином, 2 - спиртом и тд), также использованные 
    тесты токсичности были разделены на 17 групп, типы клеток были преобразованы в два больших 
    класса, а именно раковые и нормальные, морфология клеток также была преобразована в 11 
    разных классов. Похожие преобразования коснулись и других дескрипторов.

### 2. Статистический анализ данных (визуализация и обработка данных, удаление выбросов)
    Это происходит в statistics.ipynb
    После объединения в один файл были изучены предложенные данные. 
    Сначала для каждой категориальной характеристики были построены круговые или столбчатые диаграммы. 
    Основные выводы:
    1. В датасете представлено больше неорганических частиц
    2. Чаще использовались человеческие клетки для экспериментов, чем клетки животных
    3. Раковых и нераковых клеток было примерно одинаковое количество
    4. Наиболее часто встречающимися частицами являются ZnO, Fe2O3, Ag, Au
    5. Чаще всего частицы не имели поверхностной модификации
    Для числовых признаков были построены гистограммы и боксплоты, чтобы посмотреть на выбросы. 
    Для удаления выбросов использовали в основном 99 квантиль. Для viability использовался 
    диапазон от 0 до 120%, так как часть клеток могла не только выжить, но и размножиться.
    Чтобы не терять данные, удалялись только отдельные значения, после этого все Nan заменялись с помощью метода kNN.
    Далее для полученного файла была построена тепловая матрица со значениями корреляции. Основные выводы:
    1.  Наибольшая корреляция наблюдается для electronegativity и charge_density
    2. viability_% (интересующая нас характеристика) не коррелирует практически ни с чем, 
    наибольшая корреляция наблюдается для electronegativity и charge_density
    3. Все остальные характеристики также слабо коррелируют между собой, коэффициенты окло 0 – 0,2
    Для более подробного изучения зависимостей между числовыми переменными были построены паирплоты, 
    которые также делились в зависимости от того органическая или неорганическая частица. Выводы для viability:
    1. Клетки в присутствии частиц с большим размером реже размножаются и в основном их viability не больше 100
    2. Более электроотрицательные частицы чаще имеют высокие значения viability
    Скрипичные диаграммы совмещают в себе боксплоты и графики плотности, 
    поэтому они также были построены для числовых переменных в зависимости от категориальных. 
    Краткие выводы:
    1. Органические и неорганические частицы в основном различаются молекулярной массой и зарядом поверхности
    2. Как и предполагалось, исследование на раковых и не раковых клетках никак не влияет на сами частицы
    3. Поверхностная модификация существенно изменяет такие параметры частиц как электроотрицательность, 
    заряд поверхности, а также размер, однако слабо влияет на выживаемость клеток
### 3. Построение алгоритмов машинного обучения (расчет метрик, выбор лучшей модели, оптимизация выбранных моделей)
    Это происходит в analysis.ipynb
    Данная часть включала в себя предобработку базы данных, а именно масштабирование всех 
    дескрипторов от 0 до 1, также в ней данные были рандомно разделены на обучающий и тестовый наборы. 
    Затем были построены и опробованы следующие модели: Linear Regression, Gradient Boosting Regression, 
    Cat Boosting Regression, Decision Tree Regression, Random Forest Regression, XGB Regression, 
    при этом для последних двух была также проведена оптимизация и feature importance.

Для запуска проекта необходимо выполнить следующие команды:

pip install -r requirements.txt
