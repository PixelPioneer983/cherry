# cherry
cherryin
import json
import os

class Transaction:
    def __init__(self, date, description, amount):
        self.date = date
        self.description = description
        self.amount = amount

    def __str__(self):
        return f"Date: {self.date}, Description: {self.description}, Amount: {self.amount}"

class FinanceManager:
    def __init__(self, filename="transactions.json"):
        self.filename = filename
        self.transactions = []
        self.load_transactions()

    def load_transactions(self):
        if os.path.exists(self.filename):
            with open(self.filename, "r") as file:
                transactions_data = json.load(file)
                self.transactions = [Transaction(**data) for data in transactions_data]
        else:
            self.transactions = []

    def save_transactions(self):
        with open(self.filename, "w") as file:
            json.dump([transaction.__dict__ for transaction in self.transactions], file, indent=4)

    def add_transaction(self, date, description, amount):
        new_transaction = Transaction(date, description, amount)
        self.transactions.append(new_transaction)
        self.save_transactions()

    def remove_transaction(self, description):
        self.transactions = [transaction for transaction in self.transactions if transaction.description != description]
        self.save_transactions()

    def display_transactions(self):
        if not self.transactions:
            print("No transactions found.")
        else:
            for transaction in self.transactions:
                print(transaction)
                print("-" * 30)

def main():
    manager = FinanceManager()

    while True:
        print("\nFinance Manager Menu")
        print("1. Add transaction")
        print("2. Remove transaction")
        print("3. View transactions")
        print("4. Exit")

        choice = input("Enter your choice: ")

        if choice == "1":
            date = input("Enter transaction date (YYYY-MM-DD): ")
            description = input("Enter transaction description: ")
            amount = float(input("Enter transaction amount: "))
            manager.add_transaction(date, description, amount)
        elif choice == "2":
            description = input("Enter the description of the transaction to remove: ")
            manager.remove_transaction(description)
        elif choice == "3":
            manager.display_transactions()
        elif choice == "4":
            break
        else:
            print("Invalid choice. Please try again.")

if __name__ == "__main__":
    main()
