# Team-Merchandising-Revenue-System
import streamlit as st
import json

def display_image(url):
    st.image(url, caption='Brand', use_column_width=True)
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
    st.title("Team Merchandising Revenue System")
    image_url = "C:/Users/aishw/Downloads/merch1.jpg"

    # Display the image
    display_image(image_url)

    st.sidebar.header("Menu")

    input_team = st.sidebar.checkbox("Input Team Details")
    input_merchandise = st.sidebar.checkbox("Input Merchandise Details")
    calculate_revenue = st.sidebar.checkbox("Calculate Team Revenue")
    save_data_btn = st.sidebar.checkbox("Save Data")
    load_data_btn = st.sidebar.checkbox("Load Data")
    display_data_btn = st.sidebar.checkbox("Display Data")

    team = None
    if input_team:
        st.subheader("Input Team Details")
        team_id = st.number_input("Enter Team ID", step=1)
        team_name = st.text_input("Enter Team Name")
        team = Team(team_id, team_name)
        if st.button("Submit Team Details"):
            pass

    if input_merchandise:
        st.subheader("Input Merchandise Details")
        if not team:
            st.error("Please input team details first.")
        else:
            merch_id = st.number_input("Enter Merchandise ID", step=1)
            name = st.text_input("Enter Merchandise Name")
            price = st.number_input("Enter Merchandise Price", step=0.01)
            total_quantity = st.number_input("Enter Total Quantity", step=1)
            sold_quantity = st.number_input("Enter Sold Quantity", step=1)
            selling_price = st.number_input("Enter Selling Price", step=0.01)
            merchandise = Merchandise(merch_id, name, price, total_quantity, sold_quantity, selling_price)
            team.merchandise_list.append(merchandise)
            if st.button("Submit Merchandise Details"):
                pass

    if calculate_revenue:
        if not team:
            st.error("Please input team details first.")
        else:
            production_cost = st.number_input("Enter Production Cost", step=0.01)
            advertising_cost = st.number_input("Enter Advertising Cost", step=0.01)
            if st.button("Calculate Revenue"):
                total_revenue, profit = team.calculate_team_revenue(production_cost, advertising_cost)
                st.write(f"Total Revenue for Team {team.team_name}: ${total_revenue}")
                st.write(f"Profit for Team {team.team_name}: ${profit}")

    if save_data_btn:
        if not team:
            st.error("Please input team details first.")
        else:
            if st.button("Save Data"):
                data_to_save = {
                    "team_id": team.team_id,
                    "team_name": team.team_name,
                    "merchandise_list": [{"id": merch.id, "name": merch.name,
                                          "price": merch.price, "total_quantity": merch.total_quantity,
                                          "sold_quantity": merch.sold_quantity, "selling_price": merch.selling_price}
                                         for merch in team.merchandise_list]
                }
                save_data(data_to_save)
                st.success("Data saved successfully.")

    if load_data_btn:
        if st.button("Load Data"):
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
                    st.success("Data loaded successfully.")
                else:
                    st.error("Loaded data is missing required fields.")
            else:
                st.error("No data found.")

    if display_data_btn:
        if not team:
            st.error("Please input or load team details first.")
        else:
            if st.button("Display Data"):
                st.write(f"Team ID: {team.team_id}, Team Name: {team.team_name}")
                st.write("Merchandise List:")
                for merch in team.merchandise_list:
                    st.write(f"- ID: {merch.id}, Name: {merch.name}, Price: {merch.price}, Total Quantity: {merch.total_quantity}, Sold Quantity: {merch.sold_quantity}, Selling Price: {merch.selling_price}")

if __name__ == "__main__":
    main()
