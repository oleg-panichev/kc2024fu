# Kaggle Competition 2024 for Ukrainians (1st place solution)

Цей репозиторій містить два рішення конкурсу [Kaggle Competition 2024 for Ukrainians](https://www.kaggle.com/competitions/ml-competition-2024-for-ukrainians) і короткий опис до кожного з них. Final рішення займає друге місце на [публічному лідерборді](https://www.kaggle.com/competitions/ml-competition-2024-for-ukrainians/leaderboard?tab=public) і перше місце [на приватному](https://www.kaggle.com/competitions/ml-competition-2024-for-ukrainians/leaderboard?).

## Baseline

- Використані всі ознаки
- Приведеня таргету до нормального розподілу за допомогою np.log1p()
- Label Encoder для категоріальних ознак
- 5 фолдів KFold
- Модель: LGBMRegressor
- Фінальний результат: середнє з 5 передбачень для тестової вибірки після кожного фолду


## Final

- Попередня обробка Item_Visibility із заміною нульових значень на середнє та приведенням розподілу до нормального за допомогою np.log1p()
- Приведеня таргету до нормального розподілу за допомогою np.log1p()
- Target Encoding для:
	- "Item_Identifier"
	- "Item_Type"
	- "Outlet_Identifier"
	- "Item_MRP"
	- "Item_Weight"
	- "Item_Visibility"
- Frequency Encoding для:
	- "Item_Identifier"
	- "Item_Type"
	- "Outlet_Identifier"
	- "Item_MRP"
	- "Item_Weight"
	- "Item_Visibility"
- Interaction-based Encoding для пар:
	- ("Item_Identifier", "Item_MRP")
	- ("Item_Identifier", "Item_Weight")
	- ("Item_Identifier", "Item_Visibility")
	- ("Item_Type", "Item_MRP")
	- ("Item_Type", "Item_Weight")
	- ("Item_Type", "Item_Visibility")
	- ("Outlet_Identifier", "Item_MRP")
	- ("Outlet_Identifier", "Item_Weight")
	- ("Outlet_Identifier", "Item_Visibility")
- Label Encoder для категоріальних ознак
- Генерація окремих ознак для лінійних моделей та моделей, які в основі мають дерева рішень
- Ознаки для моделей, які базуються на деревах рішень (27 ознак):
	- Вилучено колонки "Outlet_Establishment_Year", "Item_Fat_Content", "Outlet_Size", "Outlet_Location_Type", "Outlet_Type"
- Ознаки для лінійних моделей (1617 ознак):
	- Вилучено колонки "Outlet_Establishment_Year", "Item_Fat_Content", "Outlet_Size", "Outlet_Location_Type", "Outlet_Type"
	- Додано ознаку "Item_MRP_class", яка ділить "Item_MRP" на 4 класи
	- One-hot encoding колонок "Item_Identifier", "Item_Type", "Outlet_Identifier", "Item_MRP_class"
	- StandardScaler для всіх числових ознак
- 5 фолдів KFold
- Моделі:
	- LGBMRegressor
	- XGBRegressor
	- HistGradientBoostingRegressor
	- LinearRegression
	- RandomForestRegressor
- Гіперпараметри моделей були оптимізовані за допомого optuna в одній із попередніх ітерацій на меншій кількості ознак
- Фінальний результат: натренована LinearRegression модель для обʼєднання передбачень всіх моделей


# Метрики

|          | Validation | Public LB | Private LB |
|----------|------------|-----------|------------|
| Baseline | 0.707058   | 0.70132   | 0.70038    |
| Final    | 0.694096   | 0.69253   | 0.69087    |
