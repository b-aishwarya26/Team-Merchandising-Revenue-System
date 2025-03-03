# Team Merchandising Revenue System

## Overview
The **Team Merchandising Revenue System** is a Streamlit-based web application that allows users to manage and analyze team merchandise sales. Users can input team details, add merchandise data, calculate revenue and profit, and save/load data for future use.

## Features
- **Input Team Details**: Enter the team ID and name.
- **Input Merchandise Details**: Add merchandise with attributes like price, total quantity, sold quantity, and selling price.
- **Calculate Revenue**: Compute the total revenue and profit based on production and advertising costs.
- **Save Data**: Store team and merchandise details in a JSON file.
- **Load Data**: Retrieve previously saved data.
- **Display Data**: View stored team and merchandise information.
- **Display Image**: Show an image related to the merchandise.

## Installation
To run this project, follow these steps:

### Prerequisites
- Python installed (>=3.7)
- Streamlit installed (`pip install streamlit`)

### Steps
1. Clone or download this repository.
2. Install the required dependencies:
   ```sh
   pip install streamlit
   ```
3. Run the Streamlit application:
   ```sh
   streamlit run app.py
   ```

## File Structure
```
|-- app.py               # Main application file
|-- data.json            # JSON file to store team and merchandise data
|-- README.md            # Project documentation
```

## Usage
1. Start the Streamlit app.
2. Use the sidebar menu to navigate:
   - Enter team details first.
   - Add merchandise details.
   - Calculate revenue.
   - Save or load data.
   - Display stored data.
3. The system calculates revenue as:
   ```
   Total Revenue = Selling Price × Sold Quantity (for all merchandise)
   Profit = Total Revenue - (Production Cost + Advertising Cost)
   ```

## Example Input
### Team Details
```
Team ID: 101
Team Name: Warriors
```
### Merchandise Details
```
ID: 1
Name: Jersey
Price: 500
Total Quantity: 100
Sold Quantity: 80
Selling Price: 700
```
### Revenue Calculation
```
Production Cost: 20000
Advertising Cost: 5000
Total Revenue: 56000
Profit: 31000
```

## Known Issues
- Ensure the `data.json` file exists before loading data.
- Enter valid numeric inputs to avoid errors.

## Contributing
Feel free to submit issues or pull requests to enhance the project.

## License
This project is open-source and available under the MIT License.

