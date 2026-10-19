# IT140-Project-Prototype-Move_between_rooms

# Dictionary defining the 8 interconnected rooms, their directional moves, and assets
rooms = {
    'Administrative Office': {
        'North': 'Vault1',
        'South': 'Vault 6',
        'East': 'Vault 4',
        'West': 'Vault 3'
    },
    'Vault1': {
        'South': 'Administrative Office',
        'East': 'Vault 2',
        'item': 'Gold'
    },
    'Vault 2': {
        'West': 'Vault1',
        'item': 'Diamond'
    },
    'Vault 3': {
        'East': 'Administrative Office',
        'item': 'Lithium'
    },
    'Vault 4': {
        'West': 'Administrative Office',
        'North': 'Vault 5',
        'item': 'Nickel'
    },
    'Vault 5': {
        'South': 'Vault 4',
        'item': 'Silver'
    },
    'Vault 6': {
        'North': 'Administrative Office',
        'East': 'Villain, attacker',
        'item': 'Copper'
    },
    'Villain, attacker': {
        'West': 'Vault 6'
    }
}


def show_instructions():
    """Prints game instructions and command options."""
    print("=" * 60)
    print("SECURITY AGENT TEXT ADVENTURE GAME")
    print("=" * 60)
    print("Collect all 6 high-value assets before confronting the villain!")
    print("Move commands: go North, go South, go East, go West (or North, South, East, West)")
    print("Asset collection: get <asset name> (e.g., 'get Gold')")
    print("Type 'exit' to quit the game.")
    print("=" * 60)


def main():
    current_room = 'Administrative Office'
    inventory = []
    total_assets = 6

    show_instructions()

    while True:
        # Display current status
        print(f"\nYou are in the {current_room}.") 
        print(f"Inventory: {inventory}")

        # Check if the room contains an uncollected item
        if 'item' in rooms[current_room]:
            print(f"You see an asset here: {rooms[current_room]['item']}")

        print("-" * 40)

        # Prompt user input
        user_input = input("Enter your move: ").strip()

        if not user_input:
            continue

        # Exit condition
        if user_input.lower() == 'exit':
            print("Exiting game. Goodbye!")
            break

        # Check for item collection command
        if user_input.lower().startswith("get "):
            asset_input = user_input[4:].strip()

            if 'item' not in rooms[current_room]:
                print("There are no assets to collect in this room")
            elif asset_input.lower() == rooms[current_room]['item'].lower():
                collected_item = rooms[current_room].pop('item')
                inventory.append(collected_item)
                print(f"Successfully! Assets collected: {collected_item}")
            else:
                print("Invalid assets")

            continue

        # Check for directional movement command
        direction = user_input.replace("go ", "").replace("Go ", "").strip().capitalize()

        if direction not in ['North', 'South', 'East', 'West']:
            print("Invalid input, enter North, South, East, West")
            continue

        if direction in rooms[current_room]:
            current_room = rooms[current_room][direction]

            # Check win/loss condition upon entering the Villain room
            if current_room == 'Villain, attacker':
                print(f"\nYou have entered the {current_room}!")
                if len(inventory) == total_assets:
                    print("CONGRATULATIONS! You collected all 6 high-value assets and successfully confronted the attacker!")
                else:
                    print(f"GAME OVER! You confronted the attacker with only {len(inventory)}/{total_assets} assets collected.")
                break
        else:
            print(f"You cannot go {direction} from this room.")


if __name__ == "__main__":
    main()

