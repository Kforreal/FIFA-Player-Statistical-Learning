# Predicting Football Player Market Value Using LASSO and Regression Trees

**STAT 444 – Statistical Learning | University of Waterloo | August 2026**

**Author:** Yiqi Xiang

## Project Overview

This project investigates the relationship between football player attributes and market value using statistical learning methods in R. Using player data from EA Sports FC 24, the analysis aims to identify the characteristics most strongly associated with player valuation and determine whether nonlinear models offer better predictive performance than regularized linear regression.

Two statistical learning approaches are implemented and compared:

- **LASSO Regression:** Applies L1 regularization for coefficient shrinkage and automatic variable selection.
- **Regression Trees:** Models nonlinear relationships and interactions through recursive partitioning.

Both models are tuned using cross-validation and evaluated on an independent test set using Root Mean Squared Error (RMSE).

## Dataset and Preprocessing

**Data Source:** [EA Sports FC 24 Complete Player Dataset – Kaggle](https://www.kaggle.com/datasets/stefanoleone992/ea-sports-fc-24-complete-player-dataset)

The dataset contains player attributes from multiple FIFA/EA Sports FC releases. The analysis focuses on male outfield players from FC 24.

Key preprocessing steps included:

- Filtered the dataset to EA Sports FC 24, initially containing 18,350 players.
- Removed observations with missing or nonpositive market values and excluded goalkeepers because their skill profiles differ from those of outfield players.
- Applied a natural logarithm transformation to market value to address substantial right skewness.
- Selected 32 candidate predictors covering physical characteristics, attacking, technical skills, movement, power, mentality, and defending.
- Excluded overall ratings, potential, wages, and release clauses to avoid relying on variables closely tied to player valuation.
- Split the final dataset into 80% training and 20% testing observations using a fixed random seed.

**Final modeling dataset:** 16,217 players, with 12,973 training observations and 3,244 test observations.

## Methodology

### 1. LASSO Regression

LASSO regression was implemented using the `glmnet` package to predict the natural logarithm of player market value.

- Performed 10-fold cross-validation to select the regularization parameter.
- Applied the one-standard-error rule to balance predictive accuracy and model complexity.
- Selected a regularization parameter of approximately λ = 0.00516.
- Reduced the feature set from 32 predictors to 25.

The model identified several important predictive attributes, including:

- **Age:** Negative association with predicted log market value, conditional on other features.
- **Movement Reactions:** Positive association.
- **Ball Control:** Positive association.
- **Short Passing:** Positive association.
- **Composure:** Positive association.

Seven candidate variables were eliminated through coefficient shrinkage, demonstrating LASSO's ability to simplify models containing overlapping predictive information.

### 2. Regression Trees

Regression trees were implemented using the `rpart` package to investigate nonlinear relationships and interactions among player attributes.

- Constructed an initial regression tree using recursive partitioning.
- Applied cost-complexity pruning and cross-validation.
- Selected the pruning parameter using the one-standard-error criterion.
- Evaluated the selected tree on the held-out test set.

The cross-validation-selected tree contained 274 splits. A smaller tree was also constructed separately to visualize major predictive relationships.

The simplified tree identified **movement reactions** as its first splitting variable, with additional splits involving ball control and standing tackle.

These findings were broadly consistent with the LASSO results, highlighting the predictive importance of technical ability and movement-related attributes.

## Model Performance

Both models were evaluated using RMSE on the held-out test set, with the response measured on the natural-log market-value scale.

| Model | Test RMSE |
|---|---:|
| LASSO Regression | 0.4741 |
| Regression Tree | **0.4188** |

The regression tree achieved approximately **11.7% lower test RMSE** than LASSO.

This improvement suggests that nonlinear relationships and interactions among player characteristics contain additional predictive information beyond the additive linear structure captured by LASSO.

However, the cross-validation-selected regression tree was substantially more complex, illustrating the trade-off between predictive accuracy and interpretability.

## Key Findings

1. **Nonlinear relationships matter.** Regression trees achieved stronger predictive performance than regularized linear regression, suggesting that the effects of player attributes on market value are not entirely additive or linear.

2. **Movement reactions and ball control are important predictors.** Both modeling approaches identified these attributes as useful sources of predictive information.

3. **Regularization supports feature selection.** LASSO reduced the original 32 predictors to 25 while maintaining a relatively interpretable model.

4. **Predictive accuracy and interpretability involve trade-offs.** The larger regression tree delivered better test performance, whereas LASSO offered a simpler global interpretation of predictor relationships.

## Technologies and Skills

**Programming:** R, R Markdown

**Libraries:** `tidyverse`, `glmnet`, `rpart`, `rpart.plot`, `ggplot2`

**Statistical and Machine Learning Methods:**

- Exploratory Data Analysis (EDA)
- Data Preprocessing and Feature Selection
- Log Transformation
- LASSO Regression and L1 Regularization
- Decision Tree Regression
- 10-Fold Cross-Validation
- Hyperparameter Tuning
- Cost-Complexity Pruning
- Predictive Model Evaluation using RMSE

## Repository Contents

- `STAT 444 Final Project.Rmd` – R Markdown source containing data preprocessing, model implementation, evaluation, and analysis.
- `STAT-444-Final-Project.pdf` – Final project report with methodology, visualizations, results, and discussion.
- `README.md` – Project documentation.

## Reproducibility

The analysis was conducted in R using the `tidyverse`, `glmnet`, `rpart`, and `rpart.plot` packages.

To reproduce the analysis:

1. Download the original player dataset from the linked Kaggle source.
2. Place `male_players.csv` in the R working directory.
3. Install the required R packages.
4. Open `STAT 444 Final Project.Rmd` in RStudio and run the analysis.

A fixed random seed (`444`) was used for the training/test split and model-tuning procedures.

## Limitations and Future Work

The player market values are derived from the EA Sports FC database and should not be interpreted as exact real-world transfer valuations.

Additional information, including player positions, clubs, leagues, nationalities, and contract details, could potentially improve predictive performance.

Future extensions could investigate alternative regularization methods, ensemble tree models, and additional predictors to improve generalization and explore the stability of feature importance.

## Reference

Leone, S. *EA Sports FC 24 Complete Player Dataset*. Kaggle. Accessed August 2026.

[Dataset Link](https://www.kaggle.com/datasets/stefanoleone992/ea-sports-fc-24-complete-player-dataset)
