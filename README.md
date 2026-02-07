# Personalized Patient Meal Planning with Genetic Algorithms

A **patient meal planning system** that uses a **genetic algorithm (GA)** to generate weekly meal plans tailored to a patient’s nutritional targets, allergies, and budget. The fitness function is combined with **machine learning** modules for satisfaction prediction and cost-under-uncertainty, so the optimizer balances nutrition, cost, and predicted satisfaction in a single objective.

## What It Does

- **Meal database**: 32 meals (8 breakfasts, 12 lunches, 12 dinners) with calories, protein, fat, carbs, cost, and allergy tags (e.g. nuts, gluten, dairy).
- **Patient profile**: Daily calorie and protein targets, allergy list (e.g. nuts), and weekly budget (e.g. in Rs).
- **Chromosome**: A candidate solution is **21 integers** — one meal index per slot for 7 days × 3 meals (breakfast, lunch, dinner). Meal type is enforced (e.g. breakfast slot only chooses from breakfast meals).
- **Fitness**: Combines:
  - **Nutrition score**: How close average daily calories and protein are to targets.
  - **Cost score**: Weekly cost vs budget, using **expected cost under uncertainty** (Monte Carlo over price volatility).
  - **Satisfaction score**: **Random Forest** predictor trained on synthetic meal-plan features (avg calories, protein, cost ratio, variety, balance).
  - **Penalties**: Allergen violations, excessive meal repetition, and very off-target daily calories.
- **Evolution**: Implemented with **DEAP** (Distributed Evolutionary Algorithms in Python): tournament selection, **day-block crossover** (swap whole days between two plans), and **mutation** (replace a meal with another of the same type).

The notebook runs the GA, plots fitness progression, and exports the best plan plus evolution log and meal DB as CSVs.

## Repository Contents

- **`meal_planner.ipynb`** — Main Jupyter notebook: meal DB setup, patient profile, ML models, GA (DEAP), fitness evaluation, and analysis/plots.
- **`meals_db_deap.csv`** — Meal database used by the GA (meal_id, type, calories, protein, fat, carbs, cost, tags, int_id).
- **`deap_best_weekly_plan.csv`** — Best weekly plan found (per-day breakfast/lunch/dinner IDs and daily calories, protein, cost).
- **`deap_logbook.csv`** — Evolution log (per generation: gen, nevals, avg/min/max fitness).

