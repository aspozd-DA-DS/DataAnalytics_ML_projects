<a id="top"></a>

<p align="right">
  <a href="#ru">🇷🇺 RU</a> | <a href="#en">🇬🇧 EN</a>
</p>

---

<a id="ru"></a>

# Forecast Direction: прогнозирование направления движения цены

## 📌 Описание проекта

Проект представляет собой полный цикл анализа и построения модели бинарной классификации для прогнозирования направления движения цены финансового инструмента на следующий торговый день. Включает EDA для временных рядов, feature engineering, проверку стационарности и сезонности, обучение моделей с TimeSeriesSplit, калибровку вероятностей и формулирование практических рекомендаций для трейдеров.

**Ключевые цели проекта:**
- Разработать интерпретируемую модель бинарной классификации направления цены (рост/падение).
- Построить устойчивый пайплайн, исключающий "заглядывание в будущее" (look-ahead bias).
- Выявить ключевые рыночные факторы, влияющие на направление движения.
- Оценить реальную предсказательную силу модели на отложенном тесте.

**Практическая ценность:**
- Формирование торговых сигналов для алгоритмических систем.
- Поддержка инвестиционных решений и управление рисками.
- Понимание факторов, влияющих на краткосрочную динамику рынка.

---

## 🎯 Цели исследования
1. Построить модель бинарной классификации, предсказывающую направление цены на следующий день.
2. Выбрать и обосновать метрики качества с учётом бизнес-смысла (ложные сигналы vs пропуск движения).
3. Проверить временную структуру: монотонность дат, пропуски (выходные/праздники).
4. Изучить свойства ряда: тренды, сезонность, волатильность, автокорреляцию.
5. Создать информативные признаки на основе лагов, скользящих окон и технических индикаторов.
6. Сравнить модели (Logistic Regression, Decision Tree, Random Forest, XGBoost, LightGBM) с TimeSeriesSplit.
7. Проанализировать важность признаков и интерпретировать логику модели через SHAP.
8. Сформулировать практические рекомендации для трейдеров.

---

## 🔧 Технический стек
- **Язык программирования:** Python  
- **Анализ данных:** Pandas, NumPy  
- **Визуализация:** Matplotlib, Seaborn  
- **Статистика:** SciPy, statsmodels, `phik`  
- **Машинное обучение:** Scikit-learn, XGBoost, LightGBM  
- **Интерпретация моделей:** SHAP  
- **Калибровка вероятностей:** Platt scaling (sigmoid)  
- **Валидация:** TimeSeriesSplit, Walk-Forward Validation

---

## 📊 Данные
Используется синтетический датасет OHLCV (Open, High, Low, Close, Volume) за период с 2014 по 2024 год.

**Структура данных:**
- `date` — дата торгов (индекс)
- `open` — цена открытия
- `high` — максимальная цена за день
- `low` — минимальная цена за день
- `close` — цена закрытия
- `vol` — объём торгов
- `target` — направление движения цены на следующий день (1 — рост, 0 — падение)

**Особенности данных:**
- 2507 торговых дней
- Пропуски календарных дат — выходные и праздники (нормально для биржевых данных)
- Классы сбалансированы: 51.3% рост, 48.7% падение
- Целевая переменная не является простым `close[t+1] > close[t]` (10% несовпадений)

---

## 📈 Ключевые этапы проекта

### Шаг 1: Загрузка и предобработка данных
- Загрузка Excel-файла, установка даты как индекса
- Проверка монотонности дат, пропусков, дубликатов
- Анализ целевой переменной: баланс классов, логическая проверка
- Выявлено 254 несовпадения с простым правилом close-to-close

### Шаг 2: Feature Engineering
- **Базовые признаки:** тело свечи, тени, внутридневная волатильность
- **Доходности и лаги:** лог-доходность, лаги доходности, лаги цены и объёма
- **Скользящие статистики:** SMA 5/10/20/50, волатильность (rolling_std), относительный объём
- **Технические индикаторы:** RSI (14), ROC (5)
- **Календарные признаки:** день недели, месяц
- **Итоговый набор:** 18 признаков (исключены нестационарные ценовые ряды)

### Шаг 3: EDA для временных рядов
- Распределения: доходности имеют тяжёлые хвосты, не нормальны
- Временные графики: долгосрочный восходящий тренд с коррекцией после 2022
- Декомпозиция: выраженная годовая сезонность, кластеры волатильности
- Автокорреляция: отсутствие линейной автокорреляции в доходностях
- Корреляционный анализ: 83 пары признаков с корреляцией >0.7

### Шаг 4: Подготовка к моделированию
- Отбор 18 стационарных признаков, исключение сырых цен
- Удаление NaN (первые ~50 строк)
- Разделение по времени: Train (2014–2021), Valid (2022–2023), Test (2024)
- Масштабирование: RobustScaler (fit на train)
- Раздельные пайплайны для деревьев и логистической регрессии

### Шаг 5: Моделирование
- **Бейзлайны:** most_frequent (F1=0.324), stratified (F1=0.501), naive (F1=0.473)
- **Logistic Regression:** C=0.05, VALID F1=0.501
- **Decision Tree:** max_depth=3, VALID F1=0.605 — лучшая модель
- **Random Forest:** VALID F1=0.488
- **XGBoost:** VALID F1=0.525
- **LightGBM:** VALID F1=0.501
- **Калибровка:** Platt scaling улучшил Brier score на 7.4%
- **Оптимальный порог:** 0.20 (F1=0.646, Recall=0.987)

### Шаг 6: Оценка на тесте
- **Метрики (порог 0.20):** F1=0.643, Recall=1.000, Precision=0.474, ROC-AUC=0.529
- **Матрица ошибок:** TP=54, FP=60, TN=0, FN=0
- **Стабильность:** средний F1 по месяцам 0.636, размах 0.250
- **Прибыльность стратегии:** 0.218 (DecisionTree), 0.193 (XGBoost calibrated)

### Шаг 7: Интерпретация модели
- **Feature Importance (DecisionTree):** только 6 признаков из 18
  - return_lag1 (0.194), roc_5 (0.178), dist_to_low_5 (0.173), dist_to_high_5 (0.155), overnight_return (0.154), vol_lag1 (0.146)
- **SHAP-анализ:**
  - dist_to_high_5 — самый сильный вклад в сигналы роста
  - dist_to_low_5 — главный фильтр качества (delta TP-FP = +0.0132)
  - return_lag1 — второй по значимости фильтр качества
- **Логика дерева:** моментум + локальные уровни + гэпы + объём

---

## 📊 Основные выводы

### Ключевые признаки, связанные с направлением движения
- `return_lag1` — вчерашняя доходность (главный драйвер)
- `roc_5` — 5-дневный моментум
- `dist_to_low_5` / `dist_to_high_5` — положение цены относительно локальных экстремумов
- `overnight_return` — гэп открытия
- `vol_lag1` — лагированный объём

### Логика модели (DecisionTree)
Модель реализует простую, но интерпретируемую торговую стратегию:
- Если цена недалеко от локального минимума → вероятен рост
- Если недавняя доходность положительная → вероятен рост
- Если 5-дневный рост умеренный → вероятен рост
- Если гэп открытия не отрицательный → вероятен рост
- Если объём вчера был нормальным → вероятен рост

### Качественные сигналы vs ложные
- **Верные сигналы (TP):** цена в правильной зоне относительно минимума, положительная доходность, гэп не против движения
- **Ложные сигналы (FP):** цена далеко от минимума, отрицательная вчерашняя доходность, слабый моментум

### Стабильность признаков
- Топ-6 признаков стабильны на train/valid/test
- Это подтверждает, что модель выучила устойчивые рыночные паттерны

### Рекомендации по выбору модели

| Сценарий | Рекомендуемая модель | Порог | Ожидаемый F1 |
|----------|---------------------|-------|--------------|
| Максимальный охват роста | DecisionTree | 0.20 | 0.646 |
| Сбалансированный подход | XGBoost (калиброванный) | 0.50 | 0.525 |
| Консервативный (мало ложных сигналов) | XGBoost (калиброванный) | 0.55–0.65 | 0.46–0.58 |

### Ограничения исследования
- **Малый тестовый период:** 114 наблюдений → высокая дисперсия метрик
- **Низкий Precision:** 47% точных сигналов → более половины ложные
- **Слабое качество вероятностей:** ROC-AUC = 0.529
- **Модель предсказывает только рост на тесте:** TN = 0
- **Отсутствие учёта транзакционных издержек**

### Идеи для развития
- Улучшить Feature Engineering (interaction features, более длинные лаги)
- Использовать Optuna для оптимизации гиперпараметров
- Стекинг моделей (DecisionTree + XGBoost)
- Expanding window валидация для увеличения объёма данных
- Добавить фильтрацию сигналов на основе SHAP
- Внедрить онлайн-дообучение на новых данных

---

## 📁 Структура репозитория

`04_Forecast_Direction_OHLCV/`

```text
├── `Dataset_Forecast_Direction_OHLCV/`     — исходный CSV-файл 
├── `Forecast_Direction_OHLCV.ipynb`        — основной ноутбук с полным анализом  
├── `Forecast_Direction_OHLCV.pdf`          — экспорт в PDF  
├── `requirements.txt`                      — зависимости проекта
└── `README.md`                             — эта документация
```
---

## 🚀 Как запустить
1. Склонировать репозиторий:
   ```bash
   git clone https://github.com/aspozd-DA-DS/DataAnalytics_projects.git
   ```
2. Перейти в папку проекта:
   ```bash
   cd DataAnalytics_projects/04_Forecast_Direction_OHLCV
   ```
3. Запустить ноутбук:
   ```bash
   jupyter notebook Forecast_Direction_OHLCV.ipynb
   ```

---
   
## 🏷 Topics
`Data Analysis` `EDA` `Visualization` `Time Series` `Financial Data` `Python` `Binary Classification` `Machine Learning` `XGBoost LightGBM` `SHAP Feature Engineering` `Trading Quantitative Finance`

<p align="right"><a href="#top">⬆ наверх</a></p>

---

<a id="en"></a>

# Forecast Direction: forecasting price movement direction

## 📌 Project description

This project is a full analysis cycle and a binary classification model for forecasting the direction of a financial instrument's price movement on the next trading day. It covers time series EDA, feature engineering, stationarity and seasonality checks, model training with TimeSeriesSplit, probability calibration, and practical recommendations for traders.

**Key project goals:**
- Develop an interpretable binary classification model of price direction (up/down).
- Build a robust pipeline that avoids look-ahead bias.
- Identify the key market factors that influence the direction of movement.
- Evaluate the model's real predictive power on a held-out test set.

**Practical value:**
- Generating trading signals for algorithmic systems.
- Supporting investment decisions and risk management.
- Understanding the factors that drive short-term market dynamics.

---

## 🎯 Research goals
1. Build a binary classification model that predicts the price direction for the next day.
2. Choose and justify quality metrics in light of business meaning (false signals vs missed moves).
3. Check the temporal structure: date monotonicity, gaps (weekends/holidays).
4. Study the properties of the series: trends, seasonality, volatility, autocorrelation.
5. Create informative features based on lags, rolling windows, and technical indicators.
6. Compare models (Logistic Regression, Decision Tree, Random Forest, XGBoost, LightGBM) with TimeSeriesSplit.
7. Analyze feature importance and interpret the model's logic via SHAP.
8. Formulate practical recommendations for traders.

---

## 🔧 Technical stack
- **Programming language:** Python  
- **Data analysis:** Pandas, NumPy  
- **Visualization:** Matplotlib, Seaborn  
- **Statistics:** SciPy, statsmodels, `phik`  
- **Machine learning:** Scikit-learn, XGBoost, LightGBM  
- **Model interpretation:** SHAP  
- **Probability calibration:** Platt scaling (sigmoid)  
- **Validation:** TimeSeriesSplit, Walk-Forward Validation

---

## 📊 Data
The project uses a synthetic OHLCV (Open, High, Low, Close, Volume) dataset covering 2014 to 2024.

**Data structure:**
- `date` — trading date (index)
- `open` — opening price
- `high` — highest price of the day
- `low` — lowest price of the day
- `close` — closing price
- `vol` — trading volume
- `target` — price direction on the next day (1 — up, 0 — down)

**Data notes:**
- 2507 trading days
- Gaps in calendar dates — weekends and holidays (normal for exchange data)
- Balanced classes: 51.3% up, 48.7% down
- The target is not a simple `close[t+1] > close[t]` (10% mismatches)

---

## 📈 Key project stages

### Step 1: Data loading and preprocessing
- Loading the Excel file, setting the date as the index
- Checking date monotonicity, gaps, and duplicates
- Analyzing the target: class balance, logical check
- 254 mismatches with the simple close-to-close rule identified

### Step 2: Feature engineering
- **Basic features:** candle body, shadows, intraday volatility
- **Returns and lags:** log return, return lags, price and volume lags
- **Rolling statistics:** SMA 5/10/20/50, volatility (rolling_std), relative volume
- **Technical indicators:** RSI (14), ROC (5)
- **Calendar features:** day of week, month
- **Final set:** 18 features (non-stationary price series excluded)

### Step 3: Time series EDA
- Distributions: returns have heavy tails and are not normal
- Time plots: long-term upward trend with a correction after 2022
- Decomposition: pronounced annual seasonality, volatility clusters
- Autocorrelation: no linear autocorrelation in returns
- Correlation analysis: 83 feature pairs with correlation >0.7

### Step 4: Preparing for modeling
- Selecting 18 stationary features, excluding raw prices
- Dropping NaN rows (first ~50 rows)
- Time-based split: Train (2014–2021), Valid (2022–2023), Test (2024)
- Scaling: RobustScaler (fit on train)
- Separate pipelines for tree-based models and logistic regression

### Step 5: Modeling
- **Baselines:** most_frequent (F1=0.324), stratified (F1=0.501), naive (F1=0.473)
- **Logistic Regression:** C=0.05, VALID F1=0.501
- **Decision Tree:** max_depth=3, VALID F1=0.605 — best model
- **Random Forest:** VALID F1=0.488
- **XGBoost:** VALID F1=0.525
- **LightGBM:** VALID F1=0.501
- **Calibration:** Platt scaling improved the Brier score by 7.4%
- **Optimal threshold:** 0.20 (F1=0.646, Recall=0.987)

### Step 6: Test set evaluation
- **Metrics (threshold 0.20):** F1=0.643, Recall=1.000, Precision=0.474, ROC-AUC=0.529
- **Confusion matrix:** TP=54, FP=60, TN=0, FN=0
- **Stability:** average monthly F1 0.636, range 0.250
- **Strategy profitability:** 0.218 (DecisionTree), 0.193 (XGBoost calibrated)

### Step 7: Model interpretation
- **Feature importance (DecisionTree):** only 6 of 18 features
  - return_lag1 (0.194), roc_5 (0.178), dist_to_low_5 (0.173), dist_to_high_5 (0.155), overnight_return (0.154), vol_lag1 (0.146)
- **SHAP analysis:**
  - dist_to_high_5 — strongest contribution to buy signals
  - dist_to_low_5 — main quality filter (delta TP-FP = +0.0132)
  - return_lag1 — second most important quality filter
- **Tree logic:** momentum + local levels + gaps + volume

---

## 📊 Key findings

### Key features linked to the direction of movement
- `return_lag1` — yesterday's return (main driver)
- `roc_5` — 5-day momentum
- `dist_to_low_5` / `dist_to_high_5` — position of the price relative to local extremes
- `overnight_return` — opening gap
- `vol_lag1` — lagged volume

### Model logic (DecisionTree)
The model implements a simple but interpretable trading strategy:
- If the price is close to a local minimum → a rise is likely
- If the recent return is positive → a rise is likely
- If the 5-day rise is moderate → a rise is likely
- If the opening gap is not negative → a rise is likely
- If yesterday's volume was normal → a rise is likely

### Quality signals vs false signals
- **True signals (TP):** price in the right zone relative to the minimum, positive return, gap not against the move
- **False signals (FP):** price far from the minimum, negative previous-day return, weak momentum

### Feature stability
- The top 6 features are stable across train/valid/test
- This suggests the model has learned stable market patterns

### Model selection recommendations

| Scenario | Recommended model | Threshold | Expected F1 |
|----------|------------------|-----------|-------------|
| Maximum coverage of up-moves | DecisionTree | 0.20 | 0.646 |
| Balanced approach | XGBoost (calibrated) | 0.50 | 0.525 |
| Conservative (few false signals) | XGBoost (calibrated) | 0.55–0.65 | 0.46–0.58 |

### Limitations of the study
- **Short test period:** 114 observations → high variance of metrics
- **Low precision:** 47% of signals are correct → more than half are false
- **Weak probability quality:** ROC-AUC = 0.529
- **On the test set the model predicts only "up":** TN = 0
- **Transaction costs not accounted for**

### Ideas for development
- Improve feature engineering (interaction features, longer lags)
- Use Optuna for hyperparameter optimization
- Model stacking (DecisionTree + XGBoost)
- Expanding window validation to increase the amount of data
- Add signal filtering based on SHAP
- Implement online retraining on new data

---

## 📁 Repository structure

`04_Forecast_Direction_OHLCV/`

```text
├── `Dataset_Forecast_Direction_OHLCV/`     — source CSV file  
├── `Forecast_Direction_OHLCV.ipynb`        — main notebook with full analysis  
├── `Forecast_Direction_OHLCV.pdf`          — PDF export  
├── `requirements.txt`                      — project dependencies
└── `README.md`                             — this documentation
```
---

## 🚀 How to run
1. Clone the repository:
   ```bash
   git clone https://github.com/aspozd-DA-DS/DataAnalytics_projects.git
   ```
2. Go to the project folder:
   ```bash
   cd DataAnalytics_projects/04_Forecast_Direction_OHLCV
   ```
3. Run the notebook:
   ```bash
   jupyter notebook Forecast_Direction_OHLCV.ipynb
   ```

---
   
## 🏷 Topics
`Data Analysis` `EDA` `Visualization` `Time Series` `Financial Data` `Python` `Binary Classification` `Machine Learning` `XGBoost LightGBM` `SHAP Feature Engineering` `Trading Quantitative Finance`

<p align="right"><a href="#top">⬆ back to top</a></p>
