# Home Credit Default Risk Prediction

**Optimizing Loan Approval through Machine Learning Classification**

## 🎯 Project Mission

Home Credit aims to leverage statistical methods and Machine Learning to accurately predict credit risk scores. The goal is to support smarter, fairer, and more proactive lending decisions by:

- ✅ Avoiding the rejection of customers who are actually capable of repaying their loans
- ✅ Ensuring loans are disbursed to the right, creditworthy applicants
- ✅ Detecting default risk at an early stage, before it materializes
- Link slides : [Slides](https://docs.google.com/presentation/d/1z5bRefZK00VCbiaykaLjvWS6dOM0hgff4SDRIcs5Gck/edit?usp=sharing)
## 📊 Dataset

This project uses the **Home Credit Default Risk** dataset, which consists of multiple relational tables describing each applicant's current loan application as well as their historical credit behavior.

| File | Description |
|------|-------------|
| `application_{train\|test}.csv` | Main static table for all loan applications; each row represents one loan. |
| `bureau.csv` | The client's previous credits from other financial institutions, as reported to the Credit Bureau. |
| `bureau_balance.csv` | Monthly balances of previous credits reported to the Credit Bureau (one row per month of history). |
| `previous_application.csv` | All previous Home Credit loan applications made by clients in the sample. |
| `POS_CASH_balance.csv` | Monthly balance snapshots of previous POS and cash loans the applicant had with Home Credit. |
| `installments_payments.csv` | Repayment history for previous Home Credit loans (one row per payment/missed payment). |
| `credit_card_balance.csv` | Monthly balance snapshots of previous credit cards the client had with Home Credit. |

## 🧪 Model Evaluation

The best model is (**Logistic Regression**) to predict the `TARGET` variable (likelihood of payment difficulty).

### Top 10 Most Important Features

1. `EXT_SOURCE_2`
2. `EXT_SOURCE_3`
3. `DAYS_BIRTH`
4. `BUREAU_DAYS_CREDIT_mean`
5. `BUREAU_DAYS_CREDIT_min`
6. `DAYS_LAST_PHONE_CHANGE`
7. `DAYS_ID_PUBLISH`
8. `DAYS_REGISTRATION`
9. `REGION_POPULATION_RELATIVE`
10. `DAYS_EMPLOYED`

External credit bureau scores (`EXT_SOURCE_2`, `EXT_SOURCE_3`) and applicant demographic/behavioral signals (age, employment tenure, ID/registration recency) are the strongest predictors of default risk.

### Performance Metrics

**Confusion Matrix**

|              | Predicted 0 | Predicted 1 |
|--------------|------------:|------------:|
| **Actual 0** | 38,867 | 17,671 |
| **Actual 1** | 1,545  | 3,420  |

**Classification Report**

| Class | Precision | Recall | F1-score | Support |
|-------|----------:|-------:|---------:|--------:|
| 0 (No default) | 0.96 | 0.69 | 0.80 | 56,538 |
| 1 (Default)     | 0.16 | 0.69 | 0.26 | 4,965  |

- **Accuracy:** 0.69
- **AUC (ROC):** 0.69

The ROC curve confirms moderate discriminative power (AUC = 0.69), notably better than random guessing. The model is tuned to maximize recall on the minority (default) class, which is intentional given the business cost of missing a true default is typically higher than over-flagging a safe applicant — at the expense of precision on Class 1.

## 🔍 Risk Analysis Insights
<img width="693" height="589" alt="plot" src="https://github.com/user-attachments/assets/b5d49ddb-5680-4101-960a-d563c18c3fbb" />

### 1. Early Detection of Late-Payment Risk
Low external credit scores correlate strongly with payment problems early in the loan tenor. This makes external scores an effective **early warning system** for new loan applications.

### 2. Limitations of Credit Bureau History
A long credit history does not guarantee payment discipline on a new loan. Past credit experience alone is not a strong enough predictor of early-tenor repayment behavior.

## 💡 Business Recommendations

### 1. Design a Score-Based Early Intervention Program
Rather than waiting for a missed payment to occur, the collections/CRM team can use the model's risk score to proactively engage customers flagged with low external scores, for example through:

- H-3 payment reminders
- Ongoing installment education for borrowers
- Lightweight rescheduling options

The goal is to act *before* a late payment actually happens.

### 2. Independently Validate Every New Application
"Being a long-standing credit customer" should not be used as a justification for relaxing approval requirements. Long credit history has not been shown to correlate with smooth early repayment behavior. Automatic leniency policies for clients with long credit histories should be reviewed — every new application should still be evaluated using the applicant's most current external score.

