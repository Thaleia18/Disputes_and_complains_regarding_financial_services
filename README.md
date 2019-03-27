# Disputes_and_complains_regarding_financial_services
Trying to predict if a costumer will start a dispute after a complain regarding financial services. Data extracted from: Consumer Financial Protection Financial Bureau.

"Every complaint provides insight into problems that people are experiencing, helping us identify inappropriate practices and allowing us to stop them before they become major issues." https://www.consumerfinance.gov/data-research/consumer-complaints/

After every complain consumers can start a dispute. This is the first approach.

## Consumer data notebook
### Exploratory analysis.


### Featuring ingeneering


### Unbalanced class and setting simple models.


## Optimizing with grid search

I used grid search to optimize the parameter values for each model.

## is this overffiting

MY main problem was that even when I have high accuracy F1 is smaller. The model is good  to predict no disputes, but is failing to predict disputes.

I want to analize if there is overfitting. I used learning_curve to plot the accuracy curves for different sizes of training samples.
All the models got a gap between the training score and validation score, with training score being higher, this means models are overfitting.

The overfit is reduced when 20-25% of the data is used to training and the other is set to the validation set.

## Insights
I used eli5 to perform Permutacion Importance.

Permutacion importance returns the weight of each feature (weight is how the accuracy changes when the values in the feature shuffle).
When negative weights are got, that means that the accuracy improved with the shuffle values meaning that the feature is not important.

I want to check with features got positive weight for each models. So I can eliminate the other features and explore features that I didnt include in the previous models.

### Next steps (Currently working on that...)

- Kept features with positive weight and create new models using features such sub_products, sub_issues and customer_narrative.
- Change the metrics on the models to AUROC.
