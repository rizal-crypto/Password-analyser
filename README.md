# Password-analyser
import re
import tkinter as tk
from tkinter import messagebox

# Function to check password strength
def password_strength(password):
    score = 0
    feedback = []

    # Length check
    if len(password) >= 8:
        score += 1
    else:
        feedback.append("Use at least 8 characters")
    if len(password) >= 12:
        score += 1

    # Uppercase letters
    if re.search(r"[A-Z]", password):
        score += 1
    else:
        feedback.append("Add uppercase letters")

    # Lowercase letters
    if re.search(r"[a-z]", password):
        score += 1
    else:
        feedback.append("Add lowercase letters")

    # Numbers
    if re.search(r"[0-9]", password):
        score += 1
    else:
        feedback.append("Add numbers")

    # Special characters
    if re.search(r"[!@#$%^&*()_+=-]", password):
        score += 1
    else:
        feedback.append("Add special characters (!@#$%^&*)")

    # Common passwords check
    common_passwords = ["password", "123456", "qwerty", "admin", "letmein"]
    if password.lower() in common_passwords:
        score = 0
        feedback = ["Very Weak – Common password"]

    # Determine strength
    if score <= 2:
        strength = "Weak"
        color = "red"
    elif score <= 4:
        strength = "Medium"
        color = "orange"
    else:
        strength = "Strong"
        color = "green"

    return strength, color, feedback

# Function to update GUI
def check_password():
    pwd = entry.get()
    strength, color, feedback = password_strength(pwd)
    result_label.config(text=f"Strength: {strength}", fg=color)
    feedback_label.config(text="\n".join(feedback))

# GUI setup
root = tk.Tk()
root.title("Password Strength Analyzer")
root.geometry("400x300")

tk.Label(root, text="Enter Password:").pack(pady=10)
entry = tk.Entry(root, show="*", width=30)
entry.pack(pady=5)

check_btn = tk.Button(root, text="Check Strength", command=check_password)
check_btn.pack(pady=10)

result_label = tk.Label(root, text="Strength: ", font=("Arial", 14))
result_label.pack(pady=10)

feedback_label = tk.Label(root, text="", fg="blue")
feedback_label.pack(pady=10)

root.mainloop()
