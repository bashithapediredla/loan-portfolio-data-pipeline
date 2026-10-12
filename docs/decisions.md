# Design decisions

## 1. Domain: loans
- **Problem:** Needed a domain where SCD2 is actually useful.
- **Options:** Food delivery, loans, insurance claims.
- **Chose:** Loans. A borrower's credit tier changes after approval, and the credit manager needs the tier from approval day.
- **Gave up:** Had to learn banking terms (EMI, DPD, NPA).

## 2. Users and questions
- **Users:** Credit manager, Collections manager, Risk manager.
- **Questions:**
  1. % of loans that went 90+ DPD, by credit tier at approval, per approval quarter
  2. Loans with EMI due in the next 7 days, and total amount due
  3. Overdue loans and amount by DPD bucket, as of today
  4. % of auto-debits that failed each month, by reason
  5. NPA % per loan type, month by month
- **Gave up:** Kept it to 5 questions to finish in a week.

## 3. Default = 90+ DPD
- **Problem:** "Default" needs one clear rule.
- **Chose:** Loan is NPA when DPD is over 90 (RBI rule). Store DPD; calculate bucket and NPA flag from it.
- **Gave up:** Nothing. If the rule changes, just recalculate.

## 4. Data source: generated data
- **Problem:** Need data for SCD2, daily files and bad rows.
- **Options:** Kaggle dataset, generate my own, mix of both.
- **Chose:** Generate my own. Kaggle data has no credit tier history and doesn't arrive as daily files.
- **Gave up:** Data is not real (README says so).

## 5. Source tables
| Table | One row = | Comes as | Changes later? |
|---|---|---|---|
| borrowers | one borrower | database | yes (phone/address) |
| loans | one loan | database | no |
| credit_tier_updates | one tier change | bureau files | no |
| emi_schedule | one EMI | daily files | yes (paid_date filled) |
| auto_debit_attempts | one debit attempt | daily files | no |