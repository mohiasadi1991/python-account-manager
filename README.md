import json
import os

FILE = "accounts.json"

def load_accounts():
    if os.path.exists(FILE):
        try:
            with open(FILE, "r", encoding="utf-8") as :
                return json.load(f)
        except (json.JSONDecodeError, OSError):
            return []
    return []

def save_accounts(accounts):
    with open(FILE, "w", encoding="utf-8") as :
        json.dump(accounts, f, indent=4, ensure_ascii=False)

def add_account(accounts):
    website = input("Website or service: ").strip()
    username = input("Username: ").strip()

    if not website or not username:
        print("Website and username are required.")
        return

    accounts.append({
        "website": website,
        "username": username
    })

    save_accounts(accounts)
    print("Account added successfully!")

def show_accounts(accounts):
    if not accounts:
        print("No accounts found.")
        return

    for i, account in enumerate(accounts, 1):
        print(
            f"{i}. {account['website']} "
            f"| Username: {account['username']}"
        )

def search_account(accounts):
    keyword = input("Search website: ").strip().lower()

    results = [
        account for account in accounts
        if keyword in account["website"].lower()
    ]

    show_accounts(results)

def delete_account(accounts):
    show_accounts(accounts)

    if not accounts:
        return

    try:
        number = int(input("Account number to delete: "))

        if 1 <= number <= len(accounts):
            removed = accounts.pop(number - 1)
            save_accounts(accounts)
            print(f"Deleted: {removed['website']}")
        else:
            print("Invalid account number.")

    except ValueError:
        print("Please enter a valid number.")

def main():
    accounts = load_accounts()

    while True:
        print("\n===== ACCOUNT MANAGER =====")
        print("1. Add account")
        print("2. Show accounts")
        print("3. Search account")
        print("4. Delete account")
        print("5. Exit")

        choice = input("Choose an option: ")

        if choice == "1":
            add_account(accounts)
        elif choice == "2":
            show_accounts(accounts)
        elif choice == "3":
            search_account(accounts)
        elif choice == "4":
            delete_account(accounts)
        elif choice == "5":
            print("Goodbye!")
            break
        else:
            print("Invalid choice.")

if __name__ == "__main__":
    main()
