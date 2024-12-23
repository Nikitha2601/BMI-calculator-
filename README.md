import tkinter as tk
from tkinter import messagebox

# Global variable to store the calculated BMI
calculated_bmi = None

# Function to calculate BMI
def calculate_bmi():
    global calculated_bmi  # Declare the variable as global to modify it
    try:
        height_cm = float(height_entry.get())
        weight = float(weight_entry.get())
        
        if height_cm > 0 and weight > 0:
            height_m = height_cm / 100  # Convert height from cm to meters
            bmi = weight / (height_m * height_m)
            calculated_bmi = round(bmi, 2)  # Store the BMI in the global variable
            
            if bmi < 18.5:
                result_text = f"Your BMI is {calculated_bmi}. You are Underweight."
            elif 18.5 <= bmi < 24.9:
                result_text = f"Your BMI is {calculated_bmi}. You have a Normal weight."
            elif 25 <= bmi < 29.9:
                result_text = f"Your BMI is {calculated_bmi}. You are Overweight."
            else:
                result_text = f"Your BMI is {calculated_bmi}. You are Obese."
            
            result_label.config(text=result_text)  # Update the label with the result
            advice_button.config(state=tk.NORMAL)  # Enable the health tips button
        else:
            messagebox.showerror("Input Error", "Height and Weight must be positive numbers!")
    except ValueError:
        messagebox.showerror("Input Error", "Please enter valid numbers for height and weight!")

# Function to provide health tips based on BMI
def show_health_tips():
    global calculated_bmi
    if calculated_bmi is not None:  # Check if BMI has been calculated
        tips = bmi_tips(calculated_bmi)  # Get health tips using the global BMI
        display_health_tips(tips)  # Show health tips in a full-screen window
    else:
        messagebox.showerror("Error", "Please calculate your BMI first!")

# Function to provide health tips based on BMI
def bmi_tips(bmi):
    if bmi < 18.5:
        return "You are underweight. It's important to eat a balanced diet with more calories. Consider consulting a nutritionist."
    elif 18.5 <= bmi < 24.9:
        return "You have a normal weight. Keep maintaining a balanced diet and regular physical activity."
    elif 25 <= bmi < 29.9:
        return "You are overweight. Consider a healthy diet and regular exercise to lose weight."
    else:
        return "You are in the obese category. It's recommended to consult a healthcare provider for personalized advice."

# Function to display health tips in a full-screen window
def display_health_tips(tips):
    tips_window = tk.Toplevel()
    tips_window.title("Health Tips")
    tips_window.attributes("-fullscreen", True)  # Make the window full screen

    # Create a frame for the tips
    frame = tk.Frame(tips_window, bg="#f0f0f0", padx=50, pady=50)
    frame.pack(expand=True)

    # Label to show health tips
    tips_label = tk.Label(frame, text=tips, font=("Helvetica", 24), wraplength=800, justify="center", bg="#f0f0f0")
    tips_label.pack(pady=20)

    # Button to close the full-screen window
    close_button = tk.Button(frame, text="Close", font=("Helvetica", 16), command=tips_window.destroy)
    close_button.pack(pady=20)

# Function to reset the inputs
def reset():
    global calculated_bmi
    height_entry.delete(0, tk.END)
    weight_entry.delete(0, tk.END)
    result_label.config(text="")
    advice_button.config(state=tk.DISABLED)  # Disable health tips button
    calculated_bmi = None  # Reset the BMI value

# Creating the main window
window = tk.Tk()
window.title("BMI Calculator")
window.geometry("300x300")

# Creating input fields and labels
height_label = tk.Label(window, text="Height (in centimeters):")
height_label.pack()

height_entry = tk.Entry(window)
height_entry.pack()

weight_label = tk.Label(window, text="Weight (in kilograms):")
weight_label.pack()

weight_entry = tk.Entry(window)
weight_entry.pack()

# Button to calculate BMI
calculate_button = tk.Button(window, text="Calculate BMI", command=calculate_bmi)
calculate_button.pack()

# Button to show health tips based on calculated BMI
advice_button = tk.Button(window, text="Show Health Tips", command=show_health_tips, state=tk.DISABLED)
advice_button.pack()

# Button to reset fields
reset_button = tk.Button(window, text="Reset", command=reset)
reset_button.pack()

# Label to display result
result_label = tk.Label(window, text="")
result_label.pack()

# Start the GUI event loop
window.mainloop()
