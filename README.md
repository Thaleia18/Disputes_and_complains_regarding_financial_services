# Predicting disputes regarding financial services.

In this project, I intended to predict if a customer will start a dispute after submitting a complaint regarding financial services. I extracted data extracted from Consumer Financial Protection Bureau.

The Consumer Financial Protection Bureau is an agency of the United States government in charge of consumer protection in the financial sector. CFPB's jurisdiction consists of banks, credit unions, securities firms, payday lenders, mortgage-servicing operations, foreclosure relief services, debt collectors, and other financial companies operating in the United States.  The CFPB’s agency was initially proposed in 2007 by the US Senator Elizabeth Warren, in response to the Late-2000s recession and financial crisis, and approved in the Congress in July 2010.

 https://www.consumerfinance.gov/data-research/consumer-complaints/
 
The database generally is updated daily and contains information for each complaint, including the cause of the complaint, the date of submission, and the company the complaint was sent to for response. The database also includes information about the actions taken by the company in response to the complaint, such as, whether the company's response was timely and how the company responded. If the consumer opts to share it and after taking steps to remove personal information, they publish the consumer’s description of what happened

After a complaint is submitted by a consumer, and the provider take action or response to the complaint, the consumer has the option to start a dispute. I want to predict under which conditions the consumer will decide to start a dispute.

## Consumer data notebook

### Exploratory analysis.
My first step was an exploratory analysis of the database. I downloaded the csv version of the database, simplified columns name strings, eliminated duplicates, review shape and column names. My data base contains 1230079 rows, and 18 columns.  The columns are: 

'date_received', 'product', 'sub_product', 'issue', 'sub_issue', 'consumer_complaint_narrative', 'company_public_response', 'company','state', 'zip_code', 'tags', 'consumer_consent_provided', 'submitted_via', 'date_sent_to_company', 'company_response_to_consumer', 'timely_response', 'consumer_disputed', 'complaint_id'

All the values are strings, except the dates which I changed to the date format after download the file.

From the exploratory analysis, I found many findings:
•	A lot of null values from disputes:
o	Products with no data about disputes.
o	Some companies do not have any data about disputes.
o	Cases still in progress.
• I did not find any pattern that indicated that a feature was a key to predict disputes.
•	I decided not to use date and locations as features since it seems like all the dates and locations have the same rate of dispute vs. non-disputes. Also, all these were categorical variables that add more complexity to the models.
•	In most of the cases, where there is information about disputes available, the customer decided not to start a dispute.

### Featuring engineering

I used just the data with non-null values for dispute. Create a model to predict if the customer will have the option to start a dispute is another exciting issue. Now I will focus on:  
having the option to start a dispute, will a customer start a dispute??

I had to process the variables, since all of them are categorical; in some of them they had thousands of unique values so I decided to start with:
'product' (13), 'company_public_response'(10), 'consumer_consent_provided'(4), 'submitted_via'(6), 'company_response_to_consumer'(7), 'timely_response'(2), 'consumer_disputed'(2) .

Moreover, for variables with a large amount of unique values, I grouped these values:
•	Companies since I have 4293 different values, I grouped the companies with less than 5000 complaints into large, medium, small, and petite; the companies with more than 5000 remained as a variable (for example Wells Fargo, Bank of America, et cetera). The new variable, 'company_code' has 31 unique values.
•	For the variable ‘issue’ I found 99 unique values, so I also grouped the values and got the variable: issue_code' with 18 unique values,

Number of features for basic models= 90

### Unbalanced class and setting simple models.

Since my goal is to understand what makes the consumers start a dispute, I decided to use: logistic regression, decision tree, and random forest. Deep learning algorithms should have better performance, but they are black boxes algorithms and it is difficult to make sense of the purpose of the various components in their models. With the algorithms that I choose is easy to understand the impact of each variable in the disputes.

The classes for dispute are unbalanced; most of the data is for non-disputes. This is an issue for the models because I will reach high accuracy if the models are good predicting non-dispute, but from a business perspective is more interesting find when a dispute will start.

There are different approaches to handle unbalanced classes:
•	Resampling -- Upsampling the dispute data -- Downsampling the non-disputes But I do not want to create synthetic data.
•	Change performance metric from accuracy to AUROC.
•	Penalizing algorithms using class weight.
•	Using tree-based algorithms

I used a combination of the last two options.

## Optimizing with grid search notebook

Since I got a poor accuracy for the non-disputes, I removed some of the grouped variables in the company and issue columns and returned the unique values. I got 283 features. Also, I used a grid search for tuning the parameter values for each model.

## Is this overffiting?? notebook

My main problem was that even when I have high total accuracy F1 is smaller. The models are still good predicting the category of non-disputes, but are failing to predict disputes.

I wanted to analyze if there is overfitting. I used the learning curve to plot the accuracy curves for different sizes of training samples. All the models got a gap between the training score and validation score, with training score being higher, this means models are overfitting.

The overfit is reduced when 20-25% of the data is used to training, and the other is set to the validation set, and I got an improvement in the prediction of the disputes.

## Insights notebook

I used eli5 to perform Permutation Importance.

Permutation importance returns the weight of each feature (weight is how the accuracy changes when the values in the feature shuffle). When negative weights are got, that means that the accuracy improved with the shuffle values meaning that the feature is not essential. 
I wanted to check with features got positive weight for each model. So I can eliminate the other features and explore features that I didn't include in the previous models.

## Next steps (Currently working on that...)

The models still can be improved. I am currently working on the next approaches:
•	I am keeping just the features with positive weight, and creating new models using features that I did not use such sub_products, sub_issues.
•	I want to experiment with the metrics; changing the metrics on the models to AUROC can improve the prediction of the unbalanced classes.
•	I want to experiment with the customer narrative. Using a sentiment analysis in the customer's descriptions of the complaint will increase the accuracy of prediction of disputes. I am working with Keras to get this. 

