# cars_predict_hw1
Проект предсказания стоимости автомобиля по его характеристикам. Реализован сервис, написанный на основе применения FastAPI.

[HW1_Regression_with_inference.ipynb](HW1_Regression_with_inference.ipynb) - ipynb-ноутбук со всеми проведёнными экспериментами

[car_pred_hw1.py](car_pred_hw1.py) - файл с реализацией сервиса

[car_price_predictor.pkl](car_price_predictor.pkl) - файл с сохранёнными весами модели, коэффициентами скейлинга и прочими числовыми значениями (используется в inference)

[requerments.txt](requerments.txt) - файл с актуальными версиями библиотек

[test_ml.csv](test_ml.csv) - файл, который подавала на вход запроса post(/predict_items)

[items_predict.csv](items_predict.csv) - файл, который получила на выходе запроса post(/predict_items)

README.md-файл с выводами по проделанной работе

**Что было сделано**
1. Проведен eda анализ. Из train-выборки удалены дубли.
2. Поля mileage, engine, max_power приведены к числовому типу (убраны единицы измерения). Поле torque удалено.
3. Пропуски на train, test заменены медианой, полученной на train.
4. Выполнена визуализация: построены диаграммы рассеяния, heatmap, дополнительно по категориальным переменным выведено количество наблюдений в каждой группе.
Построены распределения цен на автомобили в зависимости от категории. Выведено распределение целевой.
5. Построены модели только на числовых факторах (включая seats) с применением gridsearch и kfold. Также выполнена стандартизация факторов.
Лучший результат на тесте: R2 0.5941
6. Добавлены категориальные переменные (кроме поля name) с применением ohe.
Лучший результат на тесте: R2 0.64469
7. Добавлены полиномиальные признаки (степени 2).
Лучший результат на тесте: R2 0.8312
8. Поля fuel, transmission, owner преобразованы на основе распределения цен на автомобили в группах каждой категории.
Добавлен новый фактор - страна производитель автомобиля (country.csv) - файл получен только на основе поля name из train.
Лучший результат на тесте: R2 0.8793
9. Попробовала выбросы заменять (если значения выше 95 перцентиля, заменить 95 перцентилем, если ниже 1 перцентиля, заменить 1-ым перцентилем).
Применяла не во всех полях, а только полях 'km_driven', 'engine', 'max_power', 'year'. Качество на test сразу стало ниже.
Попробовала добавить флаги (поле было пустым изначально или нет) - никакого прироста в качестве не дало.
10. Применила логарифмирование целевой (так как на этапе визуализации заметен был хвост у распределения).
Лучший результат на тесте: R2 0.9039
11. Затем еще применила gridsearch, чтобы найти лучшие гиперпараметры модели.
Лучший результат на тесте: R2 0.9183, MSE 46955640032.1502, бизнес метрика: 32.4%
12. Не вышло спарсить autoru и поработать с полем torque. Также можно еще попробовать улучшить модель за счет применения дополнительных способов отборов факторов в модель.

**Финальная модель**

Заменяем пропуски медианой. Делаем препроцессинг. Признаки fuel, transmission, owner обрабатываем по доп. логике, читаем
информацию о стране производителе автомобиля. Затем делаем ohe (я не выбрасывала одну категорию, так как в test могут
быть категории, которых не было в train). Затем создаем с помощью poly новые признаки. Логарифмируем целевую и обучаем модель.
Качество я смотрела, выполнив обратное преобразование от логарифмирования. Для построения использовала Ridge.

Тестирование проводила на http://localhost:8000/docs#/default/predict_items_predict_items_post.

Скрины работы ниже:
<img src="Снимок экрана 2023-11-28 в 15.57.44.png"/>
<img src="Снимок экрана 2023-11-28 в 15.58.03.png"/>
<img src="Снимок экрана 2023-11-28 в 15.58.17.png"/>
<img src="Снимок экрана 2023-11-28 в 15.58.40.png"/>
