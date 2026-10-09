<a id="top"></a>

<p align="right">
  <a href="#ru">🇷🇺 RU</a> | <a href="#en">🇬🇧 EN</a>
</p>

---

<a id="ru"></a>

# Анализ ценовых факторов на рынке смартфонов (2025)

## 📌 Описание
Этот проект представляет собой комплексное исследование рынка смартфонов, направленное на выявление ключевых факторов, влияющих на цену устройств, и анализ региональных различий в стоимости и доступности.
В рамках анализа были изучены цены, технические характеристики, валюты, уровень доходов и региональные особенности рынка смартфонов в 2025 году.

---

## Цели исследования
1.  Определить, какие технические характеристики (RAM, камеры, процессор и т.д.) и нематериальные факторы (бренд, год выпуска) сильнее всего влияют на цену.
2.  Сравнить уровень цен и **доступность** смартфонов в 5 странах: Пакистан, Индия, Китай, США, ОАЭ. Доступность рассчитывалась как отношение цены смартфона к медианной месячной зарплате в стране.
3.  Выявить арбитражные возможности — модели и страны, где покупка с целью перепродажи в другом регионе наиболее выгодна.
4.  Проверить статистические гипотезы о различии цен между странами и брендами.

---

## 🔧 Стек
*   **Языки программирования:** Python
*   **Библиотеки для анализа данных:** Pandas, NumPy
*   **Библиотеки для визуализации:** Matplotlib, Seaborn
*   **Статистический анализ:** SciPy (проверка гипотез), `phik` (корреляционный анализ)
*   **Машинное обучение:** Scikit-learn (GradientBoostingRegressor для импутации пропусков)
*   **Система контроля версий:** Git

---

## 📊 Данные
Датасет автоматически скачивается из Kaggle:  
[Mobiles Dataset (2025)](https://www.kaggle.com/datasets/abdulmalik1518/mobiles-dataset-2025)
- Датасет содержит 930 строк с характеристиками смартфонов и их стартовыми ценами в разных странах.
  - 🇵🇰 Пакистан
  - 🇮🇳 Индия
  - 🇨🇳 Китай
  - 🇺🇸 США
  - 🇦🇪 Дубай

Всего: 930 моделей / 15 признаков

---

## Ключевые этапы проекта
1.  **Предобработка данных:** Очистка от артефактов, преобразование типов, обработка пропусков с помощью ML-модели, конвертация валют в USD.
2.  **Исследовательский анализ данных (EDA):** Анализ распределений, поиск корреляций, сравнение брендов и стран.
3.  **Анализ доступности:** Расчет метрики `affordability_index` и сравнение стран.
4.  **Проверка гипотез:** Статистическое подтверждение различий в ценах между странами и влияния характеристик на стоимость.
5.  **Формирование выводов и рекомендаций** для разных стейкхолдеров.

---

## 📊 Выводы
- Цена определяется комплексно: бренд, RAM и год выпуска — ключевые драйверы.
- США и ОАЭ — самые доступные рынки (0.1–0.13 месячной зарплаты).
- Пакистан — самые низкие цены, но наименее доступные из-за уровня доходов.
- Выявлены модели с разницей цен до 123% между странами → потенциал перепродажи.

---

## 📁 Структура репозитория

`01_Kaggle_Price-phone-analysis-master/`

```text
├── `Dataset_Kaggle_Price-phone-analysis-master/` — исходный CSV-файл 
├── `Price-phone-analysis-master.ipynb`           — основной ноутбук с полным анализом  
├── `Price_phone_analysis_master.pdf`             — экспорт в PDF  
├── `requirements.txt`                            — зависимости проекта
└── `README.md`                                   — эта документация
```

---

## 🚀 Как запустить
1. Склонировать репозиторий:
   ```bash
   git clone https://github.com/aspozd-DA-DS/DataAnalytics_ML_projects.git
   ```
2. Перейти в папку проекта:
   ```bash
   cd DataAnalytics_ML_projects/01_Kaggle_Price-phone-analysis-master
   ```
3. Запустить ноутбук:
   ```bash
   jupyter notebook Price-phone-analysis-master.ipynb
   ```
4. Убедитесь, что у вас настроен Kaggle API для автоматической загрузки данных

---
  
## 🏷 Topics
`Data Analysis` `EDA` `Visualization` `Kaggle` `Python`

<p align="right"><a href="#top">⬆ наверх</a></p>

---

<a id="en"></a>

# Smartphone price factor analysis (2025)

## 📌 Description
This project is a comprehensive study of the smartphone market, aimed at identifying the key factors that influence device prices and analyzing regional differences in cost and affordability.
The analysis examined prices, technical specifications, currencies, income levels, and regional market features for smartphones in 2025.

---

## Research goals
1.  Determine which technical specifications (RAM, cameras, processor, etc.) and non-material factors (brand, release year) influence price the most.
2.  Compare price levels and smartphone **affordability** across 5 countries: Pakistan, India, China, the USA, and the UAE. Affordability was calculated as the ratio of a smartphone's price to the median monthly salary in the country.
3.  Identify arbitrage opportunities — models and countries where buying for resale in another region is most profitable.
4.  Test statistical hypotheses about price differences between countries and brands.

---

## 🔧 Stack
*   **Programming language:** Python
*   **Data analysis libraries:** Pandas, NumPy
*   **Visualization libraries:** Matplotlib, Seaborn
*   **Statistical analysis:** SciPy (hypothesis testing), `phik` (correlation analysis)
*   **Machine learning:** Scikit-learn (GradientBoostingRegressor for missing value imputation)
*   **Version control:** Git

---

## 📊 Data
The dataset is downloaded automatically from Kaggle:  
[Mobiles Dataset (2025)](https://www.kaggle.com/datasets/abdulmalik1518/mobiles-dataset-2025)
- The dataset contains 930 rows with smartphone specifications and their starting prices in different countries.
  - 🇵🇰 Pakistan
  - 🇮🇳 India
  - 🇨🇳 China
  - 🇺🇸 USA
  - 🇦🇪 UAE (Dubai)

Total: 930 models / 15 features

---

## Key project stages
1.  **Data preprocessing:** Cleaning artifacts, converting data types, handling missing values with an ML model, converting currencies to USD.
2.  **Exploratory data analysis (EDA):** Distribution analysis, correlation search, comparison of brands and countries.
3.  **Affordability analysis:** Calculating the `affordability_index` metric and comparing countries.
4.  **Hypothesis testing:** Statistical confirmation of price differences between countries and of the influence of features on cost.
5.  **Conclusions and recommendations** for different stakeholders.

---

## 📊 Conclusions
- Price is determined by a combination of factors: brand, RAM, and release year are the key drivers.
- The USA and UAE are the most affordable markets (0.1–0.13 of a monthly salary).
- Pakistan has the lowest prices, but is the least affordable due to income levels.
- Models with price differences of up to 123% between countries were identified → resale potential.

---

## 📁 Repository structure

`01_Kaggle_Price-phone-analysis-master/`

```text
├── `Dataset_Kaggle_Price-phone-analysis-master/` — source CSV file 
├── `Price-phone-analysis-master.ipynb`           — main notebook with full analysis  
├── `Price_phone_analysis_master.pdf`             — PDF export  
├── `requirements.txt`                            — project dependencies
└── `README.md`                                   — this documentation
```

---

## 🚀 How to run
1. Clone the repository:
   ```bash
   git clone https://github.com/aspozd-DA-DS/DataAnalytics_ML_projects.git
   ```
2. Go to the project folder:
   ```bash
   cd DataAnalytics_ML_projects/01_Kaggle_Price-phone-analysis-master
   ```
3. Run the notebook:
   ```bash
   jupyter notebook Price-phone-analysis-master.ipynb
   ```
4. Make sure the Kaggle API is configured for automatic data download

---
  
## 🏷 Topics
`Data Analysis` `EDA` `Visualization` `Kaggle` `Python`

<p align="right"><a href="#top">⬆ back to top</a></p>
