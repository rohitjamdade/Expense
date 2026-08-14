import os
import json

FILE_NAME = "expenses.json"


# Load expenses from file
def load_expenses():
    try:
        if os.path.exists(FILE_NAME):
            with open(FILE_NAME, "r") as file:
                return json.load(file)
        return []

    except json.JSONDecodeError:
        print("Error: Invalid JSON file. Starting with empty expenses.")
        return []

    except FileNotFoundError:
        return []

    except Exception as e:
        print("Error while loading expenses:", e)
        return []


# Save expenses to file
def save_expenses(expenses):
    try:
        with open(FILE_NAME, "w") as file:
            json.dump(expenses, file, indent=4)

    except PermissionError:
        print("Error: Permission denied while saving file.")

    except Exception as e:
        print("Error while saving expenses:", e)


# Add expense
def add_expense(expenses):
    try:
        date = input("Enter date (DD-MM-YYYY): ")
        category = input("Enter category: ")

        amount = float(input("Enter amount: "))

        if amount <= 0:
            raise ValueError("Amount must be greater than 0.")

        description = input("Enter description: ")

        expense = {
            "date": date,
            "category": category,
            "amount": amount,
            "description": description
        }

        expenses.append(expense)
        save_expenses(expenses)

        print("\nExpense added successfully.\n")

    except ValueError as e:
        print("Invalid input:", e)
        print()

    except Exception as e:
        print("Error while adding expense:", e)
        print()


# View all expenses
def view_expenses(expenses):
    try:
        if len(expenses) == 0:
            print("\nNo expenses found.\n")
            return

        print("\n----- Expense List -----")

        total = 0

        for i, expense in enumerate(expenses, start=1):
            print(f"\nExpense {i}")
            print("Date:", expense["date"])
            print("Category:", expense["category"])
            print("Amount:", expense["amount"])
            print("Description:", expense["description"])

            total += expense["amount"]

        print("\nTotal Expense:", total)
        print()

    except Exception as e:
        print("Error while displaying expenses:", e)
        print()


# Search expenses by category
def search_expense(expenses):
    try:
        category = input("Enter category to search: ")

        found = False

        print("\n----- Search Result -----")

        for expense in expenses:
            if expense["category"].lower() == category.lower():
                print("Date:", expense["date"])
                print("Amount:", expense["amount"])
                print("Description:", expense["description"])
                print()

                found = True

        if not found:
            print("No expense found.\n")

    except Exception as e:
        print("Error while searching expenses:", e)
        print()


# Delete expense
def delete_expense(expenses):
    try:
        view_expenses(expenses)

        if len(expenses) == 0:
            return

        index = int(input("Enter expense number to delete: "))

        if 1 <= index <= len(expenses):
            expenses.pop(index - 1)
            save_expenses(expenses)

            print("Expense deleted successfully.\n")

        else:
            print("Invalid expense number.\n")

    except ValueError:
        print("Error: Please enter a valid number.\n")

    except Exception as e:
        print("Error while deleting expense:", e)
        print()


# Main menu
def main():
    try:
        expenses = load_expenses()

        while True:
            print("===== Expense Tracker =====")
            print("1. Add Expense")
            print("2. View Expenses")
            print("3. Search Expense")
            print("4. Delete Expense")
            print("5. Exit")

            try:
                choice = input("Enter your choice: ")

                if choice == "1":
                    add_expense(expenses)

                elif choice == "2":
                    view_expenses(expenses)

                elif choice == "3":
                    search_expense(expenses)

                elif choice == "4":
                    delete_expense(expenses)

                elif choice == "5":
                    print("Thank you for using Expense Tracker.")
                    break

                else:
                    print("Invalid choice. Please choose 1-5.\n")

            except KeyboardInterrupt:
                print("\nProgram interrupted by user.")
                break

    except Exception as e:
        print("Unexpected error:", e)


# Run program
if __name__ == "__main__":
    main()
