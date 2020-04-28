# Loan-Prediction

## References :
[1]https://datahack.analyticsvidhya.com/contest/practice-problem-loan-prediction-iii/#ProblemStatement

## Problem statement
   
Dream Housing Finance company deals in all home loans. They have presence across all urban, semi urban and rural areas. Customer first apply for home loan after that company validates the customer eligibility for loan.

Company wants to automate the loan eligibility process (real time) based on customer detail provided while filling online application form. These details are Gender, Marital Status, Education, Number of Dependents, Income, Loan Amount, Credit History and others. To automate this process, they have given a problem to identify the customers segments, those are eligible for loan amount so that they can specifically target these customers. Here they have provided a partial data set.

## Description about the Data Columns:

Variable| Description
:-:|:-:
Loan_ID|   Unique Loan ID
Gender | Male/ Female
Married |Applicant married (Y/N)
Dependents|  Number of dependents
Education  | Applicant Education (Graduate/ Under Graduate)
Self_Employed|   Self employed (Y/N)
ApplicantIncome|   Applicant income
CoapplicantIncome|   Coapplicant income
LoanAmount   |Loan amount in thousands
Loan_Amount_Term|   Term of loan in months
Credit_History   |credit history meets guidelines
Property_Area  |Urban/ Semi Urban/ Rural
Loan_Status   |Loan approved (Y/N)

## Hypothesis Generation :

1. Salary: Applicants with high income should have more chances of loan approval.    
2. Previous history: Applicants who have repayed their previous debts should have higher chances of loan approval.
3. Loan amount: Loan approval should also depend on the loan amount. If the loan amount is less, chances of loan approval should be high.
4. Loan term: Loan for less time period and less amount should have higher chances of approval.
5. EMI: Lesser the amount to be paid monthly to repay the loan, higher the chances of loan approval.

## Explolatory Data Analysis:

### 1. Univariate Analysis

1.1 For Categorical Variables
![alt text](https://github.com/Mann1904/Loan-Prediction/blob/master/rmd-file-files/univariate%20analysis-1.png)
Conclusion :
 
1. Around 80% applicants in the dataset are male.
2. Around 65% of the applicants in the dataset are married.
3. Most of the applicants don’t have any dependents.
4. Around 80% of the applicants are Graduate.
5. Around 15% applicants in the dataset are self employed.
6. Most of the applicants are from Semiurban area.
7. The loan of 422(around 69%) people out of 614 was approved.
    
1.2 For Numerical Variables

![alt text](https://github.com/Mann1904/Loan-Prediction/blob/master/rmd-file-files/univariate%20analysis-2.png)

It can be inferred that most of the data in the distribution of applicant income is towards left which means it is not normally distributed.We will try to make it normal in later sections as algorithms works better if the data is normally distributed.
    
The boxplot confirms the presence of a lot of outliers/extreme values. Part of this can be driven by the fact that we are looking at people with different education levels. Let us segregate them by Education:

![alt text](https://github.com/Mann1904/Loan-Prediction/blob/master/rmd-file-files/univariate%20analysis-3.png)

We can see that there are a higher number of graduates with very high incomes, which are appearing to be the outliers.
Let’s look at the Coapplicant income distribution.

![alt text](https://github.com/Mann1904/Loan-Prediction/blob/master/rmd-file-files/univariate%20analysis-4.png)

We see a similar distribution as that of the applicant income. We also see a lot of outliers in the coapplicant income and it is not normally distributed.  
Let’s look at the distribution of LoanAmount variable.

![alt text](https://github.com/Mann1904/Loan-Prediction/blob/master/rmd-file-files/univariate%20analysis-5.png)

We see a lot of outliers in this variable and the distribution is normal. We will treat the outliers in later sections.

### 2. Bivariate Analysis

After looking at every variable individually in univariate analysis, we will now explore them again with respect to the target variable.


2.1 Categorical Variable vs Target Variable

![alt text](https://github.com/Mann1904/Loan-Prediction/blob/master/rmd-file-files/bivariate%20analysis-1.png)

Conclusion :

1. Distribution of applicants with male or female is similar across both the categories of Loan_Status.    
2. proportion of married applicants is higher for the approved loans.
3. Distribution of applicants with 1 or 3+ dependents is similar across both the categories of Loan_Status.
4. Proportion of loans approved in semiurban area is higher.
5. proportion of loans approved for applicants with credit history as 1 is higher.
    

2.2 Numerical Independent Variable vs Target Variable

![alt text](https://github.com/Mann1904/Loan-Prediction/blob/master/rmd-file-files/bivariate%20analysis-2.png)

We don’t see any change in the mean income. So, let’s make bins for the applicant income variable based on the values in it and analyze the corresponding loan status for each bin.

![alt text](https://github.com/Mann1904/Loan-Prediction/blob/master/rmd-file-files/bivariate%20analysis-3.png)

It can be affered that ApplicantIncome does not affect the chances of loan approval which contradics our hypothesis in which we assumed that if the ApplicantIncome is high the chances of loan approval will also be high.

![alt text](https://github.com/Mann1904/Loan-Prediction/blob/master/rmd-file-files/bivariate%20analysis-4.png)

It shows that if coapplicant’s income is less the chances of loan approval are high. But this does not look right.The possible reason behind this may be that most of applicant don't have any coapplicant so coaaplicant income for such applicant is 0 and hence the loan approval is not dependent on it.So we can make new variable in which we will combine the applicant's and coapplicant's income to visualize the combined effect of income on loan approval.
        
![alt text](https://github.com/Mann1904/Loan-Prediction/blob/master/rmd-file-files/bivariate%20analysis-5.png)

We can see that Proportion of loans getting approved for applicants having low Total_Income is very less as compared to that of applicants with Average, High and Very High Income.

![alt text](https://github.com/Mann1904/Loan-Prediction/blob/master/rmd-file-files/bivariate%20analysis-6.png)

It can be seen that the proportion of approved loans is higher for Low and Average Loan Amount as compared to that of High Loan Amount which supports our hypothesis in which we considered that the chances of loan approval will be high when the loan amount is less.

![alt text](https://github.com/Mann1904/Loan-Prediction/blob/master/rmd-file-files/corr.png)

We see that the most correlated variables are (ApplicantIncome - LoanAmount). LoanAmount is also correlated with CoapplicantIncome.


## Handling Missing Values :

We can consider these methods to fill the missing values:

1.  For numerical variables: imputation using mean or median
2.  For categorical variables: imputation using mode


## Getting new data columns :

1) We have two column named 'ApplicantIncome' and 'CoapplicantIncome'. It may be the case total income might have good impact on Loan Status.
2) It may be the case EMI is good impact on Loan Status as it combine two column 'LoanAmount' and 'Loan_Amount_Term'. 
3) It may be the case Debt amount ratio(EMI*12/total income) is good impact on loan Status.

Hence the formula that can be used is

1) Total Income = Applicant Income + Co-applicant Income

2) The mathematical formula for calculating EMIs is:
EMI = [P x R x (1+R)^N]/[(1+R)^N-1]
where P stands for the loan amount or principal, R is the interest rate per month and N is the number of monthly instalments.
for home loans let’s check the interest rates in google for several banks. I have found that on an average it would be around 8.5% to 9.5%. Hence for safe-side I am assuming that 9% is the interest rate.

3) Debt_Amount_Ratio = 12*EMI / TotalIncome.


Check the distribution of new created variables :

![alt text](https://github.com/Mann1904/Loan-Prediction/blob/master/rmd-file-files/new%20column-1.png)

![alt text](https://github.com/Mann1904/Loan-Prediction/blob/master/rmd-file-files/new%20column-2.png)

![alt text](https://github.com/Mann1904/Loan-Prediction/blob/master/rmd-file-files/new%20column-3.png)


We can see that TotalIncome is not normally distributed and there are lots of outliers and also EMI & Debt_Amount_Ratio are not normally distributed.
   
Log transformation on TotalIncome variable :  
data['TotalIncome'] = np.log(data['TotalIncome']) 

![alt text](https://github.com/Mann1904/Loan-Prediction/blob/master/rmd-file-files/new%20column-4.png)

Now, We can see that distribusion of TotalIncome is normal.



Cube root transformation on EMI variable : 
data['EMI'] = (data['EMI']**(1/3))

![alt text](https://github.com/Mann1904/Loan-Prediction/blob/master/rmd-file-files/new%20column-5.png)


Cube root transformation on Debt_Amount_Ratio variable : 
data['Debt_Amount_Ratio'] = (data['Debt_Amount_Ratio']**(1/3))

![alt text](https://github.com/Mann1904/Loan-Prediction/blob/master/rmd-file-files/new%20column-6.png)





### Models performance comparison

Model| CV Score
:-:|:-:
LogisticRegression    |     Mean = 0.8097 , std = 0.0507
DecisionTreeClassification |  Mean = 0.8097, std = 0.0507
RandomForestClassification | Mean = 0.8063, std = 0.0512
XgBoostClassification | Mean = 0.8111, std = 0.0517
AdaBoostClassification | Mean = 0.8145 | std = 0.0517

