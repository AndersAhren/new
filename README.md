import tkinter as tk
from tkinter import messagebox
import requests
import json

# API credentials from Edamam API
API_ID = "your_api_id_here"
API_KEY = "your_api_key_here"
API_URL = "https://api.edamam.com/search"

# Function to fetch recipes based on ingredients
def get_recipes(ingredients):
    params = {
        "q": ingredients,
        "app_id": API_ID,
        "app_key": API_KEY,
        "to": 5  # Number of recipes to fetch
    }
    response = requests.get(API_URL, params=params)
    data = response.json()

    if "hits" in data:
        recipes = []
        for hit in data["hits"]:
            recipe = hit["recipe"]
            recipes.append(f"{recipe['label']}\nLink: {recipe['url']}\n")
        return recipes
    else:
        return ["No recipes found. Please try different ingredients."]

# Function to handle button click and display recipes
def find_food():
    ingredients = entry.get()
    if not ingredients:
        messagebox.showwarning("Input Error", "Please enter some ingredients.")
        return
    
    recipes = get_recipes(ingredients)
    
    # Display the recipes in the text widget
    results.delete(1.0, tk.END)  # Clear previous results
    for recipe in recipes:
        results.insert(tk.END, recipe + "\n")

# Create the main window
root = tk.Tk()
root.title("Food Finder AI App")

# Create and place the widgets
label = tk.Label(root, text="Enter ingredients (comma separated):")
label.pack(pady=10)

entry = tk.Entry(root, width=50)
entry.pack(pady=5)

button = tk.Button(root, text="Find Recipes", command=find_food)
button.pack(pady=10)

results = tk.Text(root, height=10, width=50)
results.pack(pady=10)

# Start the GUI event loop
root.mainloop()
