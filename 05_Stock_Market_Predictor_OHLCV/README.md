<a id="top"></a>

<p align="right">
  <a href="#ru">🇷🇺 RU</a> | <a href="#en">🇬🇧 EN</a>
</p>

---

<a id="ru"></a>

# Stock Market Predictor: прогнозирование направления движения цены

## 📌 Описание проекта

Проект представляет собой полный цикл анализа и построения модели бинарной классификации для прогнозирования направления движения цены акции на следующий торговый день. Включает EDA для временных рядов, feature engineering, проверку стационарности и сезонности, обучение моделей с TimeSeriesSplit, калибровку вероятностей и формулирование практических рекомендаций для трейдеров.

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
Используется синтетический датасет OHLCV (Open, High, Low, Close, Volume) с добавлением технических индикаторов (RSI, MACD) и сентимент-оценок за период с 2010 по 2062 год.

**Структура данных:**
- `date` — дата торгов (индекс)
- `open` — цена открытия
- `high` — максимальная цена за день
- `low` — минимальная цена за день
- `close` — цена закрытия
- `volume` — объём торгов
- `rsi` — индекс относительной силы (готовый индикатор)
- `macd` — схождение/расхождение скользящих средних (готовый индикатор)
- `sentiment` — синтетическая оценка рыночных настроений
- `target` — направление движения цены на следующий день (1 — рост, 0 — падение)

**Особенности данных:**
- 13 647 торговых дней
- Пропуски календарных дат — выходные и праздники (нормально для биржевых данных)
- Классы сбалансированы: 50.09% рост, 49.91% падение
- Целевая переменная строго соответствует `close[t+1] > close[t]`

---

## 📈 Ключевые этапы проекта

### Шаг 1: Загрузка и предобработка данных
- Загрузка CSV-файла, установка даты как индекса
- Проверка монотонности дат, пропусков, дубликатов
- Анализ целевой переменной: баланс классов, логическая проверка
- Верификация RSI и MACD (корреляция MACD = 0.999, RSI имеет модифицированную формулу)
- Проверка Sentiment Score на утечку данных (корреляция с target = 0.005)

### Шаг 2: Feature Engineering
- **Базовые признаки:** тело свечи, тени, внутридневная волатильность, позиция закрытия
- **Доходности и лаги:** лог-доходность, лаги доходности, лаги цены и объёма, гэп между сессиями
- **Скользящие статистики:** SMA 5/10/20/50, волатильность (rolling_std), относительный объём, расстояния до экстремумов
- **Технические индикаторы:** ROC (5), сигналы пересечения MACD, гистограмма MACD
- **Признаки сентимента:** лаги, скользящие средние, волатильность, изменение сентимента
- **Календарные признаки:** день недели, месяц, квартал
- **Итоговый набор:** 23 признака (исключены нестационарные ценовые ряды)

### Шаг 3: EDA для временных рядов
- Распределения: доходности имеют тяжёлые хвосты, не нормальны (kurtosis ≈ 83)
- Временные графики: долгосрочный восходящий тренд с несколькими циклами
- Декомпозиция: выраженная годовая сезонность, кластеры волатильности
- Автокорреляция: отсутствие линейной автокорреляции в доходностях
- Корреляционный анализ: 78 пар признаков с корреляцией >0.7
- Анализ сентимента: Sentiment Score имеет нулевую корреляцию с доходностью следующего дня (-0.009)

### Шаг 4: Подготовка к моделированию
- Отбор 23 стационарных признаков, исключение сырых цен
- Удаление NaN (первые ~600 строк)
- Разделение по времени: Train (2010–2045), Valid (2046–2053), Test (2054–2062)
- Масштабирование: RobustScaler (fit на train)
- One-Hot Encoding для `day_of_week` и `month` (только для логистической регрессии)

### Шаг 5: Моделирование
- **Бейзлайны:** most_frequent (F1=0.642), stratified (F1=0.503), naive (F1=0.492)
- **Logistic Regression:** C=0.05, VALID F1=0.632
- **Decision Tree:** max_depth=6, VALID F1=0.619
- **Random Forest:** n_estimators=1000, max_depth=9, VALID F1=0.634 — **лучшая модель**
- **XGBoost:** max_depth=4, VALID F1=0.630
- **LightGBM:** max_depth=5, VALID F1=0.625
- **LSTM:** VALID F1=0.757 (демонстрационный результат)
- **Калибровка:** Platt scaling и isotonic regression не дали значимого улучшения
- **Оптимальный порог:** 0.35 (F1=0.663, Recall=0.954)

### Шаг 6: Оценка на тесте
- **Метрики (порог 0.35):** F1=0.684, Recall=0.971, Precision=0.528, ROC-AUC=0.677
- **Матрица ошибок:** TP=930, FP=833, TN=80, FN=28
- **Стабильность:** средний F1 по месяцам 0.683, std=0.027, размах=0.079
- **Feature drift:** признаки волатильности имеют дрейф (ожидаемо), ключевые признаки стабильны

### Шаг 7: Интерпретация модели
- **Feature Importance (RandomForest):** топ-5 признаков
  - close_position (0.130), rsi_lag1 (0.108), rsi (0.099), macd_hist (0.094), dist_to_low_5 (0.057)
- **SHAP-анализ:**
  - macd_hist — главный дифференциатор TP vs FP (delta = +0.085)
  - rsi_lag1 — второй ключевой фильтр качества (delta = +0.050)
  - dist_to_high_5 — третий по значимости (delta = +0.023)
- **Логика модели:** моментум + позиция цены + волатильность + сентимент

---

## 📊 Основные выводы

### Ключевые признаки, связанные с направлением движения
- `close_position` — положение цены в дневном диапазоне (самый важный признак)
- `rsi_lag1` / `rsi` — инерция и текущее значение RSI
- `macd_hist` — гистограмма MACD (главный фильтр качества)
- `dist_to_low_5` / `dist_to_high_5` — расстояние до локальных экстремумов

### Технические индикаторы vs Sentiment Score

| Категория | Важность | Вывод |
|-----------|----------|-------|
| RSI и производные | ★★★★★ | Ключевые драйверы |
| Позиция цены | ★★★★★ | Самый важный признак |
| MACD (гистограмма) | ★★★★☆ | Важный фильтр качества |
| Сентимент | ★★☆☆☆ | Слабый, но стабильный вклад |
| MACD-пересечения | ★☆☆☆☆ | Почти не используются |

### Качественные сигналы vs ложные
- **Верные сигналы (TP):** положительный macd_hist, высокая инерция RSI, цена близко к экстремумам
- **Ложные сигналы (FP):** высокая волатильность, нейтральный RSI, цена далеко от экстремумов

### Стабильность признаков
- `close_position` и `rsi` стабильны на всех выборках
- `sentiment_ma_5` (сглаженный сентимент) стабильнее, чем сырой `sentiment`

### Рекомендации по выбору модели

| Сценарий | Рекомендуемая модель | Порог | Ожидаемый F1 |
|----------|---------------------|-------|--------------|
| Максимальный охват роста | RandomForest | 0.30 | 0.650 |
| Сбалансированный подход | **RandomForest** | **0.35** | **0.684** |
| Консервативный (мало ложных сигналов) | RandomForest | 0.50 | 0.658 |
| Максимальная точность (демонстрация) | LSTM | 0.50 | 0.757 |

### Ограничения исследования
- **Низкий Precision:** 52.8% точных сигналов → почти половина ложные
- **Модель предсказывает рост в 91% случаев** — не умеет предсказывать падения
- **Синтетические данные** — результаты могут не полностью соответствовать реальному рынку
- **Отсутствие учёта транзакционных издержек**

### Идеи для развития
- Стекинг моделей (RandomForest + XGBoost) для улучшения F1 до 0.70+
- Optuna для оптимизации гиперпараметров
- Фильтрация сигналов на основе SHAP-анализа (игнорировать сигналы при dist_to_low_5 > 0.2 или return_lag1 < 0)
- Expanding window валидация для увеличения объёма обучающих данных
- Внедрение динамического порога в зависимости от рыночной волатильности

---

## 📁 Структура репозитория

`05_Stock_Market_Predictor_OHLCV/`

```text
├── `Dataset_Stock_Market_Predictor_OHLCV/`  — исходный датасет 
├── `Stock_Market_Predictor_OHLCV.ipynb`     — основной ноутбук с полным анализом  
├── `Stock_Market_Predictor_OHLCV.pdf`       — экспорт в PDF  
├── `requirements.txt`                       — зависимости проекта
└── `README.md`                              — эта документация
```

---

## 🚀 Как запустить
1. Склонировать репозиторий:
   ```bash
   git clone https://github.com/aspozd-DA-DS/DataAnalytics_projects.git
   ```
2. Перейти в папку проекта:
   ```bash
   cd DataAnalytics_projects/05_Stock_Market_Predictor_OHLCV
   ```
3. Запустить ноутбук:
   ```bash
   jupyter notebook Stock_Market_Predictor_OHLCV.ipynb
   ```

---
  
## 🏷 Topics
`Data Analysis` `EDA` `Visualization` `Time Series` `Financial Data` `Python` `Binary Classification` `Machine Learning` `XGBoost LightGBM` `SHAP Feature Engineering` `Trading Quantitative Finance` `LSTM`

<p align="right"><a href="#top">⬆ наверх</a></p>

---

<a id="en"></a>

# Stock Market Predictor: forecasting price movement direction

## 📌 Project description

This project is a full analysis cycle and a binary classification model for forecasting the direction of a stock's price movement on the next trading day. It covers time series EDA, feature engineering, stationarity and seasonality checks, model training with TimeSeriesSplit, probability calibration, and practical recommendations for traders.

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
The project uses a synthetic OHLCV (Open, High, Low, Close, Volume) dataset enriched with technical indicators (RSI, MACD) and sentiment scores, covering 2010 to 2062.

**Data structure:**
- `date` — trading date (index)
- `open` — opening price
- `high` — highest price of the day
- `low` — lowest price of the day
- `close` — closing price
- `volume` — trading volume
- `rsi` — relative strength index (pre-computed indicator)
- `macd` — moving average convergence/divergence (pre-computed indicator)
- `sentiment` — synthetic market sentiment score
- `target` — price direction on the next day (1 — up, 0 — down)

**Data notes:**
- 13,647 trading days
- Gaps in calendar dates — weekends and holidays (normal for exchange data)
- Balanced classes: 50.09% up, 49.91% down
- The target strictly matches `close[t+1] > close[t]`

---

## 📈 Key project stages

### Step 1: Data loading and preprocessing
- Loading the CSV file, setting the date as the index
- Checking date monotonicity, gaps, and duplicates
- Analyzing the target: class balance, logical check
- Verifying RSI and MACD (MACD correlation = 0.999, RSI uses a modified formula)
- Checking the sentiment score for data leakage (correlation with target = 0.005)

### Step 2: Feature engineering
- **Basic features:** candle body, shadows, intraday volatility, close position
- **Returns and lags:** log return, return lags, price and volume lags, overnight gap
- **Rolling statistics:** SMA 5/10/20/50, volatility (rolling_std), relative volume, distances to extremes
- **Technical indicators:** ROC (5), MACD crossover signals, MACD histogram
- **Sentiment features:** lags, moving averages, volatility, sentiment change
- **Calendar features:** day of week, month, quarter
- **Final set:** 23 features (non-stationary price series excluded)

### Step 3: Time series EDA
- Distributions: returns have heavy tails and are not normal (kurtosis ≈ 83)
- Time plots: long-term upward trend with several cycles
- Decomposition: pronounced annual seasonality, volatility clusters
- Autocorrelation: no linear autocorrelation in returns
- Correlation analysis: 78 feature pairs with correlation >0.7
- Sentiment analysis: the sentiment score has zero correlation with next-day return (-0.009)

### Step 4: Preparing for modeling
- Selecting 23 stationary features, excluding raw prices
- Dropping NaN rows (first ~600 rows)
- Time-based split: Train (2010–2045), Valid (2046–2053), Test (2054–2062)
- Scaling: RobustScaler (fit on train)
- One-hot encoding for `day_of_week` and `month` (logistic regression only)

### Step 5: Modeling
- **Baselines:** most_frequent (F1=0.642), stratified (F1=0.503), naive (F1=0.492)
- **Logistic Regression:** C=0.05, VALID F1=0.632
- **Decision Tree:** max_depth=6, VALID F1=0.619
- **Random Forest:** n_estimators=1000, max_depth=9, VALID F1=0.634 — **best model**
- **XGBoost:** max_depth=4, VALID F1=0.630
- **LightGBM:** max_depth=5, VALID F1=0.625
- **LSTM:** VALID F1=0.757 (demonstration result)
- **Calibration:** Platt scaling and isotonic regression gave no significant improvement
- **Optimal threshold:** 0.35 (F1=0.663, Recall=0.954)

### Step 6: Test set evaluation
- **Metrics (threshold 0.35):** F1=0.684, Recall=0.971, Precision=0.528, ROC-AUC=0.677
- **Confusion matrix:** TP=930, FP=833, TN=80, FN=28
- **Stability:** average monthly F1 0.683, std=0.027, range=0.079
- **Feature drift:** volatility features drift (as expected), key features are stable

### Step 7: Model interpretation
- **Feature importance (RandomForest):** top 5 features
  - close_position (0.130), rsi_lag1 (0.108), rsi (0.099), macd_hist (0.094), dist_to_low_5 (0.057)
- **SHAP analysis:**
  - macd_hist — main differentiator between TP and FP (delta = +0.085)
  - rsi_lag1 — second key quality filter (delta = +0.050)
  - dist_to_high_5 — third in importance (delta = +0.023)
- **Model logic:** momentum + price position + volatility + sentiment

---

## 📊 Key findings

### Key features linked to the direction of movement
- `close_position` — position of the price within the daily range (the most important feature)
- `rsi_lag1` / `rsi` — RSI inertia and current value
- `macd_hist` — MACD histogram (main quality filter)
- `dist_to_low_5` / `dist_to_high_5` — distance to local extremes

### Technical indicators vs sentiment score

| Category | Importance | Conclusion |
|----------|------------|------------|
| RSI and derivatives | ★★★★★ | Key drivers |
| Price position | ★★★★★ | The most important feature |
| MACD (histogram) | ★★★★☆ | Important quality filter |
| Sentiment | ★★☆☆☆ | Weak but stable contribution |
| MACD crossovers | ★☆☆☆☆ | Almost unused |

### Quality signals vs false signals
- **True signals (TP):** positive macd_hist, high RSI inertia, price close to extremes
- **False signals (FP):** high volatility, neutral RSI, price far from extremes

### Feature stability
- `close_position` and `rsi` are stable across all samples
- `sentiment_ma_5` (smoothed sentiment) is more stable than the raw `sentiment`

### Model selection recommendations

| Scenario | Recommended model | Threshold | Expected F1 |
|----------|------------------|-----------|-------------|
| Maximum coverage of up-moves | RandomForest | 0.30 | 0.650 |
| Balanced approach | **RandomForest** | **0.35** | **0.684** |
| Conservative (few false signals) | RandomForest | 0.50 | 0.658 |
| Maximum accuracy (demonstration) | LSTM | 0.50 | 0.757 |

### Limitations of the study
- **Low precision:** 52.8% of signals are correct → almost half are false
- **The model predicts "up" in 91% of cases** — it does not predict declines well
- **Synthetic data** — results may not fully reflect a real market
- **Transaction costs not accounted for**

### Ideas for development
- Stacking models (RandomForest + XGBoost) to push F1 to 0.70+
- Optuna for hyperparameter optimization
- SHAP-based signal filtering (ignore signals when dist_to_low_5 > 0.2 or return_lag1 < 0)
- Expanding window validation to increase the amount of training data
- A dynamic threshold depending on market volatility

---

## 📁 Repository structure

`05_Stock_Market_Predictor_OHLCV/`

```text
├── `Dataset_Stock_Market_Predictor_OHLCV/`  — source dataset  
├── `Stock_Market_Predictor_OHLCV.ipynb`     — main notebook with full analysis  
├── `Stock_Market_Predictor_OHLCV.pdf`       — PDF export  
├── `requirements.txt`                       — project dependencies
└── `README.md`                              — this documentation
```

---

## 🚀 How to run
1. Clone the repository:
   ```bash
   git clone https://github.com/aspozd-DA-DS/DataAnalytics_projects.git
   ```
2. Go to the project folder:
   ```bash
   cd DataAnalytics_projects/05_Stock_Market_Predictor_OHLCV
   ```
3. Run the notebook:
   ```bash
   jupyter notebook Stock_Market_Predictor_OHLCV.ipynb
   ```

---
  
## 🏷 Topics
`Data Analysis` `EDA` `Visualization` `Time Series` `Financial Data` `Python` `Binary Classification` `Machine Learning` `XGBoost LightGBM` `SHAP Feature Engineering` `Trading Quantitative Finance` `LSTM`

<p align="right"><a href="#top">⬆ back to top</a></p>
