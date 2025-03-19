- Employee Salary Data Analysis

This project processes and analyzes an employee salary dataset using Python and Pandas in Jupyter Notebook. It includes functions to:

1.  **Import and preprocess salary data:** Reads a CSV file containing employee salary information and converts relevant columns to numeric data types.
2.  **Retrieve employee details:** Searches for employee details by name or retrieves details of random employees.
3.  **Export employee details:** Saves employee details to a CSV file.

## Requirements

* Python 3.x
* pandas
* numpy

You can install the required libraries using pip:

```bash
pip install pandas numpy

Usage
1. Clone the repository or download the Python script.
2. Place the Total_Salary.csv file in the same directory as the script or update the file path in the pd.read_csv() function.
3. Run the Python script in a Jupyter Notebook or Python environment.

- Example Usage in Jupyter Notebook

# Import necessary libraries
import numpy as np
import pandas as pd
import random

# Import the provided salary data
The provided salary dataset should be saved as Total_Salary.csv in the specified directory. The script automatically reads this file into a Pandas DataFrame:
df_total_salary = pd.read_csv(r"C:\Users\AMLIT\Downloads\Total_Salary.csv", low_memory=False)

# Display dataset information
Displays basic information about the dataset using:
df_total_salary.info()

# Convert relevant columns to numeric data types
Converts relevant columns (BasePay, OvertimePay, OtherPay, Benefits, etc.) to numeric data types.
salary_columns = ['BasePay', 'OvertimePay', 'OtherPay', 'Benefits', 'TotalPay', 'TotalPayBenefits']
for col in salary_columns:
    df_total_salary[col] = pd.to_numeric(df_total_salary[col], errors='coerce')

# Function to get employee details
The function get_employee_details_safe() retrieves employee details based on the name provided. It includes:
-Case-insensitive name matching.
-Error handling for missing names, empty datasets, and incorrect input types.
-Ability to fetch a random employee or multiple random employees if no name is provided.
def get_employee_details_safe(employee_name=None, df_total_salary=None, num_random=1):
    try:
        if df_total_salary is None or df_total_salary.empty:
            return "Employee database is empty or not provided."

        # Standardize the Name column (strip spaces & lowercase)
        df_total_salary['EmployeeName'] = df_total_salary['EmployeeName'].astype(str).str.strip().str.lower()

        # If no name is provided, return a random employee
        if employee_name is None:
            if num_random > len(df_total_salary):
                return f"Only {len(df_total_salary)} employees available. Adjusting random selection."

            return df_total_salary.sample(n=num_random).to_dict(orient='records')

        # Ensure 'employee_name' is string type and perform case-insensitive search
        if not isinstance(employee_name, str):
            return "Invalid input: Employee name must be a string."

        employee = df_total_salary[df_total_salary['EmployeeName'] == employee_name.lower()]

        if employee.empty:
            return "Employee not found."

        return employee.to_dict(orient='records')

    except Exception as e:
        return str(e)

# Get details of a specific employee
print(get_employee_details_safe("DAVID KUSHNER", df_total_salary))

# Get details of a random employee
print(get_employee_details_safe(df_total_salary=df_total_salary))

# Get multiple (10) random employees
print(get_employee_details_safe(df_total_salary=df_total_salary, num_random=10))

#Exporting Employee Details
def export_employee_details(employee_name, df_total_salary):
    """
    Exports employee details to a CSV file named "Employee Profile.csv".

    Args:
        employee_name (str): The name of the employee to export.
        df_total_salary (pandas.DataFrame): The DataFrame containing employee data.
    """

    employee_details = get_employee_details_safe(employee_name=employee_name, df_total_salary=df_total_salary)

    if isinstance(employee_details, str):
        print(employee_details)  # Print error message
        return

    if not employee_details:
        print("No employee details found.")
        return

    df_employee = pd.DataFrame(employee_details)

    try:
        df_employee.to_csv("Employee Profile.csv", index=False)
        print(f"Employee details for '{employee_name}' exported to 'Employee Profile.csv'")
    except Exception as e:
        print(f"Error exporting employee details: {e}")

#Exporting a specific Employee details
export_employee_details('EDWARD HARRINGTON', df_total_salary)

- Functions
1. get_employee_details_safe(employee_name=None, df_total_salary=None, num_random=1):
*Retrieves employee details by name or returns details of random employees.
*Handles cases where the employee is not found or the database is empty.
*employee_name: The name of the employee (optional). If not provided, a random employee is returned.
*df_total_salary: The pandas DataFrame containing the employee data.
*num_random: the number of random employees to return.
*returns a dictionary containing the employee data or an error string.
2. export_employee_details(employee_name, df_total_salary):
*Exports the details of a specified employee to a CSV file named "Employee Profile.csv".
*employee_name: the name of the employee to export.
*df_total_salary: the dataframe of all employee data.
*prints a success message or an error message to the console.

- Notes
*Ensure that the Total_Salary.csv file is correctly formatted for accurate search results.
*The get_employee_details_safe function performs a case-insensitive search for employee names.
*The export function creates a file named "Employee Profile.csv" in the same directory as the script. If a file with that name already exists, it will be overwritten.
*Error handling is included to manage common issues such as empty dataframes, invalid inputs, and file export problems.

Author:
This project was created as part of an assignment to demonstrate data processing, dictionary usage, and error handling in Python.

Stellamaris Okeh
stellamarisijeoma0@gmail.com
