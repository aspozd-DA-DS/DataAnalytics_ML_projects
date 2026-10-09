<a id="top"></a>

<p align="right">
  <a href="#ru">🇷🇺 RU</a> | <a href="#en">🇬🇧 EN</a>
</p>

---

<a id="ru"></a>

# HR Burnout Lab: прогноз и анализ риска выгорания сотрудников

## 📌 Описание проекта

Проект представляет собой комплексное исследование и решение задачи прогнозирования риска профессионального выгорания сотрудников на основе синтетических HR-данных. Включает полный цикл анализа: от исследования данных и проверки гипотез до построения интерпретируемых ML-моделей и формулирования практических рекомендаций для HR-отдела.

**Ключевые цели проекта:**
- Выявление ключевых факторов, влияющих на риск выгорания.
- Построение интерпретируемой модели бинарной классификации.
- Формирование профилей сотрудников с высоким риском.
- Подготовка управленческих рекомендаций для профилактики выгорания.

**Практическая ценность:**
- Раннее выявление сотрудников группы риска.
- Оптимизация рабочих процессов и программ поддержки.
- Улучшение условий труда и снижение текучести кадров.

---

## 🎯 Цели исследования
1. Выявить факторы риска выгорания (рабочие часы, стресс, удовлетворённость и др.).
2. Определить профили сотрудников с высоким риском.
3. Построить и оценить интерпретируемые модели классификации.
4. Проверить HR-гипотезы (влияние удалёнки, стажа, должностей).
5. Сформулировать управленческие рекомендации для HR.

---

## 🔧 Технический стек
- **Язык программирования:** Python
- **Анализ данных:** Pandas, NumPy
- **Визуализация:** Matplotlib, Seaborn
- **Статистика:** SciPy, `phik` (корреляционный анализ)
- **Машинное обучение:** Scikit-learn, XGBoost, LightGBM, CatBoost
- **Верификация гипотез:** статистические тесты (ANOVA, t-test, корреляции)
- **Автоматизация загрузки данных:** Kaggle API

---

## 📊 Данные
Используется синтетический датасет **Synthetic HR Burnout Dataset** с Kaggle:  
[Ссылка на датасет](https://www.kaggle.com/datasets/ankam6010/synthetic-hr-burnout-dataset)

**Структура данных (10 признаков):**
- `Name` — имя сотрудника (синтетическое, удаляется)
- `Age`, `Gender`, `JobRole`, `Experience`
- `WorkHoursPerWeek`, `RemoteRatio`, `SatisfactionLevel`, `StressLevel`
- `Burnout` — целевая переменная (1 — есть выгорание, 0 — нет)

---

## 📈 Ключевые этапы проекта
1. **Загрузка и предобработка данных** — очистка, кодирование, создание новых признаков.
2. **Исследовательский анализ (EDA)** — распределения, корреляции, пороговый анализ, профилирование.
3. **Проверка статистических гипотез** — влияние часов, стресса, удалёнки, должностей.
4. **Подготовка данных для ML** — стратификация, масштабирование, кодирование.
5. **Моделирование** — сравнение Logistic Regression, Random Forest, XGBoost, LightGBM, CatBoost.
6. **Интерпретация моделей** — важность признаков, бизнес-инсайты.
7. **Формирование отчёта и рекомендаций** — сегментация риска, практические шаги для HR.

---

## 📊 Основные выводы
- **Ключевые факторы риска:** низкая удовлетворённость, переработки (>50 ч/неделю), крайние форматы удалёнки (0% или 100%).
- **Лучшие модели:** LightGBM и CatBoost показали наилучший баланс Recall (~85–95%) и Precision (~36–37%).
- **Критические пороги:** 50 рабочих часов, 7 баллов стресса, 3 балла удовлетворённости.
- **Группы риска:** сотрудники отделов Sales, Manager, HR; молодые (22–30 лет) и предпенсионные (51–60 лет).
- **Рекомендации для HR:** лимит часов, гибридный формат работы, мониторинг удовлетворённости, ранние алерты.

---

## 📁 Структура репозитория

`02_Kaggle_HR_Burnout_Lab/`

```text
├── `Dataset_Kaggle_employee_burnout/`    — исходный CSV-файл 
├── `HR_Burnout_Lab.ipynb`                — основной ноутбук с полным анализом  
├── `HR_Burnout_Lab.pdf`                  — экспорт в PDF 
├── `requirements.txt`                    — зависимости проекта 
└── `README.md`                           — эта документация
```

---

## 🚀 Как запустить
1. Склонировать репозиторий:
   ```bash
   git clone https://github.com/aspozd-DA-DS/DataAnalytics_projects.git
   ```
2. Перейти в папку проекта:
   ```bash
   cd DataAnalytics_projects/02_Kaggle_HR_Burnout_Lab
   ```
3. Запустить ноутбук:
   ```bash
   jupyter notebook "HR_Burnout_Lab.ipynb"
   ```
4. Убедитесь, что у вас настроен Kaggle API для автоматической загрузки данных

---
   
## 🏷 Topics
`Data Analysis` `EDA` `Visualization` `Kaggle` `Python`

<p align="right"><a href="#top">⬆ наверх</a></p>

---

<a id="en"></a>

# HR Burnout Lab: forecasting and analysis of employee burnout risk

## 📌 Project description

This project is a comprehensive study and solution to the task of predicting the risk of professional burnout among employees, based on synthetic HR data. It covers the full analysis cycle: from data exploration and hypothesis testing to building interpretable ML models and formulating practical recommendations for the HR department.

**Key project goals:**
- Identify the key factors that influence burnout risk.
- Build an interpretable binary classification model.
- Build profiles of high-risk employees.
- Prepare management recommendations for burnout prevention.

**Practical value:**
- Early identification of employees at risk.
- Optimization of work processes and support programs.
- Improvement of working conditions and reduction of staff turnover.

---

## 🎯 Research goals
1. Identify burnout risk factors (working hours, stress, satisfaction, etc.).
2. Determine profiles of high-risk employees.
3. Build and evaluate interpretable classification models.
4. Test HR hypotheses (the impact of remote work, tenure, and job roles).
5. Formulate management recommendations for HR.

---

## 🔧 Technical stack
- **Programming language:** Python
- **Data analysis:** Pandas, NumPy
- **Visualization:** Matplotlib, Seaborn
- **Statistics:** SciPy, `phik` (correlation analysis)
- **Machine learning:** Scikit-learn, XGBoost, LightGBM, CatBoost
- **Hypothesis testing:** statistical tests (ANOVA, t-test, correlations)
- **Automated data download:** Kaggle API

---

## 📊 Data
The project uses the synthetic **Synthetic HR Burnout Dataset** from Kaggle:  
[Dataset link](https://www.kaggle.com/datasets/ankam6010/synthetic-hr-burnout-dataset)

**Data structure (10 features):**
- `Name` — employee name (synthetic, dropped)
- `Age`, `Gender`, `JobRole`, `Experience`
- `WorkHoursPerWeek`, `RemoteRatio`, `SatisfactionLevel`, `StressLevel`
- `Burnout` — target variable (1 — burnout, 0 — no burnout)

---

## 📈 Key project stages
1. **Data loading and preprocessing** — cleaning, encoding, creating new features.
2. **Exploratory data analysis (EDA)** — distributions, correlations, threshold analysis, profiling.
3. **Statistical hypothesis testing** — the effects of hours, stress, remote work, and job roles.
4. **Preparing data for ML** — stratification, scaling, encoding.
5. **Modeling** — comparing Logistic Regression, Random Forest, XGBoost, LightGBM, and CatBoost.
6. **Model interpretation** — feature importance, business insights.
7. **Report and recommendations** — risk segmentation, practical steps for HR.

---

## 📊 Key findings
- **Main risk factors:** low satisfaction, overtime (>50 h/week), extreme remote-work ratios (0% or 100%).
- **Best models:** LightGBM and CatBoost showed the best balance of Recall (~85–95%) and Precision (~36–37%).
- **Critical thresholds:** 50 working hours, a stress level of 7, a satisfaction score of 3.
- **Risk groups:** employees in Sales, Manager, and HR departments; young (22–30) and pre-retirement (51–60) employees.
- **Recommendations for HR:** hour limits, hybrid work format, satisfaction monitoring, early alerts.

---

## 📁 Repository structure

`02_Kaggle_HR_Burnout_Lab/`

```text
├── `Dataset_Kaggle_employee_burnout/`    — source CSV file 
├── `HR_Burnout_Lab.ipynb`                — main notebook with full analysis  
├── `HR_Burnout_Lab.pdf`                  — PDF export 
├── `requirements.txt`                    — project dependencies 
└── `README.md`                           — this documentation
```

---

## 🚀 How to run
1. Clone the repository:
   ```bash
   git clone https://github.com/aspozd-DA-DS/DataAnalytics_projects.git
   ```
2. Go to the project folder:
   ```bash
   cd DataAnalytics_projects/02_Kaggle_HR_Burnout_Lab
   ```
3. Run the notebook:
   ```bash
   jupyter notebook "HR_Burnout_Lab.ipynb"
   ```
4. Make sure the Kaggle API is configured for automatic data download

---
   
## 🏷 Topics
`Data Analysis` `EDA` `Visualization` `Kaggle` `Python`

<p align="right"><a href="#top">⬆ back to top</a></p>
