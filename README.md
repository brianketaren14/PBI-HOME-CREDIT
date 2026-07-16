# Home Credit Default Risk Prediction

## 🎯 Project Mission

Home Credit aims to leverage statistical methods and Machine Learning to accurately predict credit risk scores. The goal is to support smarter, fairer, and more proactive lending decisions by:

- ✅ Avoiding the rejection of customers who are actually capable of repaying their loans
- ✅ Ensuring loans are disbursed to the right, creditworthy applicants
- ✅ Detecting default risk at an early stage, before it materializes

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

The best model is (**XGBoost + SMOTETomek**) to predict the `TARGET` variable (likelihood of payment difficulty).

External credit bureau scores (`EXT_SOURCE_2`, `EXT_SOURCE_3`) and applicant demographic/behavioral signals (age, employment tenure, ID/registration recency) are the strongest predictors of default risk.

### Performance Metrics

**Confusion Matrix**

|              | Predicted 0 | Predicted 1 |
|--------------|------------:|------------:|
| **Actual 0** | 40,556 | 14,038 |
| **Actual 1** | 1,830  | 5,079  |

**Classification Report**

| Class | Precision | Recall | F1-score |
|-------|----------:|-------:|---------:|
| 0 (No default) | 0.96 | 0.74 | 0.84 | 
| 1 (Default)     | 0.27 | 0.74 | 0.39 |

- **Accuracy:** 74%
- **AUC (ROC):** 74%

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

