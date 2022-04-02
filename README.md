# Loan Qualifier Application

This is a command line application that assist users with matching loan applicants with qualifying bank loans. This application was created to streamline the loan qualifying process for user and to save the qualifying loans so that that the results can be shared in a spreadsheet to other users.

---

## Technologies

The programming language used for this application was **Python v_3.9.7**.
The applicable libraries needed to run this application are:

![<Libraries>](./Images/Libraries%20snippit.png)

---

## Installation Guide

All of the above librabries should be part of the base librabries installed with the version of Python above; if not, you will have to install them through the pip package manager which is the package manager of Python.

>Required pip install librabries (versions):

![<PiP>](./Images/Required%20pip%20install%20Libraries.png)


---

## Contributors

James Handral
james.handral@gmail.com


---

## License


N/A

---
## Usage


A- First the user needs to import the list of Bank loans (located in the "data" directory) being offered to the loan applicants into python:

```python
def load_bank_data():
    """Ask for the file path to the latest banking data and load the CSV file.

    Returns:
        The bank data from the data rate sheet CSV file.
    """

    csvpath = questionary.text("Enter a file path to a rate-sheet (.csv):").ask()
    csvpath = Path(csvpath)
    if not csvpath.exists():
        sys.exit(f"Oops! Can't find this path: {csvpath}")

    return load_csv(csvpath)
 ```   



B- Users will be prompted to input the applicant's financial information:

```python

def get_applicant_info():
    """Prompt dialog to get the applicant's financial information.

    Returns:
        Returns the applicant's financial information.
    """

    credit_score = questionary.text("What's your credit score?").ask()
    debt = questionary.text("What's your current amount of monthly debt?").ask()
    income = questionary.text("What's your total monthly income?").ask()
    loan_amount = questionary.text("What's your desired loan amount?").ask()
    home_value = questionary.text("What's your home value?").ask()

    credit_score = int(credit_score)
    debt = float(debt)
    income = float(income)
    loan_amount = float(loan_amount)
    home_value = float(home_value)

    return credit_score, debt, income, loan_amount, home_value
```

C- Afterwards a filtering Algorithm will run to get a list of Bank loans that the loan applicants are qualified for based on the banks requirements compared against their financial informtion:

```python

def find_qualifying_loans(bank_data, credit_score, debt, income, loan, home_value):
    """Determine which loans the user qualifies for.

    Loan qualification criteria is based on:
        - Credit Score
        - Loan Size
        - Debit to Income ratio (calculated)
        - Loan to Value ratio (calculated)

    Args:
        bank_data (list): A list of bank data.
        credit_score (int): The applicant's current credit score.
        debt (float): The applicant's total monthly debt payments.
        income (float): The applicant's total monthly income.
        loan (float): The total loan amount applied for.
        home_value (float): The estimated home value.

    Returns:
        A list of the banks willing to underwrite the loan.

    """

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
    print(bank_data_filtered)
    return bank_data_filtered
 ```  

 D- Finally the application gives the user a choice to save the list of qualifying loans in the "data" directory as the qualifying_loans.csv file:


```python
 def save_qualifying_loans(qualifying_loans):
    """Saves the qualifying loans to a CSV file.

    Args:
        qualifying_loans (list of lists): The qualifying bank loans.
    """
    # @TODO: Complete the usability dialog for savings the CSV Files.
    if not qualifying_loans:
        sys.exit("Sorry, there are no qualifying loans.")

    save_qualifying_loans = questionary.confirm("Do you want to save you qualifying loan list?").ask()
    if save_qualifying_loans:
        csvpath = questionary.text("Save file by typing this Path: data/qualifying_loans.csv").ask()
        header = ["Lender", "max loan", "max LTV", "Max DTI", "Min Credit", "Interest Rate"]
        save_csv(Path(csvpath), qualifying_loans, header=header)
```



**Please note, the folders below and their usage:**

 > "data" Folder:
   
Contains the Bank loan list - "daily_rate_sheet.csv" and this is where the new qualifying loans csv file will be saved.

>"qualifer" Folder:

filters - sub folder where the filtering functions (filters for bank loans) are saved for usage in the main app.py file.

utils - sub folders where the utility functions and financial functions (debt ratio, loan to value ratio etc) are saved for usage in the main app.py file.






