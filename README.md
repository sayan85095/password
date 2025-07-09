# password
Here I create password inn python making GUI

import random
import string
import tkinter as tk
from tkinter import ttk, messagebox

def generate_password(length, use_uppercase, use_numbers, use_special):
    """Generate a random password based on specified criteria."""
    characters = string.ascii_lowercase  # Start with lowercase letters

    if use_uppercase:
        characters += string.ascii_uppercase
    if use_numbers:
        characters += string.digits
    if use_special:
        characters += string.punctuation

    if length < 1 or not characters:
        raise ValueError("Invalid password length or character set.")

    password = ''.join(random.choice(characters) for _ in range(length))
    return password

def run_cli_password_generator():
    """Command-line password generator."""
    print("\n🔑 Password Generator")
    
    while True:
        try:
            length = int(input("Enter desired password length: "))
            use_uppercase = input("Include uppercase letters? (y/n): ").strip().lower() == 'y'
            use_numbers = input("Include numbers? (y/n): ").strip().lower() == 'y'
            use_special = input("Include special characters? (y/n): ").strip().lower() == 'y'
            
            password = generate_password(length, use_uppercase, use_numbers, use_special)
            print(f"Generated Password: {password}")
            
            if input("Generate another password? (y/n): ").strip().lower() != 'y':
                break
                
        except ValueError as e:
            print(f"Error: {e}")
        except Exception as e:
            print(f"Unexpected error: {e}")

class PasswordGeneratorGUI(tk.Tk):
    """Graphical password generator using Tkinter."""
    def __init__(self):
        super().__init__()
        self.title("🔑 Password Generator")
        self.geometry("400x300")
        self.resizable(False, False)
        
        self.create_widgets()
    
    def create_widgets(self):
        # Length input
        ttk.Label(self, text="Password Length:").pack(pady=10)
        self.length_var = tk.IntVar(value=12)
        length_entry = ttk.Entry(self, textvariable=self.length_var)
        length_entry.pack(pady=5)
        
        # Checkboxes for options
        self.uppercase_var = tk.BooleanVar(value=True)
        self.numbers_var = tk.BooleanVar(value=True)
        self.special_var = tk.BooleanVar(value=True)
        
        ttk.Checkbutton(self, text="Include Uppercase Letters", variable=self.uppercase_var).pack(anchor=tk.W)
        ttk.Checkbutton(self, text="Include Numbers", variable=self.numbers_var).pack(anchor=tk.W)
        ttk.Checkbutton(self, text="Include Special Characters", variable=self.special_var).pack(anchor=tk.W)
        
        # Generate button
        generate_button = ttk.Button(self, text="Generate Password", command=self.generate_password)
        generate_button.pack(pady=20)
        
        # Result display
        self.result_var = tk.StringVar()
        result_label = ttk.Label(self, textvariable=self.result_var, font=('Arial', 14))
        result_label.pack(pady=10)
    
    def generate_password(self):
        """Generate password and display it."""
        try:
            length = self.length_var.get()
            use_uppercase = self.uppercase_var.get()
            use_numbers = self.numbers_var.get()
            use_special = self.special_var.get()
            
            password = generate_password(length, use_uppercase, use_numbers, use_special)
            self.result_var.set(password)
        except ValueError as e:
            messagebox.showerror("Error", str(e))
        except Exception as e:
            messagebox.showerror("Error", f"Unexpected error: {str(e)}")

if __name__ == "__main__":
    print("Choose password generator mode:")
    print("1. Command Line (Terminal)")
    print("2. Graphical Interface")
    
    while True:
        choice = input("Select 1 or 2: ").strip()
        if choice == '1':
            run_cli_password_generator()
            break
        elif choice == '2':
            app = PasswordGeneratorGUI()
            app.mainloop()
            break
        else:
            print("Invalid choice. Please enter 1 or 2.")
![Screenshot 2025-07-09 114232](https://github.com/user-attachments/assets/13f0656d-adff-4588-b191-09e3ffd24417)

