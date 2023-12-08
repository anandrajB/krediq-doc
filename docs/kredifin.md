# **KREDIFIN**

### Description

Kredifin solution designed to streamline financial management, providing a centralized solution for businesses to manage their finances, repayable invoices, and maintain a comprehensive ledger for every accounting entry.

### Technologies Used

The following technologies and tools were used in the development of krediq :

- Python (version 3.12.1)
- Fastapi

### Running Steps

To install and set up krediq, follow these steps:

```
- pip install -r requirements.txt

- uvicorn app.main:app --reload (for local)

- gunicorn -w 4 -k uvicorn.workers.UvicornWorker app.main:app (for production)
```

### Folder Structure

- **`/api`**: **This directory contains all the api's.**

  - The api folder api for financing ,repayment , exchange rate , currency , configuration , loan accounts , accounting entries .

- **`/core`**: **This directory contains the main application code for all transactional process .** - **_`/mixin`_**: This folder maintains all the mixins for the main class in resource folder.

      - ***`/resource`***: This folder handles the financing , repayment , invoice repayment respectively .

      - ***`/utils`***: This folder maintains descriptors for abstract class and common attributes .

      - ***`/base.py`***: Contains the currency , amount , account details class for dependency injection  .

      - ***`/main.py`***: Contains the core calculation and processing such as interest rate , interest amount , grace periods , overdue and so  .

- **`/utils`**: **This directory contains the utility functions and action that required .**
      - This folder maintains the response model , authentication , validations and schemas  .

???+ note

    1. Feel free to adjust the structure based on your  needs and best practices.
    2. Don't end up order of execution or Method resolution order error .

### Documentation

- [API Documentation](https://sea-turtle-app-r84t7.ondigitalocean.app/docs)

### Authorization

- To access all the api in the application we use a 64bit token to access the data's
- These data's are changed based on the company
- e.g., 8yu890uy12u10u201u201u2u180801wj2iwjVENZO
- The configuration can be found in **localhost/Configuration/**
- These token need to send in the headers for all other api's .

### Calculation Methods and Formulas

- **Floating interest rate** 
    - interest_rate = margin + floating_interest_for_the_period

- **Fixed interest rate** 
    - interest_rate = margin or rebate_percent

- **Interest Amount** 
    - interest_amount = (margin * (interest_rate / 100)) * (
  (end_date - start_date) / 365
  )

- **overdue interest**

    ##### case 1 :

    - interest_amount_1 = (
        self.amount.finance_amount * (general_interest_rate / 100)
        ) * (calculated_date / 365)
    - interst_amount_2 = 0

    ##### case 2 :

    - overdue_interest_day = calculate_days(today, due_date)
    - interest_amount_1 = (
        finance_amount * (general_interest_rate / 100)
        ) * (self.grace_days / 365)
    - interst_amount_2 = (
        finance_amount * (overdue_interest_rate / 100)
        ) * ( overdue_interest_day / 365)



### Version Management
- The current repo follows Semantic Versioning model (**SemVer**) in git version controlling for production , test and development .
- Major version bumps has been avoided in the spec . 
- for example ,
      - BETA-v1.1
      - TEST-v1.1
      - PRD-v2.1
