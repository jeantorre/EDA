## 1. Description
"Diabetes is a chronic disease that occurs either when the pancreas does not produce enough insulin or when the body cannot effectively use the insulin it produces. Insulin is a hormone that regulates blood glucose. Hyperglycaemia, also called raised blood glucose or raised blood sugar, is a common effect of uncontrolled diabetes and over time leads to serious damage to many of the body's systems, especially the nerves and blood vessels.

In 2014, 8.5% of adults aged 18 years and older had diabetes. In 2019, diabetes was the direct cause of 1.5 million deaths and 48% of all deaths due to diabetes occurred before the age of 70 years. Another 460 000 kidney disease deaths were caused by diabetes, and raised blood glucose causes around 20% of cardiovascular deaths.

Between 2000 and 2019, there was a 3% increase in age-standardized mortality rates from diabetes. In lower-middle-income countries, the mortality rate due to diabetes increased 13%.

By contrast, the probability of dying from any one of the four main noncommunicable diseases (cardiovascular diseases, cancer, chronic respiratory diseases or diabetes) between the ages of 30 and 70 decreased by 22% globally between 2000 and 2019."  

After these overview from World Health Organization we notice how important is take care of our healthiness. In special when talks about diabetes, a fatal disease that commit all over the world!

In a world where "time" is the most important metric for all of us, we prefer, in most of the time, fast snacks instead of to stop and taste a real meal. Some of us start to choose light/diet snacks, because we imagine all of these snacks are better than the "normal snack". But, in reality, that's not true.  

Most of these snacks can destroy our well-being in long term! It's very important to take care of our health and choose the right meals, in most part of time.  

Remember, balance is the most important things in life!  

For this second project, I received a dataset originally from the National Institute of Diabetes and Digestive and Kidney Diseases. This dataset is to diagnostically predict whether or not a patient has diabetes, based on certain diagnostic measurements included in the dataset.  

As additional information, all patients are females at least 21 years old of Pima Indian heritage.

### Premises
* Is it possible to create associations between these medical results and diabetes?  
* Is there a way to predict who is prone to diabetes?  

---

## 2. Data preparation
The data source used in this project are from Kaggle that can be found [here](https://www.kaggle.com/datasets/uciml/pima-indians-diabetes-database?datasetId=228&sortBy=voteCount).
### Data arrangement
On site have an explanation that what means every column, but I'll bring the most relevant for this analysis.  
On "Outcome" column show us the diabetes results. If the result is equal 1 means that the patient have diabetes and if is 0 means that she doesn't have.  
Now we can compare if the other medical results are associated with diabetes.

---

## 3. Data analysis
![Histogram](https://drive.google.com/uc?id=1xdooibLEua0RZPNLGnN-pJewxncrUTaV "Histogram")

In these histograms it's possible to do some initial analysis, such as:
* The most of the patients have, at least, 1 child
* The "Glucose" graph looks like a normal distribution with result 100 almost halfway the data. That means that the most part of the patients has a blood glucose test considered normal
* Most of the patients have "BMI" more than 25. That means overweight on this dataset
* This sampling have more young people

That was qualitative analysis. Let's be more specific with numbers and calculate some values.
* The number of non diabetic patient is 500
* The number of diabetic patient is 268, that correspond to a percentage of 34.89 on this data

Splitting into 2 dataframes (diabetics and non diabetics), let's see some numbers:
* The mean glucose result from dataframe with non diabetics is 109.98. That means that the patients are pre diabetics
* The difference in "BMI" mean results in these 2 groups are from 5. So the diabetics are also fatter
* The patients with diabetes are older than the non diabetics. The mean age is almost 6 years older

Let's see these information on chart, to be more easily to realize!  
![KDE 1](https://drive.google.com/uc?id=1Hhenx6aXb75zbjIjGRNXu2ADqYkoeGWf "KDE 1")

![KDE 2](https://drive.google.com/uc?id=1evr3PMKChUw6fVmo-UCnP_IcdOEQuOf- "KDE 2")

![KDE 3](https://drive.google.com/uc?id=1WL9nCoTJNUS62LlO-Vk2mj5NXk6x2Sif "KDE 3")

Using just an exploratory analysis was possible to realize some medical diagnosis relation with diabetics and non diabetics patients.  

Now let's chase some outliers values using box-plot graphs:
![Box-plot 1](https://drive.google.com/uc?id=1Zik4pALXtv5Nbm43g1qE2Mg9S4jzHwsN "Box-plot 1")
There are no much outliers values in these 3 columns (Glucose, BloodPressure and SkinThickness), but notice... there are some 0s on these data. It isn't necessary to be a doctor to realize that is something wrong, right? Or it's possible to someone live with 0 of blood pressure result?  

We will take care of this as soon as possible!  

![Box-plot 2](https://drive.google.com/uc?id=1_cUktYYJX3UtkrmK5mpqfwEYiKZ_9JUt "Box-plot 2")
In this second group of charts in Insulin and BMI result have some outliers and some 0s values, equal the first group of graphs. Let's calculate how many 0s have in each column:
* Quantity of zeros in 'Glucose' column are 5
* Quantity of zeros in 'BloodPressure' column are 35
* Quantity of zeros in 'SkinThickness' column are 227
* Quantity of zeros in 'Insulin' column are 374
* Quantity of zeros in 'BMI' column are 11
* Quantity of zeros in 'DiabetesPedigreeFunction' column are 0

There are so many zeros in some columns that it's important to have some treatment in these values.  
Initially let's use a guideline to north our study! I found a website called "Analytics Yogi" with some ""rules"":
1. Mean imputation is often used when the missing values are numerical and the distribution of the variable is approximately normal.
2. Median imputation is preferred when the distribution is skewed, as the median is less sensitive to outliers than the mean.
3. Mode imputation is suitable for categorical variables or numerical variables with a small number of unique values.

Using this guideline as premises let's change the 0s values.
* On "Glucose", "BloodPressure" and "BMI" columns, will change 0s for mean values of their respective columns
* On "SkinThickness" and "Insulin" columns, will change 0s for median values of their respective columns

---

### ML treatment
**First things first!**  
Let's split the dataframe into X (input) and Y (output), to train and test, the algorithm. As all the data are numerical, so let's normalize these values.  
This step is important for the algorithm don't give preference in higher values!  

We will test 3 types of machine learning, such as:  
* Logistic Regression
* Random forest
* Neural Network

To choose the winner we will select the algorithm that have higher score on "F1-score (macro)". That's a metric calculated from confusion matrix.  
Let's have a little bit of explanation about this.  

![Confusion Matrix](https://miro.medium.com/v2/resize:fit:640/format:webp/1*Z54JgbS4DUwWSknhDCvNTQ.png "Confusion Matrix")  
Image source: https://towardsdatascience.com/understanding-confusion-matrix-a9ad42dcfd62  

* True Positive (TP) - the algorithm predicted positive and it's correct
* True Negative (TN) - the algorithm predicted negative and it's correct
* False Positive (FP - type 1 error) - the algorithm predicted positive and it's false
* False Negative (FN - type 2 error) - the algorithm predicted negative and it's false

To calculate then:  
![Confusion matrix calculation](https://miro.medium.com/v2/resize:fit:640/format:webp/1*uR09zTlPgIj5PvMYJZScVg.png "Confusion matrix calculation")  
Image source: https://miro.medium.com/v2/resize:fit:640/format:webp/1*uR09zTlPgIj5PvMYJZScVg.png  

![F-measure](https://miro.medium.com/v2/resize:fit:640/format:webp/1*98FaAKfPWo-EBTbjsxm4GA.png "F-measure")  
Image source: https://miro.medium.com/v2/resize:fit:640/format:webp/1*98FaAKfPWo-EBTbjsxm4GA.png  

As we can see, the "F-measure" uses "Recall" and "Precision", so give us a more """realistic""" value.

The result can be seen in the table:

Model|Precision|Recall|F1-Score (macro)|Accuracy
-|-|-|-|-
Logistic regression|69%|44%|70%|78%
Random forest|59%|60%|71%|76%
Neural network|64%|47%|69%|77%  

In the above values, the result for "Precision", "Accuracy" and "Recall" are the percentage that the algorithm hits when someone will be diabetics.  
As the F-measure are the more balanced result, the random forest algorithm is the better in these condition.

---

## 4. Conclusions
Using just an EDA it's possible to see some similarity in medical diagnosis result and diabetics patients. But to give one step forward we did a ML model with a simple treatment with a good result of 71%!

---

## 5. References
* Pima Indians Diabetes Database. **Kaggle**, 2016. Available on: <https://www.kaggle.com/datasets/uciml/pima-indians-diabetes-database?datasetId=228&sortBy=voteCount>. Accessed September 21, 2023.
* Diabetes. **World Health Organization**, 2023. Available on: <https://www.who.int/news-room/fact-sheets/detail/diabetes#:~:text=Over%20time%2C%20diabetes%20can%20damage,blood%20vessels%20in%20the%20eyes.>. Accessed September 21, 2023.
* Python – Replace Missing Values with Mean, Median & Mode. **Analytics Yogi**, 2023. Available on: <https://vitalflux.com/pandas-impute-missing-values-mean-median-mode/.>. Accessed September 22, 2023.
* Understanding Confusion Matrix. **Medium**, 2018. Available on: <https://towardsdatascience.com/understanding-confusion-matrix-a9ad42dcfd62>. Accessed September 22, 2023.
