# Personal Assistant Program
# Auto-install required packages if missing

try:#catch and handle errors (exceptions)
    import pandas as pd
except ImportError:#used to handle the error when module not installed
    import os
    os.system('pip install pandas')#to install pandas library
    import pandas as pd

try:
    import matplotlib.pyplot as plt
except ImportError:
    import os
    os.system('pip install matplotlib')#to install matplotlib library
    import matplotlib.pyplot as plt

import datetime#function
import os
import sys
import time
#import math
import csv

notes_file = "notes.txt"#in text format
passwords_file = "passwords.csv"
todos_file = "todo.txt"

def clear_screen():
    os.system('cls' if os.name == 'nt' else 'clear')#is used to clear the terminal screen

# ----- 1. Date and Time -----
def show_datetime():
    clear_screen()
    now = datetime.datetime.now()#function
    print("\nCurrent Date and Time:")
    print(now.strftime("%Y-%m-%d %H:%M:%S"))#format datetime objects into readable strings
    input("\nPress Enter to continue...")

# ----- 2. Notes Section -----
def notes_section():
    while True:
        clear_screen()
        print("\n--- Notes Section ---")
        print("1. View Notes")
        print("2. Write Note")
        print("3. Edit Notes")
        print("4. Back to Main Menu")
        choice = input("Enter your choice: ")

        if choice == '1': 
            try:
                with open(notes_file, 'r') as file:#opening the file
                    content = file.read()#to read the file 
                    print("\nYour Notes:\n")
                    print(content if content else "No notes found.")#content is truthy the entire expression returns content; otherwise, it returns the alternative value specified after else.
            except FileNotFoundError:#if any error occurs of file not found
                print("No notes found.")
            input("\nPress Enter to continue...")
        elif choice == '2':
            note = input("Enter your note: ")
            with open(notes_file, 'a') as file:#to open a file named notes_file in append mode where txt will be added in end
                file.write(note + '\n')#to write the note and then continue in nxt line
            print("Note saved.")
            input("\nPress Enter to continue...")

        elif choice == '3':
            try:
                with open(notes_file, 'r') as file:# to open a file named notes_file in read mode
                    lines = file.readlines()#to read the lines
                if lines:
                    print("\nNotes:")
                    for i, line in enumerate(lines):#provides both the line number (i) and the line content (line) for each iteration. 
                        print(f"{i + 1}. {line.strip()}")#f-string feature allows for inline expression evaluation within string literals and i+1 change 0 index to 1 and strip removes leading trailing whitespaces characters
                    line_no = int(input("\nEnter line number to edit: ")) - 1
                    if 0 <= line_no < len(lines):
                        new_line = input("Enter new content: ")
                        lines[line_no] = new_line + '\n'#in postion of line no of line new content wil be added
                        with open(notes_file, 'w') as file:#to open the file in write mode
                            file.writelines(lines)#can write multiple lines in one call
                        print("Note updated.")
                    else:
                        print("Invalid line number.")
                else:
                    print("No notes to edit.")
            except Exception as e:#if any unknown error occur
                print("Error:", e)
            input("\nPress Enter to continue...")

        elif choice == '4':
            break
        else:
            print("Invalid choice.")
            time.sleep(1)#pauses the execution of the current thread for 1 second

# ----- 3. Advanced Calculator -----
def calculator():
    while True:
        clear_screen()
        print("\n--- Calculator ---")
        print("1. Add")
        print("2. Subtract")
        print("3. Multiply")
        print("4. Divide")
        print("5. Square Root")
        print("6. Power")
        print("7. Logarithm")
        print("8. Factorial")
        print("9. Back to Main Menu")
        choice = input("Enter your choice: ")

        if choice == '9':
            break
        try:
            if choice in ['1', '2', '3', '4']:
                a = float(input("Enter first number: "))
                b = float(input("Enter second number: "))
                if choice == '1':
                    print("Result:", a + b)
                elif choice == '2':
                    print("Result:", a - b)
                elif choice == '3':
                    print("Result:", a * b)
                elif choice == '4':
                    if b == 0:
                        print("Cannot divide by zero.")
                    else:
                        print("Result:", a / b)

            elif choice == '5':
                num = float(input("Enter number: "))
                if num < 0:
                    print("Cannot take square root of negative number.")
                else:
                    print("Result:", math.sqrt(num))

            elif choice == '6':
                base = float(input("Enter base: "))
                exp = float(input("Enter exponent: "))
                print("Result:", math.pow(base, exp))

            elif choice == '7':
                num = float(input("Enter number: "))
                if num <= 0:
                    print("Logarithm undefined for zero or negative numbers.")
                else:
                    print("Result:", math.log(num))

            elif choice == '8':
                num = int(input("Enter a non-negative integer: "))
                if num < 0:
                    print("Factorial not defined for negative numbers.")
                else:
                    print("Result:", math.factorial(num))

            else:
                print("Invalid choice.")
        except Exception as e:
            print("Error:", e)
        input("\nPress Enter to continue...")

# ----- 4. Unit Converter -----
def unit_converter():
    while True:
        clear_screen()
        print("\n--- Unit Converter ---")
        print("1. Kg to Grams")
        print("2. Meters to Centimeters")
        print("3. Celsius to Fahrenheit")
        print("4. Inches to Centimeters")
        print("5. Liters to Milliliters")
        print("6. Back to Main Menu")
        choice = input("Enter your choice: ")

        if choice == '1':
            kg = float(input("Enter weight in Kg: "))
            print("Result:", kg * 1000, "grams")

        elif choice == '2':
            meters = float(input("Enter length in meters: "))
            print("Result:", meters * 100, "centimeters")

        elif choice == '3':
            celsius = float(input("Enter temperature in Celsius: "))
            print("Result:", (celsius * 9 / 5) + 32, "Fahrenheit")

        elif choice == '4':
            inches = float(input("Enter length in inches: "))
            print("Result:", inches * 2.54, "centimeters")

        elif choice == '5':
            liters = float(input("Enter volume in liters: "))
            print("Result:", liters * 1000, "milliliters")

        elif choice == '6':
            break
        else:
            print("Invalid choice.")
        input("\nPress Enter to continue...")

# ----- 5. Password Manager -----
def password_manager():
    while True:
        clear_screen()
        print("\n--- Password Manager ---")
        print("1. View Passwords")
        print("2. Add Password")
        print("3. Back to Main Menu")
        choice = input("Enter your choice: ")

        if choice == '1':
            try:
                with open(passwords_file, newline='') as file:
                    reader = csv.reader(file)
                    print("\nStored Passwords:\n")
                    for row in reader:
                        if len(row) >= 3:
                            print(f"Website: {row[0]}, Username: {row[1]}, Password: {row[2]}")
            except FileNotFoundError:
                print("No passwords saved.")
            input("\nPress Enter to continue...")

        elif choice == '2':
            website = input("Website: ")
            username = input("Username: ")
            password = input("Password: ")
            with open(passwords_file, 'a', newline='') as file:
                writer = csv.writer(file)
                writer.writerow([website, username, password])
            print("Password saved.")
            input("\nPress Enter to continue...")

        elif choice == '3':
            break
        else:
            print("Invalid choice.")
            time.sleep(1)

# ----- 6. Data Plotter -----
def data_plotter():
    clear_screen()
    print("\n--- Data Plotter (Daily Routine) ---")
    tasks = []
    hours = []
    while True:
        task = input("Enter task name (or type 'done' to finish): ")
        if task.lower() == 'done':
            break
        try:
            hour = float(input(f"Enter hours spent on '{task}': "))
            tasks.append(task)
            hours.append(hour)
        except ValueError:
            print("Invalid input. Please enter a number for hours.")

    if not tasks:
        print("No data entered.")
        input("\nPress Enter to continue...")
        return

    data = {'Tasks': tasks, 'Hours': hours}
    df = pd.DataFrame(data)
    plt.figure(figsize=(6, 6))
    plt.pie(df['Hours'], labels=df['Tasks'], autopct='%1.1f%%', startangle=90)
    plt.title('Custom Daily Routine')
    plt.axis('equal')
    plt.tight_layout()
    image_file = 'daily_routine_pie_chart.png'
    plt.savefig(image_file)
    print(f"\nPie chart saved as '{image_file}'")
    plt.show()
    input("\nPress Enter to continue...")

# ----- 7. To-Do List Manager -----
def todo_manager():
    while True:
        clear_screen()
        print("\n--- To-Do List Manager ---")
        print("1. View To-Do List")
        print("2. Add To-Do Item")
        print("3. Remove To-Do Item")
        print("4. Back to Main Menu")
        choice = input("Enter your choice: ")

        if choice == '1':
            try:
                with open(todos_file, 'r') as file:
                    todos = file.readlines()
                    if todos:
                        print("\nYour To-Do List:")
                        for idx, task in enumerate(todos, 1):
                            print(f"{idx}. {task.strip()}")
                    else:
                        print("To-Do list is empty.")
            except FileNotFoundError:
                print("To-Do list is empty.")
            input("\nPress Enter to continue...")

        elif choice == '2':
            task = input("Enter a new task: ")
            with open(todos_file, 'a') as file:
                file.write(task + '\n')
            print("Task added.")
            input("\nPress Enter to continue...")

        elif choice == '3':
            try:
                with open(todos_file, 'r') as file:
                    todos = file.readlines()
                if todos:
                    print("\nTo-Do List:")
                    for idx, task in enumerate(todos, 1):
                        print(f"{idx}. {task.strip()}")
                    remove_idx = int(input("Enter task number to remove: ")) - 1
                    if 0 <= remove_idx < len(todos):
                        removed_task = todos.pop(remove_idx)
                        with open(todos_file, 'w') as file:
                            file.writelines(todos)
                        print(f"Removed task: {removed_task.strip()}")
                    else:
                        print("Invalid task number.")
                else:
                    print("To-Do list is empty.")
            except Exception as e:
                print("Error:", e)
            input("\nPress Enter to continue...")

        elif choice == '4':
            break
        else:
            print("Invalid choice.")
            time.sleep(1)

# ----- Main Menu -----
def main():
    while True:
        clear_screen()
        print("\n--- Personal Assistant ---")
        print("1. Show Date and Time")
        print("2. Notes / Task Section")
        print("3. Calculator")
        print("4. Unit Converter")
        print("5. Password Manager")
        print("6. Data Plotter (Daily Routine)")
        print("7. To-Do List Manager")
        print("8. About")
        print("9. Exit")

        choice = input("Enter your choice: ")

        if choice == '1':
            show_datetime()
        elif choice == '2':
            notes_section()
        elif choice == '3':
            calculator()
        elif choice == '4':
            unit_converter()
        elif choice == '5':
            password_manager()
        elif choice == '6':
            data_plotter()
        elif choice == '7':
            todo_manager()
        elif choice == '8':
            clear_screen()
            print("\n--- About This Project ---")
            print("This is a Python-based Personal Assistant created by 'Niharika Sharma' from 12th-grade Section-C")
            print("Features include: date/time display, notes manager, calculator, unit converter,")
            print("password manager, a visual data plotter, and a to-do list manager.")
            print("Built using Python, Pandas, Matplotlib.")
            input("\nPress Enter to continue...")
        elif choice == '9':
            print("\nThank you for using the Personal Assistant. Goodbye!")
            break
        else:
            print("Invalid choice. Try again.")
            time.sleep(1)

if __name__ == "__main__":
    main() 


'''try:
    import pandas as pd
except ImportError:
    import os
    os.system('pip install pandas')
    import pandas as pd

try:
    import matplotlib.pyplot as plt
except ImportError:
    import os
    os.system('pip install matplotlib')
    import matplotlib.pyplot as plt

import datetime
import os
import sys
import time
import math

notes_file = "notes.xlsx"
passwords_file = "passwords.xlsx"
todos_file = "todo.xlsx"

def clear_screen():
    os.system('cls' if os.name == 'nt' else 'clear')

# ----- 1. Date and Time -----
def show_datetime():
    clear_screen()
    now = datetime.datetime.now()
    print("\nCurrent Date and Time:")
    print(now.strftime("%Y-%m-%d %H:%M:%S"))
    input("\nPress Enter to continue...")

# ----- 2. Notes Section -----
def notes_section():
    while True:
        clear_screen()
        print("\n--- Notes Section ---")
        print("1. View Notes")
        print("2. Write Note")
        print("3. Edit Notes")
        print("4. Back to Main Menu")
        choice = input("Enter your choice: ")

        if choice == '1':
            try:
                df = pd.read_excel(notes_file)
                if df.empty:
                    print("\nNo notes found.")
                else:
                    print("\nYour Notes:\n")
                    for idx, row in df.iterrows():
                        print(f"{idx + 1}. {row['Note']}")
            except FileNotFoundError:
                print("No notes found.")
            input("\nPress Enter to continue...")

        elif choice == '2':
            note = input("Enter your note: ")
            try:
                df = pd.read_excel(notes_file)
                df = df.append({'Note': note}, ignore_index=True)
            except FileNotFoundError:
                df = pd.DataFrame({'Note': [note]})
            df.to_excel(notes_file, index=False)
            print("Note saved.")
            input("\nPress Enter to continue...")

        elif choice == '3':
            try:
                df = pd.read_excel(notes_file)
                if df.empty:
                    print("No notes to edit.")
                else:
                    print("\nNotes:")
                    for idx, row in df.iterrows():
                        print(f"{idx + 1}. {row['Note']}")
                    line_no = int(input("\nEnter line number to edit: ")) - 1
                    if 0 <= line_no < len(df):
                        new_line = input("Enter new content: ")
                        df.at[line_no, 'Note'] = new_line
                        df.to_excel(notes_file, index=False)
                        print("Note updated.")
                    else:
                        print("Invalid line number.")
            except FileNotFoundError:
                print("No notes to edit.")
            except Exception as e:
                print("Error:", e)
            input("\nPress Enter to continue...")

        elif choice == '4':
            break
        else:
            print("Invalid choice.")
            time.sleep(1)

# ----- 3. Advanced Calculator -----
def calculator():
    while True:
        clear_screen()
        print("\n--- Calculator ---")
        print("1. Add")
        print("2. Subtract")
        print("3. Multiply")
        print("4. Divide")
        print("5. Square Root")
        print("6. Power")
        print("7. Logarithm")
        print("8. Factorial")
        print("9. Back to Main Menu")
        choice = input("Enter your choice: ")

        if choice == '9':
            break
        try:
            if choice in ['1', '2', '3', '4']:
                a = float(input("Enter first number: "))
                b = float(input("Enter second number: "))
                if choice == '1':
                    print("Result:", a + b)
                elif choice == '2':
                    print("Result:", a - b)
                elif choice == '3':
                    print("Result:", a * b)
                elif choice == '4':
                    if b == 0:
                        print("Cannot divide by zero.")
                    else:
                        print("Result:", a / b)

            elif choice == '5':
                num = float(input("Enter number: "))
                if num < 0:
                    print("Cannot take square root of negative number.")
                else:
                    print("Result:", math.sqrt(num))

            elif choice == '6':
                base = float(input("Enter base: "))
                exp = float(input("Enter exponent: "))
                print("Result:", math.pow(base, exp))

            elif choice == '7':
                num = float(input("Enter number: "))
                if num <= 0:
                    print("Logarithm undefined for zero or negative numbers.")
                else:
                    print("Result:", math.log(num))

            elif choice == '8':
                num = int(input("Enter a non-negative integer: "))
                if num < 0:
                    print("Factorial not defined for negative numbers.")
                else:
                    print("Result:", math.factorial(num))

            else:
                print("Invalid choice.")
        except Exception as e:
            print("Error:", e)
        input("\nPress Enter to continue...")

# ----- 4. Unit Converter -----
def unit_converter():
    while True:
        clear_screen()
        print("\n--- Unit Converter ---")
        print("1. Kg to Grams")
        print("2. Meters to Centimeters")
        print("3. Celsius to Fahrenheit")
        print("4. Inches to Centimeters")
        print("5. Liters to Milliliters")
        print("6. Back to Main Menu")
        choice = input("Enter your choice: ")

        if choice == '1':
            kg = float(input("Enter weight in Kg: "))
            print("Result:", kg * 1000, "grams")

        elif choice == '2':
            meters = float(input("Enter length in meters: "))
            print("Result:", meters * 100, "centimeters")

        elif choice == '3':
            celsius = float(input("Enter temperature in Celsius: "))
            print("Result:", (celsius * 9 / 5) + 32, "Fahrenheit")

        elif choice == '4':
            inches = float(input("Enter length in inches: "))
            print("Result:", inches * 2.54, "centimeters")

        elif choice == '5':
            liters = float(input("Enter volume in liters: "))
            print("Result:", liters * 1000, "milliliters")

        elif choice == '6':
            break
        else:
            print("Invalid choice.")
        input("\nPress Enter to continue...")

# ----- 5. Password Manager -----
def password_manager():
    while True:
        clear_screen()
        print("\n--- Password Manager ---")
        print("1. View Passwords")
        print("2. Add Password")
        print("3. Back to Main Menu")
        choice = input("Enter your choice: ")

        if choice == '1':
            try:
                df = pd.read_excel(passwords_file)
                if df.empty:
                    print("No passwords saved.")
                else:
                    print("\nStored Passwords:\n")
                    for idx, row in df.iterrows():
                        print(f"Website: {row['Website']}, Username: {row['Username']}, Password: {row['Password']}")
            except FileNotFoundError:
                print("No passwords saved.")
            input("\nPress Enter to continue...")

        elif choice == '2':
            website = input("Website: ")
            username = input("Username: ")
            password = input("Password: ")
            try:
                df = pd.read_excel(passwords_file)
                df = df.append({'Website': website, 'Username': username, 'Password': password}, ignore_index=True)
            except FileNotFoundError:
                df = pd.DataFrame({'Website': [website], 'Username': [username], 'Password': [password]})
            df.to_excel(passwords_file, index=False)
            print("Password saved.")
            input("\nPress Enter to continue...")

        elif choice == '3':
            break
        else:
            print("Invalid choice.")
            time.sleep(1)

# ----- 6. Data Plotter -----
def data_plotter():
    clear_screen()
    print("\n--- Data Plotter (Daily Routine) ---")
    tasks = []
    hours = []
    while True:
        task = input("Enter task name (or type 'done' to finish): ")
        if task.lower() == 'done':
            break
        try:
            hour = float(input(f"Enter hours spent on '{task}': "))
            tasks.append(task)
            hours.append(hour)
        except ValueError:
            print("Invalid input. Please enter a number for hours.")

    if not tasks:
        print("No data entered.")
        input("\nPress Enter to continue...")
        return

    data = {'Tasks': tasks, 'Hours': hours}
    df = pd.DataFrame(data)
    plt.figure(figsize=(6, 6))
    plt.pie(df['Hours'], labels=df['Tasks'], autopct='%1.1f%%', startangle=90)
    plt.title('Custom Daily Routine')
    plt.axis('equal')
    plt.tight_layout()
    image_file = 'daily_routine_pie_chart.png'
    plt.savefig(image_file)
    print(f"\nPie chart saved as '{image_file}'")
    plt.show()
    input("\nPress Enter to continue...")

# ----- 7. To-Do List Manager -----
def todo_manager():
    while True:
        clear_screen()
        print("\n--- To-Do List Manager ---")
        print("1. View To-Do List")
        print("2. Add To-Do Item")
        print("3. Remove To-Do Item")
        print("4. Back to Main Menu")
        choice = input("Enter your choice: ")

        if choice == '1':
            try:
                df = pd.read_excel(todos_file)
                if df.empty:
                    print("To-Do list is empty.")
                else:
                    print("\nYour To-Do List:")
                    for idx, row in df.iterrows():
                        print(f"{idx + 1}. {row['Task']}")
            except FileNotFoundError:
                print("To-Do list is empty.")
            input("\nPress Enter to continue...")

        elif choice == '2':
            task = input("Enter a new task: ")
            try:
                df = pd.read_excel(todos_file)
                df = df.append({'Task': task}, ignore_index=True)
            except FileNotFoundError:
                df = pd.DataFrame({'Task': [task]})
            df.to_excel(todos_file, index=False)
            print("Task added.")
            input("\nPress Enter to continue...")

        elif choice == '3':
            try:
                df = pd.read_excel(todos_file)
                if df.empty:
                    print("To-Do list is empty.")
                else:
                    print("\nTo-Do List:")
                    for idx, row in df.iterrows():
                        print(f"{idx + 1}. {row['Task']}")
                    remove_idx = int(input("Enter task number to remove: ")) - 1
                    if 0 <= remove_idx < len(df):
                        removed_task = df.at[remove_idx, 'Task']
                        df = df.drop(remove_idx).reset_index(drop=True)
                        df.to_excel(todos_file, index=False)
                        print(f"Removed task: {removed_task}")
                    else:
                        print("Invalid task number.")
            except FileNotFoundError:
                print("To-Do list is empty.")
            except Exception as e:
                print("Error:", e)
            input("\nPress Enter to continue...")

        elif choice == '4':
            break
        else:
            print("Invalid choice.")
            time.sleep(1)

# ----- 8. About Section -----
def about_section():
    clear_screen()
    print("\n--- About This Personal Assistant ---")
    print("Created by: Your Name")
    print("Version: 1.0")
    print("This assistant helps with notes, to-do lists, password management, calculations, unit conversions, and data plotting.")
    input("\nPress Enter to continue...")

# ----- Main Menu -----
def main():
    while True:
        clear_screen()
        print("=== Personal Assistant ===")
        print("1. Show Date and Time")
        print("2. Notes Section")
        print("3. Advanced Calculator")
        print("4. Unit Converter")
        print("5. Password Manager")
        print("6. Data Plotter (Daily Routine)")
        print("7. To-Do List Manager")
        print("8. About")
        print("9. Exit")

        choice = input("Enter your choice: ")
        if choice == '1':
            show_datetime()
        elif choice == '2':
            notes_section()
        elif choice == '3':
            calculator()
        elif choice == '4':
            unit_converter()
        elif choice == '5':
            password_manager()
        elif choice == '6':
            data_plotter()
        elif choice == '7':
            todo_manager()
        elif choice == '8':
            about_section()
        elif choice == '9':
            print("Goodbye!")
            time.sleep(1)
            break
        else:
            print("Invalid choice.")
            time.sleep(1)

if __name__ == "__main__":
    main()'''
    
    
'''import pandas as pd
import matplotlib.pyplot as plt
import datetime
import os
import sys
import time
import math

# Single Excel database
excel_file = "niharika_projectt.xlsx"

def read_sheet(sheet_name):
    try:
        df = pd.read_excel(excel_file, sheet_name=sheet_name)
    except FileNotFoundError:
        df = pd.DataFrame()
    except ValueError:
        df = pd.DataFrame()
    return df

def write_sheet(df, sheet_name):
    try:
        with pd.ExcelWriter(excel_file, mode='a', if_sheet_exists='replace', engine='openpyxl') as writer:
            df.to_excel(writer, sheet_name=sheet_name, index=False)
    except FileNotFoundError:
        with pd.ExcelWriter(excel_file, engine='openpyxl') as writer:
            df.to_excel(writer, sheet_name=sheet_name, index=False)

def clear_screen():
    os.system('cls' if os.name == 'nt' else 'clear')

def show_datetime():
    clear_screen()
    now = datetime.datetime.now()
    print("\nCurrent Date and Time:")
    print(now.strftime("%Y-%m-%d %H:%M:%S"))
    input("\nPress Enter to continue...")

def notes_section():
    while True:
        clear_screen()
        print("\n--- Notes Section ---")
        print("1. View Notes")
        print("2. Write Note")
        print("3. Edit Notes")
        print("4. Back to Main Menu")
        choice = input("Enter your choice: ")

        if choice == '1':
            df = read_sheet("Notes")
            if df.empty:
                print("\nNo notes found.")
            else:
                print("\nYour Notes:\n")
                for idx, row in df.iterrows():
                    print(f"{idx + 1}. {row['Note']}")
            input("\nPress Enter to continue...")

        elif choice == '2':
            note = input("Enter your note: ")
            df = read_sheet("Notes")
            df = pd.concat([df, pd.DataFrame([{'Note': note}])], ignore_index=True)
            write_sheet(df, "Notes")
            print("Note saved.")
            input("\nPress Enter to continue...")

        elif choice == '3':
            df = read_sheet("Notes")
            if df.empty:
                print("No notes to edit.")
            else:
                print("\nNotes:")
                for idx, row in df.iterrows():
                    print(f"{idx + 1}. {row['Note']}")
                try:
                    line_no = int(input("\nEnter line number to edit: ")) - 1
                    if 0 <= line_no < len(df):
                        new_line = input("Enter new content: ")
                        df.at[line_no, 'Note'] = new_line
                        write_sheet(df, "Notes")
                        print("Note updated.")
                    else:
                        print("Invalid line number.")
                except Exception as e:
                    print("Error:", e)
            input("\nPress Enter to continue...")

        elif choice == '4':
            break
        else:
            print("Invalid choice.")
            time.sleep(1)

def calculator():
    while True:
        clear_screen()
        print("\n--- Calculator ---")
        print("1. Add")
        print("2. Subtract")
        print("3. Multiply")
        print("4. Divide")
        print("5. Square Root")
        print("6. Power")
        print("7. Logarithm")
        print("8. Factorial")
        print("9. Back to Main Menu")
        choice = input("Enter your choice: ")

        if choice == '9':
            break
        try:
            if choice in ['1', '2', '3', '4']:
                a = float(input("Enter first number: "))
                b = float(input("Enter second number: "))
                if choice == '1':
                    print("Result:", a + b)
                elif choice == '2':
                    print("Result:", a - b)
                elif choice == '3':
                    print("Result:", a * b)
                elif choice == '4':
                    if b == 0:
                        print("Cannot divide by zero.")
                    else:
                        print("Result:", a / b)

            elif choice == '5':
                num = float(input("Enter number: "))
                if num < 0:
                    print("Cannot take square root of negative number.")
                else:
                    print("Result:", math.sqrt(num))

            elif choice == '6':
                base = float(input("Enter base: "))
                exp = float(input("Enter exponent: "))
                print("Result:", math.pow(base, exp))

            elif choice == '7':
                num = float(input("Enter number: "))
                if num <= 0:
                    print("Logarithm undefined for zero or negative numbers.")
                else:
                    print("Result:", math.log(num))

            elif choice == '8':
                num = int(input("Enter a non-negative integer: "))
                if num < 0:
                    print("Factorial not defined for negative numbers.")
                else:
                    print("Result:", math.factorial(num))

            else:
                print("Invalid choice.")
        except Exception as e:
            print("Error:", e)
        input("\nPress Enter to continue...")

def unit_converter():
    while True:
        clear_screen()
        print("\n--- Unit Converter ---")
        print("1. Kg to Grams")
        print("2. Meters to Centimeters")
        print("3. Celsius to Fahrenheit")
        print("4. Inches to Centimeters")
        print("5. Liters to Milliliters")
        print("6. Back to Main Menu")
        choice = input("Enter your choice: ")

        if choice == '1':
            kg = float(input("Enter weight in Kg: "))
            print("Result:", kg * 1000, "grams")

        elif choice == '2':
            meters = float(input("Enter length in meters: "))
            print("Result:", meters * 100, "centimeters")

        elif choice == '3':
            celsius = float(input("Enter temperature in Celsius: "))
            print("Result:", (celsius * 9 / 5) + 32, "Fahrenheit")

        elif choice == '4':
            inches = float(input("Enter length in inches: "))
            print("Result:", inches * 2.54, "centimeters")

        elif choice == '5':
            liters = float(input("Enter volume in liters: "))
            print("Result:", liters * 1000, "milliliters")

        elif choice == '6':
            break
        else:
            print("Invalid choice.")
        input("\nPress Enter to continue...")

def password_manager():
    while True:
        clear_screen()
        print("\n--- Password Manager ---")
        print("1. View Passwords")
        print("2. Add Password")
        print("3. Back to Main Menu")
        choice = input("Enter your choice: ")

        if choice == '1':
            df = read_sheet("Passwords")
            if df.empty:
                print("No passwords saved.")
            else:
                print("\nStored Passwords:\n")
                for idx, row in df.iterrows():
                    print(f"Website: {row['Website']}, Username: {row['Username']}, Password: {row['Password']}")
            input("\nPress Enter to continue...")

        elif choice == '2':
            website = input("Website: ")
            username = input("Username: ")
            password = input("Password: ")
            df = read_sheet("Passwords")
            df = pd.concat([df, pd.DataFrame([{
                'Website': website,
                'Username': username,
                'Password': password
            }])], ignore_index=True)
            write_sheet(df, "Passwords")
            print("Password saved.")
            input("\nPress Enter to continue...")

        elif choice == '3':
            break
        else:
            print("Invalid choice.")
            time.sleep(1)

def data_plotter():
    clear_screen()
    print("\n--- Data Plotter (Daily Routine) ---")
    tasks = []
    hours = []
    while True:
        task = input("Enter task name (or type 'done' to finish): ")
        if task.lower() == 'done':
            break
        try:
            hour = float(input(f"Enter hours spent on '{task}': "))
            tasks.append(task)
            hours.append(hour)
        except ValueError:
            print("Invalid input. Please enter a number for hours.")

    if not tasks:
        print("No data entered.")
        input("\nPress Enter to continue...")
        return

    data = {'Tasks': tasks, 'Hours': hours}
    df = pd.DataFrame(data)
    write_sheet(df, "DailyRoutine")
    plt.figure(figsize=(6, 6))
    plt.pie(df['Hours'], labels=df['Tasks'], autopct='%1.1f%%', startangle=90)
    plt.title('Custom Daily Routine')
    plt.axis('equal')
    plt.tight_layout()
    image_file = 'daily_routine_pie_chart.png'
    plt.savefig(image_file)
    print(f"\nPie chart saved as '{image_file}'")
    plt.show()
    input("\nPress Enter to continue...")

def todo_manager():
    while True:
        clear_screen()
        print("\n--- To-Do List Manager ---")
        print("1. View To-Do List")
        print("2. Add To-Do Item")
        print("3. Remove To-Do Item")
        print("4. Back to Main Menu")
        choice = input("Enter your choice: ")

        if choice == '1':
            df = read_sheet("Todos")
            if df.empty:
                print("To-Do list is empty.")
            else:
                print("\nYour To-Do List:")
                for idx, row in df.iterrows():
                    print(f"{idx + 1}. {row['Task']}")
            input("\nPress Enter to continue...")

        elif choice == '2':
            task = input("Enter a new task: ")
            df = read_sheet("Todos")
            df = pd.concat([df, pd.DataFrame([{'Task': task}])], ignore_index=True)
            write_sheet(df, "Todos")
            print("Task added.")
            input("\nPress Enter to continue...")

        elif choice == '3':
            df = read_sheet("Todos")
            if df.empty:
                print("To-Do list is empty.")
            else:
                print("\nTo-Do List:")
                for idx, row in df.iterrows():
                    print(f"{idx + 1}. {row['Task']}")
                try:
                    remove_idx = int(input("Enter task number to remove: ")) - 1
                    if 0 <= remove_idx < len(df):
                        removed_task = df.at[remove_idx, 'Task']
                        df = df.drop(remove_idx).reset_index(drop=True)
                        write_sheet(df, "Todos")
                        print(f"Removed task: {removed_task}")
                    else:
                        print("Invalid task number.")
                except Exception as e:
                    print("Error:", e)
            input("\nPress Enter to continue...")

        elif choice == '4':
            break
        else:
            print("Invalid choice.")
            time.sleep(1)

def about_section():
    clear_screen()
    print("\n--- About This Personal Assistant ---")
    print("Created by: Your Name")
    print("Version: 1.0")
    print("This assistant helps with notes, to-do lists, password management, calculations, unit conversions, and data plotting.")
    input("\nPress Enter to continue...")

def main():
    while True:
        clear_screen()
        print("=== Personal Assistant ===")
        print("1. Show Date and Time")
        print("2. Notes Section")
        print("3. Advanced Calculator")
        print("4. Unit Converter")
        print("5. Password Manager")
        print("6. Data Plotter (Daily Routine)")
        print("7. To-Do List Manager")
        print("8. About")
        print("9. Exit")

        choice = input("Enter your choice: ")
        if choice == '1':
            show_datetime()
        elif choice == '2':
            notes_section()
        elif choice == '3':
            calculator()
        elif choice == '4':
            unit_converter()
        elif choice == '5':
            password_manager()
        elif choice == '6':
            data_plotter()
        elif choice == '7':
            todo_manager()
        elif choice == '8':
            about_section()
        elif choice == '9':
            print("Goodbye!")
            time.sleep(1)
            break
        else:
            print("Invalid choice.")
            time.sleep(1)

if __name__ == "__main__":
    main()'''

