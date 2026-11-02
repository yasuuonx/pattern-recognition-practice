# Порівняння класичних моделей класифікації (Мініпроєкт ПР4–ПР7)

## Постановка задачі
Автоматичне визначення наявності ознак серцевого захворювання за клінічними характеристиками пацієнта (бінарна класифікація, позитивний клас — 1).

## Набір даних
- **Джерело:** Heart Disease Dataset (UCI Machine Learning Repository).
- **Обсяг:** 303 об'єкти, 13 вхідних ознак (5 числових, 8 категоріальних).
- **Цільова змінна:** `target` (0 — відсутність захворювання, 1 — наявність).

## Методика експерименту
- Фіксований стратифікований поділ Train/Test (80% / 20%).
- Усі перетворення (`SimpleImputer`, `StandardScaler`, `OneHotEncoder`) та підбір гіперпараметрів виконувалися виключно на `X_train` через 5-Fold `StratifiedKFold`.
- `X_test` закритий і використовувався лише один раз для фінальної оцінки зафіксованого `Pipeline`.

## Досліджені моделі
1. Logistic Regression (L1/L2)
2. k-NN (k-Nearest Neighbors)
3. Gaussian Naive Bayes
4. LDA (Linear Discriminant Analysis)
5. QDA (Quadratic Discriminant Analysis)
6. SVM (Linear & RBF Kernel)

## Результати крос-валідації (на X_train)
Зведена таблиця збережена у файлі `reports/cv_model_comparison.csv`:

| Model | CV F1 (mean) | CV F1 (std) | CV Recall (mean) | CV ROC AUC (mean) |
|:---|---:|---:|---:|---:|
| Logistic Regression | 0.8363 | 0.0127 | 0.8016 | 0.8974 |
| SVM | 0.8270 | 0.0330 | 0.7830 | 0.8968 |
| k-NN | 0.8175 | 0.0304 | 0.7917 | 0.8768 |
| LDA | 0.8128 | 0.0257 | 0.7656 | 0.8993 |
| QDA | 0.7962 | 0.0271 | 0.7743 | 0.8680 |
| GaussianNB | 0.7495 | 0.0215 | 0.8458 | 0.8773 |

## Фінальна модель і результати на X_test
- **Обрана модель:** Logistic Regression
- **Обґрунтування:** Модель продемонструвала найвищий середній показник `CV F1 = 0.8363` та `CV ROC AUC = 0.8974` з найменшою дисперсією між фолдами (`std = 0.0127`), що свідчить про відмінну стабільність та узагальнювальну здатність.
- **Результати на відкладеному X_test (`reports/final_test_metrics.csv`):**
  - **Test F1:** 0.8621
  - **Test ROC AUC:** 0.9578
  - **Test Accuracy:** 0.8689
  - **Test Recall:** 0.8929
  - **Test Precision:** 0.8333

## Запуск
1. Відкрити блокнот у Google Colab: [PR04_Demchenko.ipynb](notebooks/PR04_Demchenko.ipynb).
2. Послідовно виконати всі комірки (`Runtime → Run all`).
