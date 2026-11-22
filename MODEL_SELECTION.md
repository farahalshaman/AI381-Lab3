# Model selection

## The question

The model predicts whether a telecom customer is likely to leave the company (customer churn). A false negative may cause the company to lose a customer without taking preventive action, while a false positive may lead to unnecessary retention offers and additional costs for the company.

## Candidates

| Candidate | Why I tried it | AUC | F1 | F1 per slice (lowest) | p95 latency (ms) | Size (KB) | MLflow run ID |
|---|---|---:|---:|---:|---:|---:|---|
| Logistic Regression | A simple, interpretable baseline with low computational cost. | 0.7982 | 0.4948 | 0.2500 | 6.1497 | 3.5479 | `7815f6a039304d0290ea6b56e7d2262b` |
| Random Forest | A tree-based ensemble that can capture nonlinear relationships. | 0.7589 | 0.4536 | 0.2222 | 23.8402 | 3160.0957 | `7eef03a856ab4aa1b8e54f71035875d5` |
| Gradient Boosting | An ensemble method that builds trees sequentially to improve predictions. | 0.7476 | 0.4167 | 0.2222 | 8.4417 | 141.6445 | `c19be2329edb40f4b82d7c726c8b93fb` |

## The champion

I selected Logistic Regression as the champion.

It achieved the highest overall AUC (0.7982) and F1 score (0.4948). It also had the lowest p95 latency (6.1497 ms) and the smallest model size (3.5479 KB).

I accepted that its F1 score for the one-year and two-year contract slices was only 0.2500. Although Random Forest and Gradient Boosting offered different modeling approaches, neither achieved better overall performance. Logistic Regression provided the best balance between predictive performance, inference speed, and model size.

## My promotion rules

- Latency budget (p95): 25 ms, because the model must respond quickly enough for the prediction service while allowing some safety margin for slower environments.
- Minimum gain to replace the champion (F1 points): 
0.005, because a new model must demonstrate a meaningful improvement instead of replacing the champion for a negligible difference.

## What I would check next

I would investigate performance across customer contract types, especially the one-year and two-year contract groups, because their F1 scores are relatively low. I would also monitor changes in customer behavior and data distribution over time to detect model drift.