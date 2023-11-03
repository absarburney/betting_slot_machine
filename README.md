# betting_slot_machine
import random

MAX_LINES = 3

# Define different slot machines with varying odds and symbols
SLOT_MACHINES = [
    {"name": "Classic", "symbols": ["Cherry", "Lemon", "Orange", "Plum", "Bar", "Seven", "Diamond"], "payouts": {"Cherry": 5, "Seven": 20, "Diamond": 50}},
    {"name": "Fruit Fiesta", "symbols": ["Apple", "Banana", "Cherry", "Lemon", "Orange", "Plum", "Watermelon", "Bar", "Seven", "Diamond"], "payouts": {"Cherry": 10, "Seven": 40, "Diamond": 100}},
]

def deposit():
    while True:
        amount = input("What would you like to deposit? $")
        if amount.isdigit():
            amount = int(amount)
            if amount > 0:
                break
            else:
                print("Amount must be greater than 0.")
        else:
            print("Please enter a number.")

    return amount

def get_number_of_lines():
    while True:
        lines = input(f"Enter the number of lines to bet on (1-{MAX_LINES}): ")
        if lines.isdigit():
            lines = int(lines)
            if 1 <= lines <= MAX_LINES:
                break
            else:
                print("Enter a valid number of lines.")
        else:
            print("Please enter a number.")

    return lines

def select_slot_machine():
    while True:
        print("Available Slot Machines:")
        for i, machine in enumerate(SLOT_MACHINES, start=1):
            print(f"{i}. {machine['name']}")
        choice = input(f"Select a slot machine (1-{len(SLOT_MACHINES)}): ")
        if choice.isdigit():
            choice = int(choice)
            if 1 <= choice <= len(SLOT_MACHINES):
                return SLOT_MACHINES[choice - 1]
            else:
                print("Invalid choice. Please select a valid slot machine.")
        else:
            print("Please enter a number.")

def spin_slot_machine(machine):
    return [random.choice(machine["symbols"]) for _ in range(3)]

def display_slot_results(results):
    print("Slot Machine Results:")
    for line in results:
        print(" | ".join(line))
    print()

def check_winning_combination(results, machine):
    payouts = machine["payouts"]
    winnings = 0
    for line in results:
        for symbol, payout in payouts.items():
            if line.count(symbol) == 3:
                winnings += payout
    return winnings

def main():
    balance = deposit()
    
    while balance > 0:
        lines = get_number_of_lines()
        balance -= lines
        
        machine = select_slot_machine()
        
        results = [spin_slot_machine(machine) for _ in range(lines)]
        display_slot_results(results)
        
        winnings = check_winning_combination(results, machine)
        balance += winnings
        print(f"Balance: ${balance}")
        
        play_again = input("Do you want to play again? (yes/no): ")
        if play_again.lower() != "yes":
            break

    print("Thank you for playing!")

if __name__ == "__main__":
    main()
