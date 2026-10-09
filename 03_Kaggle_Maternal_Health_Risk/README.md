<a id="top"></a>

<p align="right">
  <a href="#ru">🇷🇺 RU</a> | <a href="#en">🇬🇧 EN</a>
</p>

---

<a id="ru"></a>

# Maternal Health Risk: прогноз и анализ рисков беременности

## 📌 Описание проекта

Проект представляет собой полный цикл анализа и построения модели многоклассовой классификации уровня риска беременности (low / mid / high) на основе физиологических показателей. Включает EDA, feature engineering, проверку статистических гипотез, обучение моделей и формулирование практических рекомендаций для врачей.

---

**Ключевые цели проекта:**
- Построение интерпретируемой модели классификации уровня риска.
- Выявление ключевых медицинских факторов, влияющих на риск.
- Формирование профилей пациенток с высоким риском.
- Подготовка рекомендаций для врачей и системы мониторинга.

**Практическая ценность:**
- Автоматическая предварительная сортировка пациенток по группам риска.
- Поддержка врачебных решений и профилактика осложнений.
- Оптимизация нагрузки на медицинский персонал.

---

## 🎯 Цели исследования
1. Построить модель, предсказывающую уровень риска беременности.
2. Выявить ключевые физиологические показатели, влияющие на риск.
3. Проверить медицинские гипотезы (давление, глюкоза, возраст).
4. Сформулировать рекомендации для врачей.
5. Оценить модель по F1-macro и Recall для high risk.

---

## 🔧 Технический стек
- **Язык программирования:** Python  
- **Анализ данных:** Pandas, NumPy  
- **Визуализация:** Matplotlib, Seaborn  
- **Статистика:** SciPy, `phik`  
- **Машинное обучение:** Scikit-learn, XGBoost, LightGBM, CatBoost  
- **Верификация гипотез:** ANOVA, t-test, Mann–Whitney, χ²  
- **Автоматизация загрузки данных:** Kaggle API, gdown

---

## 📊 Данные
Используется датасет **Maternal Health Risk Data** с Kaggle:  
[Ссылка на датасет](https://www.kaggle.com/datasets/csafrit2/maternal-health-risk-data)

**Структура данных (6 признаков + целевая переменная):**
- `Age` — возраст  
- `SystolicBP`, `DiastolicBP` — артериальное давление  
- `BS` — уровень глюкозы  
- `BodyTemp` — температура тела  
- `HeartRate` — пульс  
- `RiskLevel` — целевая переменная: low / mid / high

---

## 📈 Ключевые этапы проекта
1. **Загрузка и предобработка данных** — очистка, кодирование, создание медицинских индексов (MAP, PulsePressure).
2. **EDA** — распределения, выбросы, пороговый анализ, сравнение по классам риска.
3. **Проверка гипотез** — различия между группами, влияние давления, глюкозы, возраста.
4. **Подготовка данных для ML** — стратификация, масштабирование, кодирование.
5. **Моделирование** — сравнение Logistic Regression, Decision Tree, Random Forest, XGBoost, LightGBM, CatBoost.
6. **Интерпретация моделей** — важность признаков, критические пороги.
7. **Оценка модели** — F1-macro, Recall для high risk, матрицы ошибок, ROC-AUC.
8. **Формирование отчёта и рекомендаций** — профили риска, практические шаги для врачей.

---

## 📊 Основные выводы
- **Ключевые факторы риска:** высокое артериальное давление, гипергликемия, отклонения температуры, пульс > 100.
- **Лучшие модели:** CatBoost и LightGBM показали наилучший баланс F1-macro и Recall для high risk.
- **Критические пороги:** SystolicBP > 140, BS > 7.0, TempDeviation > 1.5°F.
- **Группы риска:** женщины старше 35 лет с гипертензией и гипергликемией.
- **Рекомендации:** ранняя диагностика, дополнительное обследование, мониторинг давления и глюкозы.

---

## 📁 Структура репозитория

`03_Kaggle_Maternal_Health_Risk/`

```text
├── `Dataset_Kaggle_maternal_health/`          — исходный CSV-файл 
├── `Maternal_Health_Risk.ipynb`               — основной ноутбук с полным анализом  
├── `Maternal_Health_Risk.pdf`                 — экспорт в PDF  
├── `catboost_info/`                           — логи обучения CatBoost  
├── `requirements.txt`                         — зависимости проекта
└── `README.md`                                — эта документация
```

---

## 🚀 Как запустить
1. Склонировать репозиторий:
   ```bash
   git clone https://github.com/aspozd-DA-DS/DataAnalytics_projects.git
   ```
2. Перейти в папку проекта:
   ```bash
   cd DataAnalytics_projects/03_Kaggle_Maternal_Health_Risk
   ```
3. Запустить ноутбук:
   ```bash
   jupyter notebook "Maternal_Health_Risk.ipynb"
   ```

---
   
## 🏷 Topics
`Data Analysis` `EDA` `Visualization` `Kaggle` `Python` `Multiclass Classification` `Healthcare`

<p align="right"><a href="#top">⬆ наверх</a></p>

---

<a id="en"></a>

# Maternal Health Risk: forecasting and analysis of pregnancy risks

## 📌 Project description

This project is a full analysis cycle and a multiclass classification model for the pregnancy risk level (low / mid / high) based on physiological indicators. It covers EDA, feature engineering, statistical hypothesis testing, model training, and practical recommendations for doctors.

---

**Key project goals:**
- Build an interpretable risk level classification model.
- Identify the key medical factors that influence risk.
- Build profiles of high-risk patients.
- Prepare recommendations for doctors and monitoring systems.

**Practical value:**
- Automatic preliminary triage of patients into risk groups.
- Support for clinical decisions and prevention of complications.
- Reducing the workload of medical staff.

---

## 🎯 Research goals
1. Build a model that predicts the pregnancy risk level.
2. Identify the key physiological indicators that influence risk.
3. Test medical hypotheses (blood pressure, glucose, age).
4. Formulate recommendations for doctors.
5. Evaluate the model by F1-macro and Recall for the high-risk class.

---

## 🔧 Technical stack
- **Programming language:** Python  
- **Data analysis:** Pandas, NumPy  
- **Visualization:** Matplotlib, Seaborn  
- **Statistics:** SciPy, `phik`  
- **Machine learning:** Scikit-learn, XGBoost, LightGBM, CatBoost  
- **Hypothesis testing:** ANOVA, t-test, Mann–Whitney, χ²  
- **Automated data download:** Kaggle API, gdown

---

## 📊 Data
The project uses the **Maternal Health Risk Data** dataset from Kaggle:  
[Dataset link](https://www.kaggle.com/datasets/csafrit2/maternal-health-risk-data)

**Data structure (6 features + target variable):**
- `Age` — age  
- `SystolicBP`, `DiastolicBP` — blood pressure  
- `BS` — blood glucose level  
- `BodyTemp` — body temperature  
- `HeartRate` — heart rate  
- `RiskLevel` — target variable: low / mid / high

---

## 📈 Key project stages
1. **Data loading and preprocessing** — cleaning, encoding, creating medical indices (MAP, PulsePressure).
2. **EDA** — distributions, outliers, threshold analysis, comparison across risk classes.
3. **Hypothesis testing** — differences between groups, effects of blood pressure, glucose, and age.
4. **Preparing data for ML** — stratification, scaling, encoding.
5. **Modeling** — comparing Logistic Regression, Decision Tree, Random Forest, XGBoost, LightGBM, and CatBoost.
6. **Model interpretation** — feature importance, critical thresholds.
7. **Model evaluation** — F1-macro, Recall for the high-risk class, confusion matrices, ROC-AUC.
8. **Report and recommendations** — risk profiles, practical steps for doctors.

---

## 📊 Key findings
- **Main risk factors:** high blood pressure, hyperglycemia, body temperature deviations, pulse > 100.
- **Best models:** CatBoost and LightGBM showed the best balance of F1-macro and Recall for the high-risk class.
- **Critical thresholds:** SystolicBP > 140, BS > 7.0, TempDeviation > 1.5°F.
- **Risk groups:** women over 35 with hypertension and hyperglycemia.
- **Recommendations:** early diagnosis, additional examinations, monitoring of blood pressure and glucose.

---

## 📁 Repository structure

`03_Kaggle_Maternal_Health_Risk/`

```text
├── `Dataset_Kaggle_maternal_health/`          — source CSV file  
├── `Maternal_Health_Risk.ipynb`               — main notebook with full analysis  
├── `Maternal_Health_Risk.pdf`                 — PDF export  
├── `catboost_info/`                           — CatBoost training logs  
├── `requirements.txt`                         — project dependencies
└── `README.md`                                — this documentation
```

---

## 🚀 How to run
1. Clone the repository:
   ```bash
   git clone https://github.com/aspozd-DA-DS/DataAnalytics_projects.git
   ```
2. Go to the project folder:
   ```bash
   cd DataAnalytics_projects/03_Kaggle_Maternal_Health_Risk
   ```
3. Run the notebook:
   ```bash
   jupyter notebook "Maternal_Health_Risk.ipynb"
   ```

---
   
## 🏷 Topics
`Data Analysis` `EDA` `Visualization` `Kaggle` `Python` `Multiclass Classification` `Healthcare`

<p align="right"><a href="#top">⬆ back to top</a></p>
