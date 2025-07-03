# Profile-manager

import json
from datetime import datetime

# File to store user data
USER_FILE = "users.json"

# Load users from JSON file
def load_users():
    try:
        with open(USER_FILE, 'r') as file:
            return json.load(file)
    except FileNotFoundError:
        return {}

# Save users to JSON file
def save_users(users):
    with open(USER_FILE, 'w') as file:
        json.dump(users, file, indent=4)

# Add a new user
def add_user():
    users = load_users()
    user_id = input("Enter new User ID: ")
    if user_id in users:
        print("User ID already exists.")
        return
    name = input("Enter Name: ")
    vehicle_number = input("Enter Vehicle Number: ")
    users[user_id] = {
        "name": name,
        "vehicle_no": vehicle_number,
        "history": []
    }
    save_users(users)
    print("User added successfully.")

# Edit an existing user
def edit_user():
    users = load_users()
    user_id = input("Enter User ID to edit: ")
    if user_id not in users:
        print("User not found.")
        return
    name = input("Enter new Name: ")
    vehicle_number = input("Enter new Vehicle Number: ")
    users[user_id]["name"] = name
    users[user_id]["vehicle_no"] = vehicle_number
    save_users(users)
    print("User updated successfully.")

# Delete a user
def delete_user():
    users = load_users()
    user_id = input("Enter User ID to delete: ")
    if user_id in users:
        del users[user_id]
        save_users(users)
        print("User deleted successfully.")
    else:
        print("User not found.")

# View a user's swap history
def view_history():
    users = load_users()
    user_id = input("Enter User ID to view history: ")
    if user_id in users:
        history = users[user_id]["history"]
        if history:
            print(f"\nSwap History for {user_id}:")
            for event in history:
                print(f"- {event}")
        else:
            print("No swap history available.")
    else:
        print("User not found.")

# Simulate a battery swap and record it
def simulate_swap():
    users = load_users()
    user_id = input("Enter User ID to simulate swap: ")
    if user_id in users:
        timestamp = datetime.now().strftime("%Y-%m-%d %H:%M")
        users[user_id]["history"].append(f"Swapped on {timestamp}")
        save_users(users)
        print("Swap recorded.")
    else:
        print("User not found.")

# Main program loop
def main():
    while True:
        print("\n--- EV User Profile Manager ---")
        print("1. Add User")
        print("2. Edit User")
        print("3. Delete User")
        print("4. View Swap History")
        print("5. Simulate Swap")
        print("6. Exit")

        choice = input("Choose an option (1-6): ")

        if choice == '1':
            add_user()
        elif choice == '2':
            edit_user()
        elif choice == '3':
            delete_user()
        elif choice == '4':
            view_history()
        elif choice == '5':
            simulate_swap()
        elif choice == '6':
            print("Exiting program.")
            break
        else:
            print("Invalid choice. Please enter a number between 1 and 6.")

# Run the program
if __name__ == "__main__":
    main()
