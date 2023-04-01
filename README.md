![Home Loan Approval]('va-home-loan-approval-1800.png')

# Loan Qualifier

**This modular style program, named `loan_qualifier_app` takes in six scripts with the purpose of taking in data froma .csv file with the following variables: Lender, Max Loan Amount, Max Loan-To-Value (LTV), Max Debt-to-Income (DTI), Min Credit Score, and Interest Rate. From this data, the code filters out what loans the applicant will qualify based on the applicant's credit score, debt, income, loan amount, and home value.**

---

## Technologies

This code utilizes python version 3.7, which will allow the person using this code to run python's Fire and Questionary libraries.

*Fire & Questionary are used to help provide user's input the applicant variables in a command line interface, rather than searching through the code to hard code the data into the python script. This is a huge time saver!*


[To learn more about Fire](https://google.github.io/python-fire/guide/)

[To learn more about Questionary](https://pypi.org/project/questionary/)

---

## Installation Guide

The `app.py` script begins with the following inputs to install Fire and Questionary.

```python
pip install fire
pip install questionary
```

Next, the `load_csv` function which loads and reads a provided bank data .csv file with the inputs Lender, Max Loan Amount, Max LTV, Max DTI, Min Credit Score, and Interest Rate. An example .csv file, "daily_rate_sheet.csv" has been included in the repository. Following, the `save_cvs` function will write and save the output data (i.e., a list of the banks that are willing to underwrite the loan).

We import `load_cvs` and `save_cvs` by pulling it from the `fileio.py` by: 

> Side Note: Both the `load_csv` and `save_cvs` files can be found in the `fileio.py`.

```python
from qualifier.utilis.fileio import load_cvs, save_cvs
```

Following importing the input/output (load and save) .cvs files, the `app.py` script imports the filter functions: max loan size, credit score, debt to income, and loan to value. These functions are in their own modular style scripts under qualifier and then filter branches, respectively. More detail on these scripts can be found below under te Usage section in the qualifier folder.

> Side Note: All the filter scripts, that is, `filter_max_loan_size`, `filter_credit_score`, `filter_debt_to_income`, `ilter_loan_to_value` can be found in the `qualifier` -> `filters` folder.

```python
from qualifier.filters.max_loan_size import filter_max_loan_size
from qualifier.filters.credit_score import filter_credit_score
from qualifier.filters.debt_to_income import filter_debt_to_income
from qualifier.filters.loan_to_value import filter_loan_to_value
```

---

## Usage

Utilizing the CLI that Questionary and Fire provides, the function `load_bank_data()` will prompt the user to put in a .csv file in the terminal which will allow `load_bank_data()` to load the file into the script. 

The `find_qualifying_loans()` function will input the list of bank data from the .csv file and return the list of banks that the banks are willing to underwrite the loan.

Inside the `find_ qualifying_loans()` function, the script calls the `calculate_monthly_debt_ratio()` function to print the monthly debt to income ratio given the inputs of debt and income of the applicant. 

Next `calculate_loan_to_value_ratio()` function will calculate the loan to value ratio given the inputs of loan and home value of the applicant. 

> Side Note: Both the `calculate_monthly_debt_ratio()` and `calculate_loan_to_value_ratio()` can be found in the `calculators.py` script which is in the utils folder.

Following the ratios above, the `app.py` script then runs the qualification filters: `filter_max_loan_size`, `filter_credit_score`, `filter_debt_to_income`, and `filter_loan_to_value`.

Returning the `find_qualifying_loans()` function will return the filtered bank data. 

All together this function looks like: 

```python
def find_qualifying_loans(bank_data, credit_score, debt, income, loan, home_value):
   

    # Calculate the monthly debt ratio
    monthly_debt_ratio = calculate_monthly_debt_ratio(debt, income)
    print(f"The monthly debt to income ratio is {monthly_debt_ratio:.02f}")

    # Calculate loan to value ratio
    loan_to_value_ratio = calculate_loan_to_value_ratio(loan, home_value)
    print(f"The loan to value ratio is {loan_to_value_ratio:.02f}.")

    # Run qualification filters
    bank_data_filtered = filter_max_loan_size(loan, bank_data)
    bank_data_filtered = filter_credit_score(credit_score, bank_data_filtered)
    bank_data_filtered = filter_debt_to_income(monthly_debt_ratio, bank_data_filtered)
    bank_data_filtered = filter_loan_to_value(loan_to_value_ratio, bank_data_filtered)

    print(f"Found {len(bank_data_filtered)} qualifying loans")

    return bank_data_filtered
```

Now that the `app.py` script has returned the filtered bank data, it will then save it to a new .csv file given the defined function `save_qualifying_loans`. 

```python
def save_qualifying_loans(qualifying_loans):

    csvpath = Path('qualifying_loans.csv')
    save_csv(csvpath, qualifying_loans)
```

The `app.py` script runs the script with the `run()` function. 

```python
def run():
    """The main function for running the script."""

    # Load the latest Bank data
    bank_data = load_bank_data()

    # Get the applicant's information
    credit_score, debt, income, loan_amount, home_value = get_applicant_info()

    # Find qualifying loans
    qualifying_loans = find_qualifying_loans(
        bank_data, credit_score, debt, income, loan_amount, home_value
    )

    # Save qualifying loans
    save_qualifying_loans(qualifying_loans)
```

Lastly, the `app.py` script runs Fire so that the user can utilize the convenience of CLI

```python 
if __name__ == "__main__":
    fire.Fire(run)
```

---

## Structure 

```
├── Starter_Code
│   ├── loan_qualifier_app
│       ├── README.md
│       ├── app.py
│       ├── data
│            ├── daily_rate_sheet.csv
│       ├── qualifier
│            ├── filters
│                ├── _pychace_
│                ├── credit_score.py
│                ├── debt_to_income.py
│                ├── loan_to_value.py
│                ├── max_loan_size.py
│            ├── utils
│                ├── _pychace_
│                ├── calculators.py
│                ├── fileio.py    
└──     ├── qualifying_loans.csv
```
---

## Contributors

Alex Novis

Columbia Fintech Bootcamp

---

## License

MIT License
