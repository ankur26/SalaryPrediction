# Salary Prediction based on job description features.

<a href = "https://github.com/ankur26/SalaryPrediction/blob/master/Salary%20Prediction%20Notebook.ipynb">Link to code</a>
## Define the problem
Let's say we are the job seeker who's gotten our very first offer and has no clue on negotiation, maybe we are HR who has to decide how much pay this person should get in his/her new job, here comes this project where we define and analyse the features which get you the salary expected across several aspects such as position and the industry you work in etc. Hence we are to train a model which can be used as a tool across several endpoints to help get a salary estimate for the Job. 

### Objective : Analyse job description data and define a model which can predict the salary based on the same by a reasonable estimate.

### Problem areas helped: HR,Finance

### Dataset: Description of features
* **JobId**: Job Identifier Used to track the jobs.(Unique key)
* **CompanyId** : Anonymized company data of the job.
* **JobType** : Position within the company(Categorical)
* **Industry** : Industry of the job.(Categorical)
* **Degree** : Level of degree of candidate in the job.(Categorical)
* **Major** : Major of the degree in.(Categorical)
* **Years of Experience** : Experience of candidate in years.(Numerical continuous)
* **Miles from metropolis** : Distance from the nearest metropolis.(Numerical continuous)
* **Salary** : The variable which we need to predict .(Numerical continuous) Also defined in thousands.

### Error metric : MSE(Mean Squared Error)
We will use <a href="https://en.wikipedia.org/wiki/Mean_squared_error">Mean squared error(MSE)</a> as our metric as it is mathematically easy to define and easy to use for optimization during the modeling phase. It also is very interpretable for example a salary MSE of a 10000 indicates that the estimate of our model will be off by (-100,+100) when we use it. This gives people a lower end and an upper mark to settle for in salary negotiations.

## A few results from our analyses and outcomes
* In our analysis of the dataset we found that CompanyId is not a prevalent feature as the average salary across each company seemed to be across the same range.<img src="https://github.com/ankur26/SalaryPrediction/blob/master/Images/companyId_plot.png">The lower plot as we see remains a straight line across every company hence this feature is not of much use during modeling.
* We also see that once we convert the categorical variables to numerical through <a href="https://www.questionpro.com/blog/ordinal-scale/">ordinal encoding</a> there is a high correlation within two features that is **major** and **degree**.<img src="https://github.com/ankur26/SalaryPrediction/blob/master/Images/corr_map_03-12-2019%2020_29_05.png"> 
We see that at the lower corner(images saved are not clear). To solve for this issue we combined both of the features in one called as degree_in_major and we were able to get a nice positive correlation while not losing a large chunk of information as shown below. <img src="https://github.com/ankur26/SalaryPrediction/blob/master/Images/degree_in_major_plot.png">
The lower plot shows a nice positive correlation with the salary once the degree and major categories are combined.

* The salary variable also had a few outliers which were dropped. This plot shows the outliers present in the salary.<img src="https://github.com/ankur26/SalaryPrediction/blob/master/Images/salaryhistogram.png">The box plot on the right clearly shows that most of the outliers are present on upper end than the lower.

* After dropping companyId we see that every other feature except **miles from Metropolis** is positively correlated with the data.

### Modeling:
For our modeling approach we used the following methods.
* <a href="https://simple.wikipedia.org/wiki/Linear_regression">Linear Regression</a> : Simple line fitted across the data, very easy to interpret. MSE score 390 i.e estimates will be off by(+19.7,+19.7)k.
* <a href="https://en.wikipedia.org/wiki/Polynomial_regression">Polynomial regression</a>: Linear Regression with polynomial features , adds features but captures non linear relations in data, easy to interpret . MSE score 360 i.e estimates will be off by(-18.97,18.97)k
* <a href="https://en.wikipedia.org/wiki/Gradient_boosting">Gradient Boosting</a> : Uses sequences of regression to optimize on the errors for modelling, difficult to interpret but very powerful and as expected has the best MSE of 342 i.e estimates are off by (-18.5,+18.5)

## Summary

To end with we see that the Gradient Boosting model fits the data the best and is able to get a good error metric of 342 k^2 i.e when we estimate from this model we can say that our salary outcome will be in the range of (-18k,+18k) which is a very reasonable estimate as compared to an industry average perspective.

We also created a pipeline that takes both the training and the test scripts and tunes the model and saves the test outcomes to a csv file.

Also a few features that the dataset can include in the future are :
* Gender: With the pay scale gap being such an issue these days this feature can make a huge difference in modeling the target variable.
* Company Jumps: A person changing companies more frequently tends to have a higher salary due to hikes.
* Years within the company also can be a good factor.

### Issues with the data:
* **Outliers** : The lower quartile had only 5 records but we also notice that some junior level people getting C-level salaries with very low years of experience hence we need to make sure to drop that data but the other data can be kept as exceptions.
* None in Major: The data showed 'Doctoral in None' (A mistake on the data entry part) NONE has to be replaced with OTHER.

**Note**: The better the data is the better the model will perform.

**Libraries used** : <a href="https://scikit-learn.org/stable/">sklearn</a>, <a href="https://pandas.pydata.org/">pandas</a>, <a href="https://seaborn.pydata.org/">seaborn</a>, <a href="https://matplotlib.org/">matplotlib</a>, <a href="https://numpy.org/">numpy</a>, datetime,warnings

**Language** : Python v3.7

