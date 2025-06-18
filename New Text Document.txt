"""
PROJECT : IT Help Desk Ticket System
Author: AJAY J
Description: A simple system to create, view, and update help desk tickets.
"""

import json
import uuid
from datetime import datetime

# File where all tickets will be stored
TICKET_FILE = 'tickets.json'

# Load existing tickets from the JSON file
def load_tickets():
    try:
        with open(TICKET_FILE, 'r') as file:
            return json.load(file)
    except FileNotFoundError:
        return []  # Return an empty list if file doesn't exist

# Save tickets back to the JSON file
def save_tickets(tickets):
    with open(TICKET_FILE, 'w') as file:
        json.dump(tickets, file, indent=4)

# Create a new support ticket
def create_ticket():
    print("\n--- Create New Ticket ---")
    name = input("Enter your name: ")
    issue = input("Describe the issue: ")
    
    ticket = {
        'id': str(uuid.uuid4()),  # Generate unique ID
        'name': name,
        'issue': issue,
        'status': 'open',
        'created_at': datetime.now().strftime("%Y-%m-%d %H:%M:%S")
    }

    tickets = load_tickets()
    tickets.append(ticket)
    save_tickets(tickets)
    print("Ticket created successfully.")

# Display all tickets
def view_tickets():
    print("\n--- View Tickets ---")
    tickets = load_tickets()
    if not tickets:
        print("No tickets found.")
        return

    for ticket in tickets:
        print("\n-------------------------")
        print(f"Ticket ID   : {ticket['id']}")
        print(f"Name        : {ticket['name']}")
        print(f"Issue       : {ticket['issue']}")
        print(f"Status      : {ticket['status']}")
        print(f"Created At  : {ticket['created_at']}")
        print("-------------------------")

# Update an existing ticket
def update_ticket():
    print("\n--- Update Ticket ---")
    ticket_id = input("Enter Ticket ID to update: ")
    tickets = load_tickets()

    for ticket in tickets:
        if ticket['id'] == ticket_id:
            print("What would you like to update?")
            print("1. Close ticket")
            print("2. Edit issue description")
            choice = input("Enter your choice (1 or 2): ")

            if choice == '1':
                ticket['status'] = 'closed'
                print("Ticket closed.")
            elif choice == '2':
                ticket['issue'] = input("Enter the new issue description: ")
                print("Issue updated.")
            else:
                print("Invalid option.")
                return

            save_tickets(tickets)
            return

    print("❌ Ticket not found.")

# Main menu to interact with the system
def menu():
    while True:
        print("\n=== IT Help Desk Menu ===")
        print("1. Create Ticket")
        print("2. View Tickets")
        print("3. Update Ticket")
        print("4. Exit")

        choice = input("Enter your choice: ")

        if choice == '1':
            create_ticket()
        elif choice == '2':
            view_tickets()
        elif choice == '3':
            update_ticket()
        elif choice == '4':
            print("Exiting the system. Goodbye!")
            break
        else:
            print("Invalid choice. Please try again.")

# Entry point of the program
if __name__ == "__main__":
    menu()
