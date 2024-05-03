# Team-Merchandising-Revenue-System
import json

class Team:
    def __init__(self, team_id, team_name):
        self.team_id = team_id
        self.team_name = team_name
        self.merchandise_list = []

    def calculate_team_revenue(self, production_cost, advertising_cost):
        total_cost = production_cost + advertising_cost
        total_revenue = sum(merch.selling_price * merch.sold_quantity for merch in self.merchandise_list)
        profit = total_revenue - total_cost
        return total_revenue, profit

class Merchandise:
    def __init__(self, merch_id, name, price, total_quantity, sold_quantity, selling_price):
        self.id = merch_id
        self.name = name
        self.price = price
        self.total_quantity = total_quantity
        self.sold_quantity = sold_quantity
        self.selling_price = selling_price

def save_data(data):
    with open("data.json", "w") as f:
        json.dump(data, f)

def load_data():
    try:
        with open("data.json", "r") as f:
            data = json.load(f)
            return data
    except FileNotFoundError:
        return None

def main():
    while True:
        print("\nTeam Merchandising Revenue System")
        print("1. Input Team Details")
        print("2. Input Merchandise Details")
        print("3. Calculate Team Revenue")
        print("4. Save Data")
        print("5. Load Data")
        print("6. Display Data")
        print("7. Exit")
        
        menu_choice = input("Enter your choice: ")

        if menu_choice == "1":
            print("Input Team Details")
            team_id = int(input("Enter Team ID: "))
            team_name = input("Enter Team Name: ")
            team = Team(team_id, team_name)

        elif menu_choice == "2":
            print("Input Merchandise Details")
            if not team:
                print("Please input team details first.")
            else:
                merch_id = int(input("Enter Merchandise ID: "))
                name = input("Enter Merchandise Name: ")
                price = float(input("Enter Merchandise Price: "))
                total_quantity = int(input("Enter Total Quantity: "))
                sold_quantity = int(input("Enter Sold Quantity: "))
                selling_price = float(input("Enter Selling Price: "))
                merchandise = Merchandise(merch_id, name, price, total_quantity, sold_quantity, selling_price)
                team.merchandise_list.append(merchandise)

        elif menu_choice == "3":
            if not team:
                print("Please input team details first.")
            else:
                production_cost = float(input("Enter Production Cost: "))
                advertising_cost = float(input("Enter Advertising Cost: "))
                total_revenue, profit = team.calculate_team_revenue(production_cost, advertising_cost)
                print(f"Total Revenue for Team {team.team_name}: ${total_revenue}")
                print(f"Profit for Team {team.team_name}: ${profit}")

        elif menu_choice == "4":
            if not team:
                print("Please input team details first.")
            else:
                data_to_save = {
                    "team_id": team.team_id,
                    "team_name": team.team_name,
                    "merchandise_list": [{"id": merch.id, "name": merch.name,
                                          "price": merch.price, "total_quantity": merch.total_quantity,
                                          "sold_quantity": merch.sold_quantity, "selling_price": merch.selling_price}
                                         for merch in team.merchandise_list]
                }
                save_data(data_to_save)
                print("Data saved successfully.")

        elif menu_choice == "5":
            loaded_data = load_data()
            if loaded_data:
                team_id = loaded_data.get("team_id")
                team_name = loaded_data.get("team_name")
                merchandise_list = loaded_data.get("merchandise_list", [])
                if team_id is not None and team_name is not None:
                    team = Team(team_id, team_name)
                    for merch_data in merchandise_list:
                        merch = Merchandise(merch_data["id"], merch_data["name"],
                                            merch_data["price"], merch_data["total_quantity"],
                                            merch_data["sold_quantity"], merch_data["selling_price"])
                        team.merchandise_list.append(merch)
                    print("Data loaded successfully.")
                else:
                    print("Loaded data is missing required fields.")
            else:
                print("No data found.")

        elif menu_choice == "6":
            if not team:
                print("Please input or load team details first.")
            else:
                print(f"Team ID: {team.team_id}, Team Name: {team.team_name}")
                print("Merchandise List:")
                for merch in team.merchandise_list:
                    print(f"- ID: {merch.id}, Name: {merch.name}, Price: {merch.price}, Total Quantity: {merch.total_quantity}, Sold Quantity: {merch.sold_quantity}, Selling Price: {merch.selling_price}")

        elif menu_choice == "7":
            print("Exiting the program.")
            break

        else:
            print("Invalid choice. Please enter a valid option.")

if __name__ == "__main__":
    main()
