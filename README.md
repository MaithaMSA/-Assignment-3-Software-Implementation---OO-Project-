# -Assignment-3-Software-Implementation---OO-Project-

#Company_system.py file
# Import Tkinter module from python
import tkinter as tk
from tkinter import messagebox
import re
# Importing the pickle module for serializing and deserializing data.
import pickle
# Importing the classes from other files
from employee import Employee
from venue import Venue
from event import Event
from client import Client
from guest import Guest
from supplier import Supplier
# Import data storage module 
from data_storage import write_data, read_data
# Import prettytable module
from prettytable import PrettyTable


# Defining a class named 'CompanySystem' to represent the event management system.
class CompanySystem:
    def __init__(self):
        self.root = tk.Tk()
        self.root.title("Event Management System")
        self.root.geometry("475x400")  # Increased window size
        self.root.config(bg="#FFFDD0")  # Set background color

        # Create a frame for sign-up and sign-in forms
        self.frame_signup = tk.Frame(self.root, bg="#FFFDD0")
        self.frame_signin = tk.Frame(self.root, bg="#FFFDD0")
        self.frame_main_menu = tk.Frame(self.root, bg="#FFFDD0")

        # Initialize variables to store user inputs
        self.username_var = tk.StringVar()
        self.email_var = tk.StringVar()
        self.password_var = tk.StringVar()

        # Sign-Up Page
        self.create_signup_page()

        # Sign-In Page
        self.create_signin_page()

        # Main Menu Page
        self.create_main_menu_page()

        # Initially show the sign-up page
        self.show_signup_page()

        # Initialize user data dictionary
        self.user_data = {}

    # Method to create the sign-up page.
    def create_signup_page(self):
        label_signup = tk.Label(self.frame_signup, text="Sign Up", font=("Helvetica", 20), bg="#FFFDD0")
        label_signup.pack(pady=(70, 20))  # Added pady to create space
        label_signup.grid(row=0, columnspan=2)

        # Creating labels and entry fields for username, email, and password inputs.
        label_username = tk.Label(self.frame_signup, text="Username:", font=("Helvetica", 12), bg="#FFFDD0")
        label_username.grid(row=1, column=0, sticky="e")
        entry_username = tk.Entry(self.frame_signup, textvariable=self.username_var, font=("Helvetica", 12))
        entry_username.grid(row=1, column=1, pady=5)

        label_email = tk.Label(self.frame_signup, text="Email:", font=("Helvetica", 12), bg="#FFFDD0")
        label_email.grid(row=2, column=0, sticky="e")
        entry_email = tk.Entry(self.frame_signup, textvariable=self.email_var, font=("Helvetica", 12))
        entry_email.grid(row=2, column=1, pady=5)

        label_password = tk.Label(self.frame_signup, text="Password:", font=("Helvetica", 12), bg="#FFFDD0")
        label_password.grid(row=3, column=0, sticky="e")
        entry_password = tk.Entry(self.frame_signup, textvariable=self.password_var, show="*", font=("Helvetica", 12))
        entry_password.grid(row=3, column=1, pady=5)

        # Creating sign-up button and sign-in link.
        button_signup = tk.Button(self.frame_signup, text="Sign Up", command=self.sign_up, bg="#4CAF50", fg="white",
                                  font=("Helvetica", 12))
        button_signup.grid(row=4, columnspan=2, pady=(20, 10))  # Added pady to create space

        button_signin = tk.Button(self.frame_signup, text="Already have an account? Sign In",
                                  command=self.show_signin_page, bg="#3f3f3f", fg="white", font=("Helvetica", 10))
        button_signin.grid(row=5, columnspan=2)

    # Method to create the sign-in page.
    def create_signin_page(self):

        # Creating labels and entry fields for username and password inputs.
        label_signin = tk.Label(self.frame_signin, text="Sign In", font=("Helvetica", 20), bg="#FFFDD0")
        label_signin.pack(pady=(50, 20))  # Added pady to create space
        label_signin.grid(row=0, columnspan=2)

        label_username = tk.Label(self.frame_signin, text="Username:", font=("Helvetica", 12), bg="#FFFDD0")
        label_username.grid(row=1, column=0, sticky="e")
        entry_username = tk.Entry(self.frame_signin, textvariable=self.username_var, font=("Helvetica", 12))
        entry_username.grid(row=1, column=1, pady=5)

        label_password = tk.Label(self.frame_signin, text="Password:", font=("Helvetica", 12), bg="#FFFDD0")
        label_password.grid(row=2, column=0, sticky="e")
        entry_password = tk.Entry(self.frame_signin, textvariable=self.password_var, show="*", font=("Helvetica", 12))
        entry_password.grid(row=2, column=1, pady=5)

        # Creating sign-in button and sign-up link.
        button_signin = tk.Button(self.frame_signin, text="Sign In", command=self.sign_in, bg="#4CAF50", fg="white",
                                  font=("Helvetica", 12))
        button_signin.grid(row=3, columnspan=2, pady=(20, 10))  # Added pady to create space

        button_signup = tk.Button(self.frame_signin, text="Don't have an account? Sign Up",
                                  command=self.show_signup_page, bg="#3f3f3f", fg="white", font=("Helvetica", 10))
        button_signup.grid(row=4, columnspan=2)

    # Method to create the main-menu page.
    def create_main_menu_page(self):
        # Button colors
        button_color = "#FFA500"  # Light orange

        label_main_menu = tk.Label(self.frame_main_menu, text="Main Menu", font=("Helvetica", 20), bg="#FFFDD0")
        label_main_menu.pack(pady=(70, 20))  # Added pady to create space
        label_main_menu.grid(row=0, column=0, pady=10, padx=(70, 20), columnspan=2, sticky="ew")

        # Employee Button
        button_employee = tk.Button(self.frame_main_menu, text="Employee", command=self.show_employee_page,
                                    bg=button_color, fg="white", font=("Helvetica", 12), width=15)
        button_employee.grid(row=1, column=0, pady=10, padx=(70, 20), sticky="ew")

        # Client Button
        button_client = tk.Button(self.frame_main_menu, text="Client", command=self.show_client_page, bg=button_color,
                                  fg="white", font=("Helvetica", 12), width=15)
        button_client.grid(row=2, column=0, pady=10, padx=(70, 20), sticky="ew")

        # Guest Button
        button_guest = tk.Button(self.frame_main_menu, text="Guest", command=self.show_guest_page, bg=button_color,
                                 fg="white", font=("Helvetica", 12), width=15)
        button_guest.grid(row=3, column=0, pady=10, padx=(70, 20), sticky="ew")

        # Venue Button
        button_venue = tk.Button(self.frame_main_menu, text="Venue", command=self.show_venue_page, bg=button_color,
                                 fg="white", font=("Helvetica", 12), width=15)
        button_venue.grid(row=4, column=0, pady=10, padx=(70, 20), sticky="ew")

        # Supplier Button
        button_supplier = tk.Button(self.frame_main_menu, text="Supplier", command=self.show_supplier_page,
                                    bg=button_color, fg="white", font=("Helvetica", 12), width=15)
        button_supplier.grid(row=5, column=0, pady=10, padx=(70, 20), sticky="ew")

        # Event Button
        button_event = tk.Button(self.frame_main_menu, text="Event", command=self.show_event_page, bg=button_color,
                                 fg="white", font=("Helvetica", 12), width=15)
        button_event.grid(row=6, column=0, pady=10, padx=(70, 20), sticky="ew")

        # Total Summary Button
        button_total_summary = tk.Button(self.frame_main_menu, text="Total Summary", command=self.show_total_summary,
                                         bg=button_color, fg="white", font=("Helvetica", 12), width=15)
        button_total_summary.grid(row=7, column=0, pady=10, padx=(70, 20), sticky="ew")

        # Centering buttons horizontally
        self.frame_main_menu.grid_columnconfigure(0, weight=1)

    # Method to show the sign-up page.
    def show_signup_page(self):
        self.frame_signin.grid_remove()
        self.frame_signup.grid(row=0, column=0, padx=100, pady=50, sticky="nsew")
        self.frame_signup.grid_columnconfigure(0, weight=1)
        self.frame_signup.grid_columnconfigure(1, weight=1)

    # Method to show the sign-in page.
    def show_signin_page(self):
        self.frame_signup.grid_remove()
        self.frame_signin.grid(row=0, column=0, padx=100, pady=50, sticky="nsew")
        self.frame_signin.grid_columnconfigure(0, weight=1)
        self.frame_signin.grid_columnconfigure(1, weight=1)

    # Method to show the main_menu page.
    def show_main_menu(self):
        self.frame_signup.grid_remove()
        self.frame_signin.grid_remove()
        self.frame_main_menu.grid(row=0, column=0, padx=100, pady=50, sticky="nsew")
        self.frame_main_menu.grid_columnconfigure(0, weight=1)
        self.frame_main_menu.grid_columnconfigure(1, weight=1)
        self.root.geometry("480x500")

    # Method to show total summary.
    def show_total_summary(self):
        # Create a Toplevel window for Total Summary
        total_summary_window = tk.Toplevel(self.root)
        total_summary_window.title("Total Summary")
        total_summary_window.geometry("400x400")
        total_summary_window.config(bg="#FFFDD0")

        # Total Employee Button
        button_total_employee = tk.Button(total_summary_window, text="Total Employee", command=self.show_total_employee,
                                          bg="#FFA500", fg="white", font=("Helvetica", 12), width=15)
        button_total_employee.pack(pady=10)

        # Total Supplier Button
        button_total_supplier = tk.Button(total_summary_window, text="Total Supplier", command=self.show_total_supplier,
                                          bg="#FFA500", fg="white", font=("Helvetica", 12), width=15)
        button_total_supplier.pack(pady=10)

        # Total Client Button
        button_total_client = tk.Button(total_summary_window, text="Total Client", command=self.show_total_client,
                                        bg="#FFA500", fg="white", font=("Helvetica", 12), width=15)
        button_total_client.pack(pady=10)

        # Total Event Button
        button_total_event = tk.Button(total_summary_window, text="Total Event", command=self.show_total_event,
                                       bg="#FFA500", fg="white", font=("Helvetica", 12), width=15)
        button_total_event.pack(pady=10)

        # Total Guest Button
        button_total_guest = tk.Button(total_summary_window, text="Total Guest", command=self.show_total_guest,
                                       bg="#FFA500", fg="white", font=("Helvetica", 12), width=15)
        button_total_guest.pack(pady=10)

        # Total Venue Button
        button_total_venue = tk.Button(total_summary_window, text="Total Venue", command=self.show_total_venue,
                                       bg="#FFA500", fg="white", font=("Helvetica", 12), width=15)
        button_total_venue.pack(pady=10)

        # Payments Pending and Revenue Button
        button_payments_revenue = tk.Button(total_summary_window, text="Payments Pending and Revenue",
                                            command=self.show_payments_revenue, bg="#FFA500", fg="white",
                                            font=("Helvetica", 12), width=25)
        button_payments_revenue.pack(pady=10)

    # Method to show total employee.
    def show_total_employee(self):
        # Read data from pickle file
        with open('employees.pkl', 'rb') as file:
            employee_data = pickle.load(file)

        total_count = len(employee_data)

        # Display total count
        messagebox.showinfo("Total Employee Count", f"Total Employee Count: {total_count}")

    # Method to show total supplier.
    def show_total_supplier(self):
        # Read data from pickle file
        with open('suppliers.pkl', 'rb') as file:
            supplier_data = pickle.load(file)

        total_count = len(supplier_data)

        # Display total count
        messagebox.showinfo("Total Supplier Count", f"Total Supplier Count: {total_count}")

    def show_total_client(self):
        # Read data from pickle file
        with open('clients.pkl', 'rb') as file:
            client_data = pickle.load(file)

        total_count = len(client_data)

        # Display total count
        messagebox.showinfo("Total Client Count", f"Total Client Count: {total_count}")

    def show_total_event(self):
        # Read data from pickle file
        with open('events.pkl', 'rb') as file:
            event_data = pickle.load(file)

        total_count = len(event_data)

        # Display total count
        messagebox.showinfo("Total Event Count", f"Total Event Count: {total_count}")

    def show_total_guest(self):
        # Read data from pickle file
        with open('guests.pkl', 'rb') as file:
            guest_data = pickle.load(file)

        total_count = len(guest_data)

        # Display total count
        messagebox.showinfo("Total Guest Count", f"Total Guest Count: {total_count}")

    def show_total_venue(self):
        # Read data from pickle file
        with open('venues.pkl', 'rb') as file:
            venue_data = pickle.load(file)

        total_count = len(venue_data)

        # Display total count
        messagebox.showinfo("Total Venue Count", f"Total Venue Count: {total_count}")

    def show_payments_revenue(self):
        # Read data from pickle file
        with open('temp_payments_revenue_data.pkl', 'rb') as file:
            payments_revenue_data = pickle.load(file)

        # Create a PrettyTable to organize the data
        table = PrettyTable()
        table.field_names = ["Payment ID", "Amount", "Date", "Type", "Details"]

        # Initialize total revenue
        total_revenue = 0

        # Add data to the table and calculate total revenue
        for payment in payments_revenue_data:
            table.add_row([
                payment["payment_id"],
                payment["amount"],
                payment["date"],
                payment["type"],
                payment.get("Venue", "") or payment.get("Event", "")
            ])
            # Add amount to total revenue
            total_revenue += payment["amount"]

        # Add a row for total revenue to the table
        table.add_row(["", "", "", "", ""])
        table.add_row(["Total Revenue", total_revenue, "", "", ""])

        # Display the table using a messagebox
        messagebox.showinfo("Payments and Revenue", str(table))

    def sign_up(self):
        username = self.username_var.get()
        email = self.email_var.get()
        password = self.password_var.get()

        # Email validation using regular expression
        if not re.match(r"[^@]+@[^@]+\.[^@]+", email):
            messagebox.showerror("Error", "Invalid email address.")
            return

        # Check if username already exists
        if username in self.user_data:
            messagebox.showerror("Error", "Username already exists. Please choose a different one.")
        else:
            # Add new user to the dictionary
            self.user_data[username] = {"email": email, "password": password}
            # Write updated user data back to file
            write_data("users.pkl", self.user_data)
            messagebox.showinfo("Success", "Sign up successful!")

    def sign_in(self):
        username = self.username_var.get()  # Retrieve username from entry widget
        password = self.password_var.get()  # Retrieve password from entry widget

        # Load existing user data from file
        try:
            existing_users = read_data("users.pkl")
        except FileNotFoundError:
            messagebox.showerror("Error", "No users found. Please sign up first.")
            return

        # Check if any field is empty
        if not username or not password:
            messagebox.showerror("Error", "Please fill in all fields.")
            return

        # Check if username exists
        if username in existing_users:
            # Check if password matches
            if existing_users[username]["password"] == password:
                messagebox.showinfo("Success", "Sign in successful!")
                self.show_main_menu()  # Show the main menu after successful sign-in
            else:
                messagebox.showerror("Error", "Incorrect password.")
        else:
            messagebox.showerror("Error", "Username not found. Please sign up first.")

    def show_employee_page(self):
        self.employee_window = tk.Toplevel(self.root)
        self.employee_window.title("Employee Menu")
        self.employee_window.geometry("480x445")
        self.employee_window.config(bg="#FFFDD0")

        label_employee_menu = tk.Label(self.employee_window, text="Employee Menu", font=("Helvetica", 20), bg="#FFFDD0")
        label_employee_menu.pack(pady=20)

        button_add_employee = tk.Button(self.employee_window, text="Add Employee",
                                        command=self.add_employee_placeholder, bg="#FFA500", fg="white",
                                        font=("Helvetica", 12), width=20)
        button_add_employee.pack(pady=10)

        button_edit_employee = tk.Button(self.employee_window, text="Edit Employee",
                                         command=self.edit_employee_placeholder, bg="#FFA500", fg="white",
                                         font=("Helvetica", 12), width=20)
        button_edit_employee.pack(pady=10)

        button_delete_employee = tk.Button(self.employee_window, text="Delete Employee",
                                           command=self.delete_employee_placeholder, bg="#FFA500", fg="white",
                                           font=("Helvetica", 12), width=20)
        button_delete_employee.pack(pady=10)

        button_display_employee = tk.Button(self.employee_window, text="Display Employee",
                                            command=self.display_employee_placeholder, bg="#FFA500", fg="white",
                                            font=("Helvetica", 12), width=20)
        button_display_employee.pack(pady=10)

    def add_employee_placeholder(self):
        # Create a Toplevel window for adding an employee
        add_employee_window = tk.Toplevel(self.employee_window)
        add_employee_window.title("Add Employee")
        add_employee_window.geometry("400x700")
        add_employee_window.config(bg="#FFFDD0")

        # Labels and Entry fields for employee details
        tk.Label(add_employee_window, text="Name:", font=("Helvetica", 12), bg="#FFFDD0").pack(pady=(20, 5))
        entry_name = tk.Entry(add_employee_window, font=("Helvetica", 12))
        entry_name.pack(pady=5)

        tk.Label(add_employee_window, text="Employee ID:", font=("Helvetica", 12), bg="#FFFDD0").pack(pady=5)
        entry_employee_id = tk.Entry(add_employee_window, font=("Helvetica", 12))
        entry_employee_id.pack(pady=5)

        tk.Label(add_employee_window, text="Department:", font=("Helvetica", 12), bg="#FFFDD0").pack(pady=5)
        entry_department = tk.Entry(add_employee_window, font=("Helvetica", 12))
        entry_department.pack(pady=5)

        tk.Label(add_employee_window, text="Job Title:", font=("Helvetica", 12), bg="#FFFDD0").pack(pady=5)
        entry_job_title = tk.Entry(add_employee_window, font=("Helvetica", 12))
        entry_job_title.pack(pady=5)

        tk.Label(add_employee_window, text="Basic Salary:", font=("Helvetica", 12), bg="#FFFDD0").pack(pady=5)
        entry_basic_salary = tk.Entry(add_employee_window, font=("Helvetica", 12))
        entry_basic_salary.pack(pady=5)

        tk.Label(add_employee_window, text="Age:", font=("Helvetica", 12), bg="#FFFDD0").pack(pady=5)
        entry_age = tk.Entry(add_employee_window, font=("Helvetica", 12))
        entry_age.pack(pady=5)

        tk.Label(add_employee_window, text="Date of Birth (DOB):", font=("Helvetica", 12), bg="#FFFDD0").pack(pady=5)
        entry_dob = tk.Entry(add_employee_window, font=("Helvetica", 12))
        entry_dob.pack(pady=5)

        tk.Label(add_employee_window, text="Passport Details:", font=("Helvetica", 12), bg="#FFFDD0").pack(pady=5)
        entry_passport_details = tk.Entry(add_employee_window, font=("Helvetica", 12))
        entry_passport_details.pack(pady=5)

        tk.Label(add_employee_window, text="Manager ID:", font=("Helvetica", 12), bg="#FFFDD0").pack(pady=5)
        entry_manager_id = tk.Entry(add_employee_window, font=("Helvetica", 12))
        entry_manager_id.pack(pady=5)

        # Function to add employee details
        def add_employee_details():
            # Get values from entry fields
            name = entry_name.get()
            employee_id = entry_employee_id.get()
            department = entry_department.get()
            job_title = entry_job_title.get()
            basic_salary = entry_basic_salary.get()
            age = entry_age.get()
            dob = entry_dob.get()
            passport_details = entry_passport_details.get()
            manager_id = entry_manager_id.get()

            if not employee_id.isdigit():
                messagebox.showerror("Error", "Employee ID must be number digit.")
                return

            # Validate name (must not be empty and contain only letters and spaces)
            if not name.replace(" ", "").isalpha():
                messagebox.showerror("Error", "Name must not contain digit it must contain string.")
                return
            # Create an instance of the Employee class with the provided details
            employee = Employee(name, employee_id, department, job_title, basic_salary, age, dob, passport_details,
                                manager_id)

            # Add employee details to the database
            employee.add_to_database()

            # Show success message
            messagebox.showinfo("Success", "Employee added successfully!")
            add_employee_window.destroy()

        # Button to add employee details
        tk.Button(add_employee_window, text="Add Employee", command=add_employee_details, bg="#4CAF50", fg="white",
                  font=("Helvetica", 12)).pack(pady=20)

    def edit_employee_placeholder(self):
        # Read existing employee data
        try:
            existing_data = read_data("employees.pkl")
        except FileNotFoundError:
            messagebox.showerror("Error", "No employee data found.")
            return

        # Create a Toplevel window for selecting an employee
        select_employee_window = tk.Toplevel(self.employee_window)
        select_employee_window.title("Select Employee")
        select_employee_window.geometry("400x400")
        select_employee_window.config(bg="#FFFDD0")

        # Function to handle the employee selection and open the Edit Employee Window
        def open_edit_window():
            selected_entry = listbox.curselection()
            if selected_entry:
                selected_employee_entry = listbox.get(selected_entry[0])  # Get the selected employee entry
                selected_employee_id = selected_employee_entry.split(" - ")[-1]  # Extract the employee ID
                select_employee_window.destroy()  # Close the Select Employee Window
                open_edit_employee_window(
                    existing_data[selected_employee_id])  # Open the Edit Employee Window with selected employee data

        # Display a list of employee names and IDs
        listbox = tk.Listbox(select_employee_window, width=50, height=10)
        for employee_id, employee_details in existing_data.items():
            listbox.insert(tk.END, f"{employee_details['name']} - {employee_id}")
        listbox.pack(pady=20)

        # Button to select an employee and open the Edit Employee Window
        select_button = tk.Button(select_employee_window, text="Select Employee", command=open_edit_window,
                                  bg="#4CAF50", fg="white", font=("Helvetica", 12))
        select_button.pack(pady=10)

        def open_edit_employee_window(employee_details):
            # Create a Toplevel window for editing an employee
            edit_employee_window = tk.Toplevel(self.employee_window)
            edit_employee_window.title("Edit Employee")
            edit_employee_window.geometry("400x900")
            edit_employee_window.config(bg="#FFFDD0")

            # Function to update employee details
            def update_employee_details():
                # Get updated values from entry fields
                updated_employee = Employee(
                    name=entry_name.get(),
                    employee_id=entry_employee_id.get(),
                    department=entry_department.get(),
                    job_title=entry_job_title.get(),
                    basic_salary=entry_basic_salary.get(),
                    age=entry_age.get(),
                    dob=entry_dob.get(),
                    passport_details=entry_passport_details.get(),
                    manager_id=entry_manager_id.get()
                )

                # Update the employee data in the database
                existing_data[updated_employee.employee_id] = {
                    "name": updated_employee.name,
                    "employee_id": updated_employee.employee_id,
                    "department": updated_employee.department,
                    "job_title": updated_employee.job_title,
                    "basic_salary": updated_employee.basic_salary,
                    "age": updated_employee.age,
                    "dob": updated_employee.dob,
                    "passport_details": updated_employee.passport_details,
                    "manager_id": updated_employee.manager_id
                }

                # Write the updated data back to the file
                write_data("employees.pkl", existing_data)

                # Show success message
                messagebox.showinfo("Success", "Employee details updated successfully!")
                edit_employee_window.destroy()

            # Labels and Entry fields for employee details
            tk.Label(edit_employee_window, text="Name:", font=("Helvetica", 12), bg="#FFFDD0").pack(pady=5)
            entry_name = tk.Entry(edit_employee_window, font=("Helvetica", 12))
            entry_name.insert(0, employee_details["name"])
            entry_name.pack(pady=5)

            tk.Label(edit_employee_window, text="Employee ID:", font=("Helvetica", 12), bg="#FFFDD0").pack(pady=5)
            entry_employee_id = tk.Entry(edit_employee_window, font=("Helvetica", 12))
            entry_employee_id.insert(0, employee_details["employee_id"])
            entry_employee_id.pack(pady=5)

            tk.Label(edit_employee_window, text="Department:", font=("Helvetica", 12), bg="#FFFDD0").pack(pady=5)
            entry_department = tk.Entry(edit_employee_window, font=("Helvetica", 12))
            entry_department.insert(0, employee_details["department"])
            entry_department.pack(pady=5)

            tk.Label(edit_employee_window, text="Job Title:", font=("Helvetica", 12), bg="#FFFDD0").pack(pady=5)
            entry_job_title = tk.Entry(edit_employee_window, font=("Helvetica", 12))
            entry_job_title.insert(0, employee_details["job_title"])
            entry_job_title.pack(pady=5)

            tk.Label(edit_employee_window, text="Basic Salary:", font=("Helvetica", 12), bg="#FFFDD0").pack(pady=5)
            entry_basic_salary = tk.Entry(edit_employee_window, font=("Helvetica", 12))
            entry_basic_salary.insert(0, employee_details["basic_salary"])
            entry_basic_salary.pack(pady=5)

            tk.Label(edit_employee_window, text="Age:", font=("Helvetica", 12), bg="#FFFDD0").pack(pady=5)
            entry_age = tk.Entry(edit_employee_window, font=("Helvetica", 12))
            entry_age.insert(0, employee_details["age"])
            entry_age.pack(pady=5)

            tk.Label(edit_employee_window, text="Date of Birth (DOB):", font=("Helvetica", 12), bg="#FFFDD0").pack(
                pady=5)
            entry_dob = tk.Entry(edit_employee_window, font=("Helvetica", 12))
            entry_dob.insert(0, employee_details["dob"])
            entry_dob.pack(pady=5)

            tk.Label(edit_employee_window, text="Passport Details:", font=("Helvetica", 12), bg="#FFFDD0").pack(pady=5)
            entry_passport_details = tk.Entry(edit_employee_window, font=("Helvetica", 12))
            entry_passport_details.insert(0, employee_details["passport_details"])
            entry_passport_details.pack(pady=5)

            tk.Label(edit_employee_window, text="Manager ID:", font=("Helvetica", 12), bg="#FFFDD0").pack(pady=5)
            entry_manager_id = tk.Entry(edit_employee_window, font=("Helvetica", 12))
            entry_manager_id.insert(0, employee_details["manager_id"])
            entry_manager_id.pack(pady=5)

            # Button to update employee details
            update_button = tk.Button(edit_employee_window, text="Update Employee", command=update_employee_details,
                                      bg="#4CAF50", fg="white", font=("Helvetica", 12))
            update_button.pack(pady=20)

    def delete_employee_placeholder(self):
        # Read existing employee data
        existing_data = Employee.read_employee_data()

        # Create a Toplevel window for deleting an employee
        delete_employee_window = tk.Toplevel(self.employee_window)
        delete_employee_window.title("Delete Employee")
        delete_employee_window.geometry("400x400")
        delete_employee_window.config(bg="#FFFDD0")

        # Function to handle the employee selection and deletion
        def delete_employee():
            selected_entry = listbox.curselection()
            if selected_entry:
                selected_employee_entry = listbox.get(selected_entry[0])  # Get the selected employee entry
                selected_employee_id = selected_employee_entry.split(" - ")[-1]  # Extract the employee ID
                confirm_delete = messagebox.askyesno("Confirmation", "Are you sure you want to delete this employee?")
                if confirm_delete:
                    del existing_data[selected_employee_id]  # Delete the selected employee
                    Employee.write_employee_data(existing_data)  # Write the updated data back to the file
                    messagebox.showinfo("Success", "Employee deleted successfully!")
                    delete_employee_window.destroy()

        # Display a list of employee names and IDs
        listbox = tk.Listbox(delete_employee_window, width=50, height=10)
        for employee_id, employee_details in existing_data.items():
            listbox.insert(tk.END, f"{employee_details['name']} - {employee_id}")
        listbox.pack(pady=20)

        # Button to delete the selected employee
        delete_button = tk.Button(delete_employee_window, text="Delete Employee", command=delete_employee, bg="#4CAF50",
                                  fg="white", font=("Helvetica", 12))
        delete_button.pack(pady=10)

    def display_employee_placeholder(self):
        # Read existing employee data
        existing_data = Employee.read_employee_data()

        # Create a Toplevel window for displaying employee details
        display_employee_window = tk.Toplevel(self.employee_window)
        display_employee_window.title("Display Employee Details")
        display_employee_window.geometry("400x650")
        display_employee_window.config(bg="#FFFDD0")

        # Function to handle the employee selection and display details
        def display_employee_details():
            selected_entry = listbox.curselection()
            if selected_entry:
                selected_employee_entry = listbox.get(selected_entry[0])  # Get the selected employee entry
                selected_employee_id = selected_employee_entry.split(" - ")[-1]  # Extract the employee ID
                employee_details = existing_data[selected_employee_id]  # Get details of the selected employee

                # Create labels to display employee details
                tk.Label(display_employee_window, text=f"Name: {employee_details['name']}", font=("Helvetica", 12),
                         bg="#FFFDD0").pack(pady=5)
                tk.Label(display_employee_window, text=f"Employee ID: {employee_details['employee_id']}",
                         font=("Helvetica", 12), bg="#FFFDD0").pack(pady=5)
                tk.Label(display_employee_window, text=f"Department: {employee_details['department']}",
                         font=("Helvetica", 12), bg="#FFFDD0").pack(pady=5)
                tk.Label(display_employee_window, text=f"Job Title: {employee_details['job_title']}",
                         font=("Helvetica", 12), bg="#FFFDD0").pack(pady=5)
                tk.Label(display_employee_window, text=f"Basic Salary: {employee_details['basic_salary']}",
                         font=("Helvetica", 12), bg="#FFFDD0").pack(pady=5)
                tk.Label(display_employee_window, text=f"Age: {employee_details['age']}", font=("Helvetica", 12),
                         bg="#FFFDD0").pack(pady=5)
                tk.Label(display_employee_window, text=f"Date of Birth (DOB): {employee_details['dob']}",
                         font=("Helvetica", 12), bg="#FFFDD0").pack(pady=5)
                tk.Label(display_employee_window, text=f"Passport Details: {employee_details['passport_details']}",
                         font=("Helvetica", 12), bg="#FFFDD0").pack(pady=5)
                tk.Label(display_employee_window, text=f"Manager ID: {employee_details['manager_id']}",
                         font=("Helvetica", 12), bg="#FFFDD0").pack(pady=5)

        # Display a list of employee names and IDs
        listbox = tk.Listbox(display_employee_window, width=50, height=10)
        for employee_id, employee_details in existing_data.items():
            listbox.insert(tk.END, f"{employee_details['name']} - {employee_id}")
        listbox.pack(pady=20)

        # Button to display employee details
        display_button = tk.Button(display_employee_window, text="Display Employee Details",
                                   command=display_employee_details, bg="#4CAF50", fg="white", font=("Helvetica", 12))
        display_button.pack(pady=10)

    def show_event_page(self):
        self.event_window = tk.Toplevel(self.root)
        self.event_window.title("Event Menu")
        self.event_window.geometry("480x445")
        self.event_window.config(bg="#FFFDD0")

        label_event_menu = tk.Label(self.event_window, text="Event Menu", font=("Helvetica", 20), bg="#FFFDD0")
        label_event_menu.pack(pady=20)

        button_add_event = tk.Button(self.event_window, text="Add Event", command=self.add_event_placeholder,
                                     bg="#FFA500", fg="white", font=("Helvetica", 12), width=20)
        button_add_event.pack(pady=10)

        button_edit_event = tk.Button(self.event_window, text="Edit Event", command=self.edit_event_placeholder,
                                      bg="#FFA500", fg="white", font=("Helvetica", 12), width=20)
        button_edit_event.pack(pady=10)

        button_delete_event = tk.Button(self.event_window, text="Delete Event", command=self.delete_event_placeholder,
                                        bg="#FFA500", fg="white", font=("Helvetica", 12), width=20)
        button_delete_event.pack(pady=10)

        button_display_event = tk.Button(self.event_window, text="Display Event",
                                         command=self.display_event_placeholder, bg="#FFA500", fg="white",
                                         font=("Helvetica", 12), width=20)
        button_display_event.pack(pady=10)

    def add_event_placeholder(self):
        # Create a Toplevel window for adding an event
        add_event_window = tk.Toplevel(self.event_window)
        add_event_window.title("Add Event")
        add_event_window.geometry("350x450")
        add_event_window.config(bg="#FFFDD0")

        # Labels and Entry fields for event details
        tk.Label(add_event_window, text="Event ID:", font=("Helvetica", 12), bg="#FFFDD0").grid(row=0, column=0,
                                                                                                pady=(20, 5))
        entry_event_id = tk.Entry(add_event_window, font=("Helvetica", 12))
        entry_event_id.grid(row=0, column=1, pady=5)

        tk.Label(add_event_window, text="Event Type:", font=("Helvetica", 12), bg="#FFFDD0").grid(row=1, column=0,
                                                                                                  pady=5)
        entry_event_type = tk.Entry(add_event_window, font=("Helvetica", 12))
        entry_event_type.grid(row=1, column=1, pady=5)

        tk.Label(add_event_window, text="Theme:", font=("Helvetica", 12), bg="#FFFDD0").grid(row=2, column=0, pady=5)
        entry_theme = tk.Entry(add_event_window, font=("Helvetica", 12))
        entry_theme.grid(row=2, column=1, pady=5)

        tk.Label(add_event_window, text="Date:", font=("Helvetica", 12), bg="#FFFDD0").grid(row=3, column=0, pady=5)
        entry_date = tk.Entry(add_event_window, font=("Helvetica", 12))
        entry_date.grid(row=3, column=1, pady=5)

        tk.Label(add_event_window, text="Time:", font=("Helvetica", 12), bg="#FFFDD0").grid(row=4, column=0, pady=5)
        entry_time = tk.Entry(add_event_window, font=("Helvetica", 12))
        entry_time.grid(row=4, column=1, pady=5)

        tk.Label(add_event_window, text="Duration:", font=("Helvetica", 12), bg="#FFFDD0").grid(row=5, column=0, pady=5)
        entry_duration = tk.Entry(add_event_window, font=("Helvetica", 12))
        entry_duration.grid(row=5, column=1, pady=5)

        tk.Label(add_event_window, text="Venue Address:", font=("Helvetica", 12), bg="#FFFDD0").grid(row=6, column=0,
                                                                                                     pady=5)
        entry_venue_address = tk.Entry(add_event_window, font=("Helvetica", 12))
        entry_venue_address.grid(row=6, column=1, pady=5)

        tk.Label(add_event_window, text="Client ID:", font=("Helvetica", 12), bg="#FFFDD0").grid(row=7, column=0,
                                                                                                 pady=5)
        entry_client_id = tk.Entry(add_event_window, font=("Helvetica", 12))
        entry_client_id.grid(row=7, column=1, pady=5)

        tk.Label(add_event_window, text="Guest List:", font=("Helvetica", 12), bg="#FFFDD0").grid(row=8, column=0,
                                                                                                  pady=5)
        entry_guest_list = tk.Entry(add_event_window, font=("Helvetica", 12))
        entry_guest_list.grid(row=8, column=1, pady=5)

        tk.Label(add_event_window, text="Company Supplier:", font=("Helvetica", 12), bg="#FFFDD0").grid(row=9, column=0,
                                                                                                        pady=5)
        entry_company_supplier = tk.Entry(add_event_window, font=("Helvetica", 12))
        entry_company_supplier.grid(row=9, column=1, pady=5)

        tk.Label(add_event_window, text="Invoice:", font=("Helvetica", 12), bg="#FFFDD0").grid(row=10, column=0, pady=5)
        entry_invoice = tk.Entry(add_event_window, font=("Helvetica", 12))
        entry_invoice.grid(row=10, column=1, pady=5)

        # Function to add event details
        def add_event_details():
            # Get values from entry fields
            event_id = entry_event_id.get()
            event_type = entry_event_type.get()
            theme = entry_theme.get()
            date = entry_date.get()
            time = entry_time.get()
            duration = entry_duration.get()
            venue_address = entry_venue_address.get()
            client_id = entry_client_id.get()
            guest_list = entry_guest_list.get()
            company_supplier = entry_company_supplier.get()
            invoice = entry_invoice.get()

            # Create an instance of the Event class with the provided details
            event = Event(event_id, event_type, theme, date, time, duration, venue_address, client_id, guest_list,
                          company_supplier, invoice)

            # Add event details to the database
            event.add_to_database()

            # Show success message
            messagebox.showinfo("Success", "Event added successfully!")
            add_event_window.destroy()

        # Button to add event details
        tk.Button(add_event_window, text="Add Event", command=add_event_details, bg="#4CAF50", fg="white",
                  font=("Helvetica", 12)).grid(row=11, column=0, columnspan=2, pady=20, padx=50)


def edit_event_placeholder(self):
    try:
        existing_data = read_data("events.pkl")
    except FileNotFoundError:
        messagebox.showerror("Error", "No event data found.")
        return

    select_event_window = tk.Toplevel(self.event_window)
    select_event_window.title("Select Event")
    select_event_window.geometry("350x450")
    select_event_window.config(bg="#FFFDD0")

    def open_edit_window():
        selected_entry = listbox.curselection()
        if selected_entry:
            selected_event_entry = listbox.get(selected_entry[0])
            selected_event_id = selected_event_entry.split(" - ")[-1]
            select_event_window.destroy()
            open_edit_event_window(existing_data[selected_event_id])

    listbox = tk.Listbox(select_event_window, width=50, height=10)
    for event_id, event_details in existing_data.items():
        listbox.insert(tk.END, f"{event_details['event_type']} - {event_id}")
    listbox.pack(pady=20)

    select_button = tk.Button(select_event_window, text="Select Event", command=open_edit_window, bg="#4CAF50",
                              fg="white", font=("Helvetica", 12))
    select_button.pack(pady=10)

    def open_edit_event_window(event_details):
        edit_event_window = tk.Toplevel(self.event_window)
        edit_event_window.title("Edit Event")
        edit_event_window.geometry("350x480")
        edit_event_window.config(bg="#FFFDD0")

        def update_event_details():
            updated_event = Event(
                event_id=event_details["event_id"],
                event_type=entry_event_type.get(),
                theme=entry_theme.get(),
                date=entry_date.get(),
                time=entry_time.get(),
                duration=entry_duration.get(),
                venue_address=entry_venue_address.get(),
                client_id=entry_client_id.get(),
                guest_list=entry_guest_list.get(),
                company_supplier=entry_company_supplier.get(),
                invoice=entry_invoice.get()
            )

            existing_data[updated_event.event_id] = {
                "event_id": updated_event.event_id,
                "event_type": updated_event.event_type,
                "theme": updated_event.theme,
                "date": updated_event.date,
                "time": updated_event.time,
                "duration": updated_event.duration,
                "venue_address": updated_event.venue_address,
                "client_id": updated_event.client_id,
                "guest_list": updated_event.guest_list,
                "company_supplier": updated_event.company_supplier,
                "invoice": updated_event.invoice
            }

            write_data("events.pkl", existing_data)

            messagebox.showinfo("Success", "Event details updated successfully!")
            edit_event_window.destroy()

        tk.Label(edit_event_window, text="Event ID:", font=("Helvetica", 12), bg="#FFFDD0").grid(row=0, column=0,
                                                                                                 pady=(20, 5))
        entry_event_id = tk.Entry(edit_event_window, font=("Helvetica", 12))
        entry_event_id.insert(0, event_details["event_id"])
        entry_event_id.grid(row=0, column=1, pady=5)

        tk.Label(edit_event_window, text="Event Type:", font=("Helvetica", 12), bg="#FFFDD0").grid(row=1, column=0,
                                                                                                   pady=(20, 5))
        entry_event_type = tk.Entry(edit_event_window, font=("Helvetica", 12))
        entry_event_type.insert(0, event_details["event_type"])
        entry_event_type.grid(row=1, column=1, pady=5)

        tk.Label(edit_event_window, text="Theme:", font=("Helvetica", 12), bg="#FFFDD0").grid(row=2, column=0, pady=5)
        entry_theme = tk.Entry(edit_event_window, font=("Helvetica", 12))
        entry_theme.insert(0, event_details["theme"])
        entry_theme.grid(row=2, column=1, pady=5)

        tk.Label(edit_event_window, text="Date:", font=("Helvetica", 12), bg="#FFFDD0").grid(row=3, column=0, pady=5)
        entry_date = tk.Entry(edit_event_window, font=("Helvetica", 12))
        entry_date.insert(0, event_details["date"])
        entry_date.grid(row=3, column=1, pady=5)

        tk.Label(edit_event_window, text="Time:", font=("Helvetica", 12), bg="#FFFDD0").grid(row=4, column=0, pady=5)
        entry_time = tk.Entry(edit_event_window, font=("Helvetica", 12))
        entry_time.insert(0, event_details["time"])
        entry_time.grid(row=4, column=1, pady=5)

        tk.Label(edit_event_window, text="Duration:", font=("Helvetica", 12), bg="#FFFDD0").grid(row=5, column=0,
                                                                                                 pady=5)
        entry_duration = tk.Entry(edit_event_window, font=("Helvetica", 12))
        entry_duration.insert(0, event_details["duration"])
        entry_duration.grid(row=5, column=1, pady=5)

        tk.Label(edit_event_window, text="Venue Address:", font=("Helvetica", 12), bg="#FFFDD0").grid(row=6, column=0,
                                                                                                      pady=5)
        entry_venue_address = tk.Entry(edit_event_window, font=("Helvetica", 12))
        entry_venue_address.insert(0, event_details["venue_address"])
        entry_venue_address.grid(row=6, column=1, pady=5)

        tk.Label(edit_event_window, text="Client ID:", font=("Helvetica", 12), bg="#FFFDD0").grid(row=7, column=0,
                                                                                                  pady=5)
        entry_client_id = tk.Entry(edit_event_window, font=("Helvetica", 12))
        entry_client_id.insert(0, event_details["client_id"])
        entry_client_id.grid(row=7, column=1, pady=5)

        tk.Label(edit_event_window, text="Guest List:", font=("Helvetica", 12), bg="#FFFDD0").grid(row=8, column=0,
                                                                                                   pady=5)
        entry_guest_list = tk.Entry(edit_event_window, font=("Helvetica", 12))
        entry_guest_list.insert(0, event_details["guest_list"])
        entry_guest_list.grid(row=8, column=1, pady=5)

        tk.Label(edit_event_window, text="Company Supplier:", font=("Helvetica", 12), bg="#FFFDD0").grid(row=9,
                                                                                                         column=0,
                                                                                                         pady=5)
        entry_company_supplier = tk.Entry(edit_event_window, font=("Helvetica", 12))
        entry_company_supplier.insert(0, event_details["company_supplier"])
        entry_company_supplier.grid(row=9, column=1, pady=5)

        tk.Label(edit_event_window, text="Invoice:", font=("Helvetica", 12), bg="#FFFDD0").grid(row=10, column=0,
                                                                                                pady=5)
        entry_invoice = tk.Entry(edit_event_window, font=("Helvetica", 12))
        entry_invoice.insert(0, event_details["invoice"])
        entry_invoice.grid(row=10, column=1, pady=5)

        update_button = tk.Button(edit_event_window, text="Update Event", command=update_event_details, bg="#4CAF50",
                                  fg="white", font=("Helvetica", 12))
        update_button.grid(row=11, column=0, columnspan=2, pady=20)


def delete_event_placeholder(self):
    existing_data = Event.read_event_data()

    delete_event_window = tk.Toplevel(self.event_window)
    delete_event_window.title("Delete Event")
    delete_event_window.geometry("400x400")
    delete_event_window.config(bg="#FFFDD0")

    def delete_event():
        selected_entry = listbox.curselection()
        if selected_entry:
            selected_event_entry = listbox.get(selected_entry[0])
            selected_event_id = selected_event_entry.split(" - ")[-1]
            confirm_delete = messagebox.askyesno("Confirmation", "Are you sure you want to delete this event?")
            if confirm_delete:
                del existing_data[selected_event_id]
                Event.write_event_data(existing_data)
                messagebox.showinfo("Success", "Event deleted successfully!")
                delete_event_window.destroy()

    listbox = tk.Listbox(delete_event_window, width=50, height=10)
    for event_id, event_details in existing_data.items():
        listbox.insert(tk.END, f"{event_details['event_type']} - {event_id}")
    listbox.pack(pady=20)

    delete_button = tk.Button(delete_event_window, text="Delete Event", command=delete_event, bg="#4CAF50", fg="white",
                              font=("Helvetica", 12))
    delete_button.pack(pady=10)


def display_event_placeholder(self):
    existing_data = Event.read_event_data()

    display_event_window = tk.Toplevel(self.event_window)
    display_event_window.title("Display Event Details")
    display_event_window.geometry("400x650")
    display_event_window.config(bg="#FFFDD0")

    def display_event_details():
        selected_entry = listbox.curselection()
        if selected_entry:
            selected_event_entry = listbox.get(selected_entry[0])
            selected_event_id = selected_event_entry.split(" - ")[-1]
            event_details = existing_data[selected_event_id]

            tk.Label(display_event_window, text=f"Event ID: {event_details['event_id']}", font=("Helvetica", 12),
                     bg="#FFFDD0").pack(pady=5)
            tk.Label(display_event_window, text=f"Event Type: {event_details['event_type']}", font=("Helvetica", 12),
                     bg="#FFFDD0").pack(pady=5)
            tk.Label(display_event_window, text=f"Theme: {event_details['theme']}", font=("Helvetica", 12),
                     bg="#FFFDD0").pack(pady=5)
            tk.Label(display_event_window, text=f"Date: {event_details['date']}", font=("Helvetica", 12),
                     bg="#FFFDD0").pack(pady=5)
            tk.Label(display_event_window, text=f"Time: {event_details['time']}", font=("Helvetica", 12),
                     bg="#FFFDD0").pack(pady=5)
            tk.Label(display_event_window, text=f"Duration: {event_details['duration']}", font=("Helvetica", 12),
                     bg="#FFFDD0").pack(pady=5)
            tk.Label(display_event_window, text=f"Venue Address: {event_details['venue_address']}",
                     font=("Helvetica", 12), bg="#FFFDD0").pack(pady=5)
            tk.Label(display_event_window, text=f"Client ID: {event_details['client_id']}", font=("Helvetica", 12),
                     bg="#FFFDD0").pack(pady=5)
            tk.Label(display_event_window, text=f"Guest List: {event_details['guest_list']}", font=("Helvetica", 12),
                     bg="#FFFDD0").pack(pady=5)
            tk.Label(display_event_window, text=f"Company Supplier: {event_details['company_supplier']}",
                     font=("Helvetica", 12), bg="#FFFDD0").pack(pady=5)
            tk.Label(display_event_window, text=f"Invoice: {event_details['invoice']}", font=("Helvetica", 12),
                     bg="#FFFDD0").pack(pady=5)

    listbox = tk.Listbox(display_event_window, width=50, height=10)
    for event_id, event_details in existing_data.items():
        listbox.insert(tk.END, f"{event_details['event_type']} - {event_id}")
    listbox.pack(pady=20)

    display_button = tk.Button(display_event_window, text="Display Event Details", command=display_event_details,
                               bg="#4CAF50", fg="white", font=("Helvetica", 12))
    display_button.pack(pady=10)


def show_supplier_page(self):
    self.supplier_window = tk.Toplevel(self.root)
    self.supplier_window.title("Supplier Menu")
    self.supplier_window.geometry("480x445")
    self.supplier_window.config(bg="#FFFDD0")

    label_supplier_menu = tk.Label(self.supplier_window, text="Supplier Menu", font=("Helvetica", 20), bg="#FFFDD0")
    label_supplier_menu.pack(pady=20)

    button_add_supplier = tk.Button(self.supplier_window, text="Add Supplier", command=self.add_supplier_placeholder,
                                    bg="#FFA500", fg="white", font=("Helvetica", 12), width=20)
    button_add_supplier.pack(pady=10)

    button_edit_supplier = tk.Button(self.supplier_window, text="Edit Supplier", command=self.edit_supplier_placeholder,
                                     bg="#FFA500", fg="white", font=("Helvetica", 12), width=20)
    button_edit_supplier.pack(pady=10)

    button_delete_supplier = tk.Button(self.supplier_window, text="Delete Supplier",
                                       command=self.delete_supplier_placeholder, bg="#FFA500", fg="white",
                                       font=("Helvetica", 12), width=20)
    button_delete_supplier.pack(pady=10)

    button_display_supplier = tk.Button(self.supplier_window, text="Display Supplier",
                                        command=self.display_supplier_placeholder, bg="#FFA500", fg="white",
                                        font=("Helvetica", 12), width=20)
    button_display_supplier.pack(pady=10)


def add_supplier_placeholder(self):
    # Create a Toplevel window for adding a supplier
    add_supplier_window = tk.Toplevel(self.supplier_window)
    add_supplier_window.title("Add Supplier")
    add_supplier_window.geometry("400x700")
    add_supplier_window.config(bg="#FFFDD0")

    # Labels and Entry fields for supplier details
    tk.Label(add_supplier_window, text="Name:", font=("Helvetica", 12), bg="#FFFDD0").pack(pady=(20, 5))
    entry_name = tk.Entry(add_supplier_window, font=("Helvetica", 12))
    entry_name.pack(pady=5)

    tk.Label(add_supplier_window, text="Supplier ID:", font=("Helvetica", 12), bg="#FFFDD0").pack(pady=5)
    entry_supplier_id = tk.Entry(add_supplier_window, font=("Helvetica", 12))
    entry_supplier_id.pack(pady=5)

    tk.Label(add_supplier_window, text="Address:", font=("Helvetica", 12), bg="#FFFDD0").pack(pady=5)
    entry_address = tk.Entry(add_supplier_window, font=("Helvetica", 12))
    entry_address.pack(pady=5)

    tk.Label(add_supplier_window, text="Contact Details:", font=("Helvetica", 12), bg="#FFFDD0").pack(pady=5)
    entry_contact_details = tk.Entry(add_supplier_window, font=("Helvetica", 12))
    entry_contact_details.pack(pady=5)

    tk.Label(add_supplier_window, text="Menu:", font=("Helvetica", 12), bg="#FFFDD0").pack(pady=5)
    entry_menu = tk.Entry(add_supplier_window, font=("Helvetica", 12))
    entry_menu.pack(pady=5)

    tk.Label(add_supplier_window, text="Min Guests:", font=("Helvetica", 12), bg="#FFFDD0").pack(pady=5)
    entry_min_guests = tk.Entry(add_supplier_window, font=("Helvetica", 12))
    entry_min_guests.pack(pady=5)

    tk.Label(add_supplier_window, text="Max Guests:", font=("Helvetica", 12), bg="#FFFDD0").pack(pady=5)
    entry_max_guests = tk.Entry(add_supplier_window, font=("Helvetica", 12))
    entry_max_guests.pack(pady=5)

    # Function to add supplier details
    def add_supplier_details():
        # Get values from entry fields
        name = entry_name.get()
        supplier_id = entry_supplier_id.get()
        address = entry_address.get()
        contact_details = entry_contact_details.get()
        menu = entry_menu.get()
        min_guests = entry_min_guests.get()
        max_guests = entry_max_guests.get()

        # Create an instance of the Supplier class with the provided details
        supplier = Supplier(supplier_id, name, address, contact_details, menu, min_guests, max_guests)

        # Add supplier details to the database
        supplier.add_to_database()

        # Show success message
        messagebox.showinfo("Success", "Supplier added successfully!")
        add_supplier_window.destroy()

    # Button to add supplier details
    tk.Button(add_supplier_window, text="Add Supplier", command=add_supplier_details, bg="#4CAF50", fg="white",
              font=("Helvetica", 12)).pack(pady=20)


def edit_supplier_placeholder(self):
    # Read existing supplier data
    existing_data = Supplier.read_supplier_data()

    # Create a Toplevel window for selecting a supplier to edit
    select_supplier_window = tk.Toplevel(self.supplier_window)
    select_supplier_window.title("Select Supplier")
    select_supplier_window.geometry("400x400")
    select_supplier_window.config(bg="#FFFDD0")

    # Function to handle the supplier selection and open the Edit Supplier Window
    def open_edit_window():
        selected_entry = listbox.curselection()
        if selected_entry:
            selected_supplier_entry = listbox.get(selected_entry[0])  # Get the selected supplier entry
            selected_supplier_id = selected_supplier_entry.split(" - ")[-1]  # Extract the supplier ID
            select_supplier_window.destroy()  # Close the Select Supplier Window
            open_edit_supplier_window(
                existing_data[selected_supplier_id])  # Open the Edit Supplier Window with selected supplier data

    # Display a list of supplier names and IDs
    listbox = tk.Listbox(select_supplier_window, width=50, height=10)
    for supplier_id, supplier_details in existing_data.items():
        listbox.insert(tk.END, f"{supplier_details['name']} - {supplier_id}")
    listbox.pack(pady=20)

    # Button to select a supplier and open the Edit Supplier Window
    select_button = tk.Button(select_supplier_window, text="Select Supplier", command=open_edit_window, bg="#4CAF50",
                              fg="white", font=("Helvetica", 12))
    select_button.pack(pady=10)

    def open_edit_supplier_window(supplier_details):
        # Create a Toplevel window for editing a supplier
        edit_supplier_window = tk.Toplevel(self.supplier_window)
        edit_supplier_window.title("Edit Supplier")
        edit_supplier_window.geometry("400x700")
        edit_supplier_window.config(bg="#FFFDD0")

        # Labels and Entry fields for supplier details
        tk.Label(edit_supplier_window, text="Name:", font=("Helvetica", 12), bg="#FFFDD0").pack(pady=(20, 5))
        entry_name = tk.Entry(edit_supplier_window, font=("Helvetica", 12))
        entry_name.insert(0, supplier_details["name"])
        entry_name.pack(pady=5)

        tk.Label(edit_supplier_window, text="Supplier ID:", font=("Helvetica", 12), bg="#FFFDD0").pack(pady=5)
        entry_supplier_id = tk.Entry(edit_supplier_window, font=("Helvetica", 12))
        entry_supplier_id.insert(0, supplier_details["supplier_id"])
        entry_supplier_id.pack(pady=5)

        tk.Label(edit_supplier_window, text="Address:", font=("Helvetica", 12), bg="#FFFDD0").pack(pady=5)
        entry_address = tk.Entry(edit_supplier_window, font=("Helvetica", 12))
        entry_address.insert(0, supplier_details["address"])
        entry_address.pack(pady=5)

        tk.Label(edit_supplier_window, text="Contact Details:", font=("Helvetica", 12), bg="#FFFDD0").pack(pady=5)
        entry_contact_details = tk.Entry(edit_supplier_window, font=("Helvetica", 12))
        entry_contact_details.insert(0, supplier_details["contact_details"])
        entry_contact_details.pack(pady=5)

        tk.Label(edit_supplier_window, text="Menu:", font=("Helvetica", 12), bg="#FFFDD0").pack(pady=5)
        entry_menu = tk.Entry(edit_supplier_window, font=("Helvetica", 12))
        entry_menu.insert(0, supplier_details["menu"])
        entry_menu.pack(pady=5)

        tk.Label(edit_supplier_window, text="Min Guests:", font=("Helvetica", 12), bg="#FFFDD0").pack(pady=5)
        entry_min_guests = tk.Entry(edit_supplier_window, font=("Helvetica", 12))
        entry_min_guests.insert(0, supplier_details["min_guests"])
        entry_min_guests.pack(pady=5)

        tk.Label(edit_supplier_window, text="Max Guests:", font=("Helvetica", 12), bg="#FFFDD0").pack(pady=5)
        entry_max_guests = tk.Entry(edit_supplier_window, font=("Helvetica", 12))
        entry_max_guests.insert(0, supplier_details["max_guests"])
        entry_max_guests.pack(pady=5)

        # Function to update supplier details
        def update_supplier_details():
            # Get updated values from entry fields
            updated_supplier = Supplier(
                supplier_id=entry_supplier_id.get(),
                name=entry_name.get(),
                address=entry_address.get(),
                contact_details=entry_contact_details.get(),
                menu=entry_menu.get(),
                min_guests=entry_min_guests.get(),
                max_guests=entry_max_guests.get()
            )

            # Update the supplier data in the database
            existing_data[updated_supplier.supplier_id] = {
                "name": updated_supplier.name,
                "supplier_id": updated_supplier.supplier_id,
                "address": updated_supplier.address,
                "contact_details": updated_supplier.contact_details,
                "menu": updated_supplier.menu,
                "min_guests": updated_supplier.min_guests,
                "max_guests": updated_supplier.max_guests
            }

            # Write the updated data back to the file
            Supplier.write_supplier_data(existing_data)

            # Show success message
            messagebox.showinfo("Success", "Supplier details updated successfully!")
            edit_supplier_window.destroy()

        # Button to update supplier details
        update_button = tk.Button(edit_supplier_window, text="Update Supplier", command=update_supplier_details,
                                  bg="#4CAF50", fg="white", font=("Helvetica", 12))
        update_button.pack(pady=20)


def delete_supplier_placeholder(self):
    # Read existing supplier data
    existing_data = Supplier.read_supplier_data()

    # Create a Toplevel window for deleting a supplier
    delete_supplier_window = tk.Toplevel(self.supplier_window)
    delete_supplier_window.title("Delete Supplier")
    delete_supplier_window.geometry("400x400")
    delete_supplier_window.config(bg="#FFFDD0")

    # Function to handle the supplier selection and deletion
    def delete_supplier():
        selected_entry = listbox.curselection()
        if selected_entry:
            selected_supplier_entry = listbox.get(selected_entry[0])  # Get the selected supplier entry
            selected_supplier_id = selected_supplier_entry.split(" - ")[-1]  # Extract the supplier ID
            confirm_delete = messagebox.askyesno("Confirmation", "Are you sure you want to delete this supplier?")
            if confirm_delete:
                del existing_data[selected_supplier_id]  # Delete the selected supplier
                Supplier.write_supplier_data(existing_data)  # Write the updated data back to the file
                messagebox.showinfo("Success", "Supplier deleted successfully!")
                delete_supplier_window.destroy()

    # Display a list of supplier names and IDs
    listbox = tk.Listbox(delete_supplier_window, width=50, height=10)
    for supplier_id, supplier_details in existing_data.items():
        listbox.insert(tk.END, f"{supplier_details['name']} - {supplier_id}")
    listbox.pack(pady=20)

    # Button to delete the selected supplier
    delete_button = tk.Button(delete_supplier_window, text="Delete Supplier", command=delete_supplier, bg="#4CAF50",
                              fg="white", font=("Helvetica", 12))
    delete_button.pack(pady=10)


def display_supplier_placeholder(self):
    # Read existing supplier data
    existing_data = Supplier.read_supplier_data()

    # Create a Toplevel window for displaying supplier details
    display_supplier_window = tk.Toplevel(self.supplier_window)
    display_supplier_window.title("Display Supplier Details")
    display_supplier_window.geometry("400x400")
    display_supplier_window.config(bg="#FFFDD0")

    # Function to handle the supplier selection and display details
    def display_supplier_details():
        selected_entry = listbox.curselection()
        if selected_entry:
            selected_supplier_entry = listbox.get(selected_entry[0])  # Get the selected supplier entry
            selected_supplier_id = selected_supplier_entry.split(" - ")[-1]  # Extract the supplier ID
            supplier_details = existing_data[selected_supplier_id]  # Get details of the selected supplier

            # Create labels to display supplier details
            tk.Label(display_supplier_window, text=f"Name: {supplier_details['name']}", font=("Helvetica", 12),
                     bg="#FFFDD0").pack(pady=5)
            tk.Label(display_supplier_window, text=f"Supplier ID: {supplier_details['supplier_id']}",
                     font=("Helvetica", 12), bg="#FFFDD0").pack(pady=5)
            tk.Label(display_supplier_window, text=f"Address: {supplier_details['address']}", font=("Helvetica", 12),
                     bg="#FFFDD0").pack(pady=5)
            tk.Label(display_supplier_window, text=f"Contact Details: {supplier_details['contact_details']}",
                     font=("Helvetica", 12), bg="#FFFDD0").pack(pady=5)
            tk.Label(display_supplier_window, text=f"Menu: {supplier_details['menu']}", font=("Helvetica", 12),
                     bg="#FFFDD0").pack(pady=5)
            tk.Label(display_supplier_window, text=f"Min Guests: {supplier_details['min_guests']}",
                     font=("Helvetica", 12), bg="#FFFDD0").pack(pady=5)
            tk.Label(display_supplier_window, text=f"Max Guests: {supplier_details['max_guests']}",
                     font=("Helvetica", 12), bg="#FFFDD0").pack(pady=5)

    # Display a list of supplier names and IDs
    listbox = tk.Listbox(display_supplier_window, width=50, height=10)
    for supplier_id, supplier_details in existing_data.items():
        listbox.insert(tk.END, f"{supplier_details['name']} - {supplier_id}")
    listbox.pack(pady=20)

    # Button to display supplier details
    display_button = tk.Button(display_supplier_window, text="Display Supplier Details",
                               command=display_supplier_details, bg="#4CAF50", fg="white", font=("Helvetica", 12))
    display_button.pack(pady=10)


def show_venue_page(self):
    self.venue_window = tk.Toplevel(self.root)
    self.venue_window.title("Venue Menu")
    self.venue_window.geometry("480x445")
    self.venue_window.config(bg="#FFFDD0")

    label_venue_menu = tk.Label(self.venue_window, text="Venue Menu", font=("Helvetica", 20), bg="#FFFDD0")
    label_venue_menu.pack(pady=20)

    button_add_venue = tk.Button(self.venue_window, text="Add Venue", command=self.add_venue_placeholder, bg="#FFA500",
                                 fg="white", font=("Helvetica", 12), width=20)
    button_add_venue.pack(pady=10)

    button_edit_venue = tk.Button(self.venue_window, text="Edit Venue", command=self.edit_venue_placeholder,
                                  bg="#FFA500", fg="white", font=("Helvetica", 12), width=20)
    button_edit_venue.pack(pady=10)

    button_delete_venue = tk.Button(self.venue_window, text="Delete Venue", command=self.delete_venue_placeholder,
                                    bg="#FFA500", fg="white", font=("Helvetica", 12), width=20)
    button_delete_venue.pack(pady=10)

    button_display_venue = tk.Button(self.venue_window, text="Display Venue", command=self.display_venue_placeholder,
                                     bg="#FFA500", fg="white", font=("Helvetica", 12), width=20)
    button_display_venue.pack(pady=10)


def add_venue_placeholder(self):
    # Create a Toplevel window for adding a venue
    add_venue_window = tk.Toplevel(self.venue_window)
    add_venue_window.title("Add Venue")
    add_venue_window.geometry("400x700")
    add_venue_window.config(bg="#FFFDD0")

    # Labels and Entry fields for venue details
    tk.Label(add_venue_window, text="Name:", font=("Helvetica", 12), bg="#FFFDD0").pack(pady=(20, 5))
    entry_name = tk.Entry(add_venue_window, font=("Helvetica", 12))
    entry_name.pack(pady=5)

    tk.Label(add_venue_window, text="Venue ID:", font=("Helvetica", 12), bg="#FFFDD0").pack(pady=5)
    entry_venue_id = tk.Entry(add_venue_window, font=("Helvetica", 12))
    entry_venue_id.pack(pady=5)

    tk.Label(add_venue_window, text="Address:", font=("Helvetica", 12), bg="#FFFDD0").pack(pady=5)
    entry_address = tk.Entry(add_venue_window, font=("Helvetica", 12))
    entry_address.pack(pady=5)

    tk.Label(add_venue_window, text="Contact:", font=("Helvetica", 12), bg="#FFFDD0").pack(pady=5)
    entry_contact = tk.Entry(add_venue_window, font=("Helvetica", 12))
    entry_contact.pack(pady=5)

    tk.Label(add_venue_window, text="Min Guests:", font=("Helvetica", 12), bg="#FFFDD0").pack(pady=5)
    entry_min_guests = tk.Entry(add_venue_window, font=("Helvetica", 12))
    entry_min_guests.pack(pady=5)

    tk.Label(add_venue_window, text="Max Guests:", font=("Helvetica", 12), bg="#FFFDD0").pack(pady=5)
    entry_max_guests = tk.Entry(add_venue_window, font=("Helvetica", 12))
    entry_max_guests.pack(pady=5)

    # Function to add venue details
    def add_venue_details():
        # Get values from entry fields
        name = entry_name.get()
        venue_id = entry_venue_id.get()
        address = entry_address.get()
        contact = entry_contact.get()
        min_guests = entry_min_guests.get()
        max_guests = entry_max_guests.get()

        # Create an instance of the Venue class with the provided details
        venue = Venue(venue_id, name, address, contact, min_guests, max_guests)

        # Add venue details to the database
        venue.add_to_database()

        # Show success message
        messagebox.showinfo("Success", "Venue added successfully!")
        add_venue_window.destroy()

    # Button to add venue details
    tk.Button(add_venue_window, text="Add Venue", command=add_venue_details, bg="#4CAF50", fg="white",
              font=("Helvetica", 12)).pack(pady=20)


def edit_venue_placeholder(self):
    # Read existing venue data
    existing_data = Venue.read_venue_data()

    # Create a Toplevel window for selecting a venue
    select_venue_window = tk.Toplevel(self.venue_window)
    select_venue_window.title("Select Venue")
    select_venue_window.geometry("400x400")
    select_venue_window.config(bg="#FFFDD0")

    # Function to handle the venue selection and open the Edit Venue Window
    def open_edit_window():
        selected_entry = listbox.curselection()
        if selected_entry:
            selected_venue_entry = listbox.get(selected_entry[0])  # Get the selected venue entry
            selected_venue_id = selected_venue_entry.split(" - ")[-1]  # Extract the venue ID
            select_venue_window.destroy()  # Close the Select Venue Window
            open_edit_venue_window(
                existing_data[selected_venue_id])  # Open the Edit Venue Window with selected venue data

    # Display a list of venue names and IDs
    listbox = tk.Listbox(select_venue_window, width=50, height=10)
    for venue_id, venue_details in existing_data.items():
        listbox.insert(tk.END, f"{venue_details['name']} - {venue_id}")
    listbox.pack(pady=20)

    # Button to select a venue and open the Edit Venue Window
    select_button = tk.Button(select_venue_window, text="Select Venue", command=open_edit_window, bg="#4CAF50",
                              fg="white", font=("Helvetica", 12))
    select_button.pack(pady=10)

    def open_edit_venue_window(venue_details):
        # Create a Toplevel window for editing a venue
        edit_venue_window = tk.Toplevel(self.venue_window)
        edit_venue_window.title("Edit Venue")
        edit_venue_window.geometry("400x900")
        edit_venue_window.config(bg="#FFFDD0")

        # Function to update venue details
        def update_venue_details():
            # Get updated values from entry fields
            updated_venue = Venue(
                venue_id=entry_venue_id.get(),
                name=entry_name.get(),
                address=entry_address.get(),
                contact=entry_contact.get(),
                min_guests=entry_min_guests.get(),
                max_guests=entry_max_guests.get()
            )

            # Update the venue data in the database
            existing_data[updated_venue.venue_id] = {
                "name": updated_venue.name,
                "venue_id": updated_venue.venue_id,
                "address": updated_venue.address,
                "contact": updated_venue.contact,
                "min_guests": updated_venue.min_guests,
                "max_guests": updated_venue.max_guests
            }

            # Write the updated data back to the file
            Venue.write_venue_data(existing_data)

            # Show success message
            messagebox.showinfo("Success", "Venue details updated successfully!")
            edit_venue_window.destroy()

        # Labels and Entry fields for venue details
        tk.Label(edit_venue_window, text="Name:", font=("Helvetica", 12), bg="#FFFDD0").pack(pady=5)
        entry_name = tk.Entry(edit_venue_window, font=("Helvetica", 12))
        entry_name.insert(0, venue_details["name"])
        entry_name.pack(pady=5)

        tk.Label(edit_venue_window, text="Venue ID:", font=("Helvetica", 12), bg="#FFFDD0").pack(pady=5)
        entry_venue_id = tk.Entry(edit_venue_window, font=("Helvetica", 12))
        entry_venue_id.insert(0, venue_details["venue_id"])
        entry_venue_id.pack(pady=5)

        tk.Label(edit_venue_window, text="Address:", font=("Helvetica", 12), bg="#FFFDD0").pack(pady=5)
        entry_address = tk.Entry(edit_venue_window, font=("Helvetica", 12))
        entry_address.insert(0, venue_details["address"])
        entry_address.pack(pady=5)

        tk.Label(edit_venue_window, text="Contact:", font=("Helvetica", 12), bg="#FFFDD0").pack(pady=5)
        entry_contact = tk.Entry(edit_venue_window, font=("Helvetica", 12))
        entry_contact.insert(0, venue_details["contact"])
        entry_contact.pack(pady=5)

        tk.Label(edit_venue_window, text="Min Guests:", font=("Helvetica", 12), bg="#FFFDD0").pack(pady=5)
        entry_min_guests = tk.Entry(edit_venue_window, font=("Helvetica", 12))
        entry_min_guests.insert(0, venue_details["min_guests"])
        entry_min_guests.pack(pady=5)

        tk.Label(edit_venue_window, text="Max Guests:", font=("Helvetica", 12), bg="#FFFDD0").pack(pady=5)
        entry_max_guests = tk.Entry(edit_venue_window, font=("Helvetica", 12))
        entry_max_guests.insert(0, venue_details["max_guests"])
        entry_max_guests.pack(pady=5)

        # Button to update venue details
        update_button = tk.Button(edit_venue_window, text="Update Venue", command=update_venue_details, bg="#4CAF50",
                                  fg="white", font=("Helvetica", 12))
        update_button.pack(pady=20)


def delete_venue_placeholder(self):
    # Read existing venue data
    existing_data = Venue.read_venue_data()

    # Create a Toplevel window for deleting a venue
    delete_venue_window = tk.Toplevel(self.venue_window)
    delete_venue_window.title("Delete Venue")
    delete_venue_window.geometry("400x400")
    delete_venue_window.config(bg="#FFFDD0")

    # Function to handle the venue selection and deletion
    def delete_venue():
        selected_entry = listbox.curselection()
        if selected_entry:
            selected_venue_entry = listbox.get(selected_entry[0])  # Get the selected venue entry
            selected_venue_id = selected_venue_entry.split(" - ")[-1]  # Extract the venue ID
            confirm_delete = messagebox.askyesno("Confirmation", "Are you sure you want to delete this venue?")
            if confirm_delete:
                del existing_data[selected_venue_id]  # Delete the selected venue
                Venue.write_venue_data(existing_data)  # Write the updated data back to the file
                messagebox.showinfo("Success", "Venue deleted successfully!")
                delete_venue_window.destroy()

    # Display a list of venue names and IDs
    listbox = tk.Listbox(delete_venue_window, width=50, height=10)
    for venue_id, venue_details in existing_data.items():
        listbox.insert(tk.END, f"{venue_details['name']} - {venue_id}")
    listbox.pack(pady=20)

    # Button to delete the selected venue
    delete_button = tk.Button(delete_venue_window, text="Delete Venue", command=delete_venue, bg="#4CAF50", fg="white",
                              font=("Helvetica", 12))
    delete_button.pack(pady=10)


def display_venue_placeholder(self):
    # Read existing venue data
    existing_data = Venue.read_venue_data()

    # Create a Toplevel window for displaying venue details
    display_venue_window = tk.Toplevel(self.venue_window)
    display_venue_window.title("Display Venue Details")
    display_venue_window.geometry("400x650")
    display_venue_window.config(bg="#FFFDD0")

    # Function to handle the venue selection and display details
    def display_venue_details():
        selected_entry = listbox.curselection()
        if selected_entry:
            selected_venue_entry = listbox.get(selected_entry[0])  # Get the selected venue entry
            selected_venue_id = selected_venue_entry.split(" - ")[-1]  # Extract the venue ID
            venue_details = existing_data[selected_venue_id]  # Get details of the selected venue

            # Create labels to display venue details
            tk.Label(display_venue_window, text=f"Name: {venue_details['name']}", font=("Helvetica", 12),
                     bg="#FFFDD0").pack(pady=5)
            tk.Label(display_venue_window, text=f"Venue ID: {venue_details['venue_id']}", font=("Helvetica", 12),
                     bg="#FFFDD0").pack(pady=5)
            tk.Label(display_venue_window, text=f"Address: {venue_details['address']}", font=("Helvetica", 12),
                     bg="#FFFDD0").pack(pady=5)
            tk.Label(display_venue_window, text=f"Contact: {venue_details['contact']}", font=("Helvetica", 12),
                     bg="#FFFDD0").pack(pady=5)
            tk.Label(display_venue_window, text=f"Min Guests: {venue_details['min_guests']}", font=("Helvetica", 12),
                     bg="#FFFDD0").pack(pady=5)
            tk.Label(display_venue_window, text=f"Max Guests: {venue_details['max_guests']}", font=("Helvetica", 12),
                     bg="#FFFDD0").pack(pady=5)

    # Display a list of venue names and IDs
    listbox = tk.Listbox(display_venue_window, width=50, height=10)
    for venue_id, venue_details in existing_data.items():
        listbox.insert(tk.END, f"{venue_details['name']} - {venue_id}")
    listbox.pack(pady=20)

    # Button to display venue details
    display_button = tk.Button(display_venue_window, text="Display Venue Details", command=display_venue_details,
                               bg="#4CAF50", fg="white", font=("Helvetica", 12))
    display_button.pack(pady=10)


def show_guest_page(self):
    self.guest_window = tk.Toplevel(self.root)
    self.guest_window.title("Guest Menu")
    self.guest_window.geometry("480x445")
    self.guest_window.config(bg="#FFFDD0")

    label_guest_menu = tk.Label(self.guest_window, text="Guest Menu", font=("Helvetica", 20), bg="#FFFDD0")
    label_guest_menu.pack(pady=20)

    button_add_guest = tk.Button(self.guest_window, text="Add Guest", command=self.add_guest_placeholder, bg="#FFA500",
                                 fg="white", font=("Helvetica", 12), width=20)
    button_add_guest.pack(pady=10)

    button_edit_guest = tk.Button(self.guest_window, text="Edit Guest", command=self.edit_guest_placeholder,
                                  bg="#FFA500", fg="white", font=("Helvetica", 12), width=20)
    button_edit_guest.pack(pady=10)

    button_delete_guest = tk.Button(self.guest_window, text="Delete Guest", command=self.delete_guest_placeholder,
                                    bg="#FFA500", fg="white", font=("Helvetica", 12), width=20)
    button_delete_guest.pack(pady=10)

    button_display_guest = tk.Button(self.guest_window, text="Display Guest", command=self.display_guest_placeholder,
                                     bg="#FFA500", fg="white", font=("Helvetica", 12), width=20)
    button_display_guest.pack(pady=10)


def add_guest_placeholder(self):
    # Create a Toplevel window for adding a guest
    add_guest_window = tk.Toplevel(self.guest_window)
    add_guest_window.title("Add Guest")
    add_guest_window.geometry("400x600")
    add_guest_window.config(bg="#FFFDD0")

    # Labels and Entry fields for guest details
    tk.Label(add_guest_window, text="Name:", font=("Helvetica", 12), bg="#FFFDD0").pack(pady=(20, 5))
    entry_name = tk.Entry(add_guest_window, font=("Helvetica", 12))
    entry_name.pack(pady=5)

    tk.Label(add_guest_window, text="Guest ID:", font=("Helvetica", 12), bg="#FFFDD0").pack(pady=5)
    entry_guest_id = tk.Entry(add_guest_window, font=("Helvetica", 12))
    entry_guest_id.pack(pady=5)

    tk.Label(add_guest_window, text="Address:", font=("Helvetica", 12), bg="#FFFDD0").pack(pady=5)
    entry_address = tk.Entry(add_guest_window, font=("Helvetica", 12))
    entry_address.pack(pady=5)

    tk.Label(add_guest_window, text="Contact Details:", font=("Helvetica", 12), bg="#FFFDD0").pack(pady=5)
    entry_contact_details = tk.Entry(add_guest_window, font=("Helvetica", 12))
    entry_contact_details.pack(pady=5)

    # Function to add guest details
    def add_guest_details():
        # Get values from entry fields
        name = entry_name.get()
        guest_id = entry_guest_id.get()
        address = entry_address.get()
        contact_details = entry_contact_details.get()

        # Create an instance of the Guest class with the provided details
        guest = Guest(guest_id, name, address, contact_details)

        # Add guest details to the database
        guest.add_to_database()

        # Show success message
        messagebox.showinfo("Success", "Guest added successfully!")
        add_guest_window.destroy()

    # Button to add guest details
    tk.Button(add_guest_window, text="Add Guest", command=add_guest_details, bg="#4CAF50", fg="white",
              font=("Helvetica", 12)).pack(pady=20)


def edit_guest_placeholder(self):
    # Read existing guest data
    try:
        existing_data = read_data("guests.pkl")
    except FileNotFoundError:
        messagebox.showerror("Error", "No guest data found.")
        return

    # Create a Toplevel window for selecting a guest
    select_guest_window = tk.Toplevel(self.guest_window)
    select_guest_window.title("Select Guest")
    select_guest_window.geometry("400x400")
    select_guest_window.config(bg="#FFFDD0")

    # Function to handle the guest selection and open the Edit Guest Window
    def open_edit_window():
        selected_entry = listbox.curselection()
        if selected_entry:
            selected_guest_entry = listbox.get(selected_entry[0])  # Get the selected guest entry
            selected_guest_id = selected_guest_entry.split(" - ")[-1]  # Extract the guest ID
            select_guest_window.destroy()  # Close the Select Guest Window
            open_edit_guest_window(
                existing_data[selected_guest_id])  # Open the Edit Guest Window with selected guest data

    # Display a list of guest names and IDs
    listbox = tk.Listbox(select_guest_window, width=50, height=10)
    for guest_id, guest_details in existing_data.items():
        listbox.insert(tk.END, f"{guest_details['name']} - {guest_id}")
    listbox.pack(pady=20)

    # Button to select a guest and open the Edit Guest Window
    select_button = tk.Button(select_guest_window, text="Select Guest", command=open_edit_window, bg="#4CAF50",
                              fg="white", font=("Helvetica", 12))
    select_button.pack(pady=10)

    def open_edit_guest_window(guest_details):
        # Create a Toplevel window for editing a guest
        edit_guest_window = tk.Toplevel(self.guest_window)
        edit_guest_window.title("Edit Guest")
        edit_guest_window.geometry("400x600")
        edit_guest_window.config(bg="#FFFDD0")

        # Function to update guest details
        def update_guest_details():
            # Get updated values from entry fields
            updated_guest = Guest(
                guest_id=entry_guest_id.get(),
                name=entry_name.get(),
                address=entry_address.get(),
                contact_details=entry_contact_details.get()
            )

            # Update the guest data in the database
            existing_data[updated_guest.guest_id] = {
                "guest_id": updated_guest.guest_id,
                "name": updated_guest.name,
                "address": updated_guest.address,
                "contact_details": updated_guest.contact_details
            }

            # Write the updated data back to the file
            write_data("guests.pkl", existing_data)

            # Show success message
            messagebox.showinfo("Success", "Guest details updated successfully!")
            edit_guest_window.destroy()

        # Labels and Entry fields for guest details
        tk.Label(edit_guest_window, text="Name:", font=("Helvetica", 12), bg="#FFFDD0").pack(pady=5)
        entry_name = tk.Entry(edit_guest_window, font=("Helvetica", 12))
        entry_name.insert(0, guest_details["name"])
        entry_name.pack(pady=5)

        tk.Label(edit_guest_window, text="Guest ID:", font=("Helvetica", 12), bg="#FFFDD0").pack(pady=5)
        entry_guest_id = tk.Entry(edit_guest_window, font=("Helvetica", 12))
        entry_guest_id.insert(0, guest_details["guest_id"])
        entry_guest_id.pack(pady=5)

        tk.Label(edit_guest_window, text="Address:", font=("Helvetica", 12), bg="#FFFDD0").pack(pady=5)
        entry_address = tk.Entry(edit_guest_window, font=("Helvetica", 12))
        entry_address.insert(0, guest_details["address"])
        entry_address.pack(pady=5)

        tk.Label(edit_guest_window, text="Contact Details:", font=("Helvetica", 12), bg="#FFFDD0").pack(pady=5)
        entry_contact_details = tk.Entry(edit_guest_window, font=("Helvetica", 12))
        entry_contact_details.insert(0, guest_details["contact_details"])
        entry_contact_details.pack(pady=5)

        # Button to update guest details
        update_button = tk.Button(edit_guest_window, text="Update Guest", command=update_guest_details, bg="#4CAF50",
                                  fg="white", font=("Helvetica", 12))
        update_button.pack(pady=20)


def delete_guest_placeholder(self):
    # Read existing guest data
    existing_data = Guest.read_guest_data()

    # Create a Toplevel window for deleting a guest
    delete_guest_window = tk.Toplevel(self.guest_window)
    delete_guest_window.title("Delete Guest")
    delete_guest_window.geometry("400x400")
    delete_guest_window.config(bg="#FFFDD0")

    # Function to handle the guest selection and deletion
    def delete_guest():
        selected_entry = listbox.curselection()
        if selected_entry:
            selected_guest_entry = listbox.get(selected_entry[0])  # Get the selected guest entry
            selected_guest_id = selected_guest_entry.split(" - ")[-1]  # Extract the guest ID
            confirm_delete = messagebox.askyesno("Confirmation", "Are you sure you want to delete this guest?")
            if confirm_delete:
                del existing_data[selected_guest_id]  # Delete the selected guest
                Guest.write_guest_data(existing_data)  # Write the updated data back to the file
                messagebox.showinfo("Success", "Guest deleted successfully!")
                delete_guest_window.destroy()

    # Display a list of guest names and IDs
    listbox = tk.Listbox(delete_guest_window, width=50, height=10)
    for guest_id, guest_details in existing_data.items():
        listbox.insert(tk.END, f"{guest_details['name']} - {guest_id}")
    listbox.pack(pady=20)

    # Button to delete the selected guest
    delete_button = tk.Button(delete_guest_window, text="Delete Guest", command=delete_guest, bg="#4CAF50", fg="white",
                              font=("Helvetica", 12))
    delete_button.pack(pady=10)


def display_guest_placeholder(self):
    # Read existing guest data
    existing_data = Guest.read_guest_data()

    # Create a Toplevel window for displaying guest details
    display_guest_window = tk.Toplevel(self.guest_window)
    display_guest_window.title("Display Guest Details")
    display_guest_window.geometry("400x400")
    display_guest_window.config(bg="#FFFDD0")

    # Function to handle the guest selection and display details
    def display_guest_details():
        selected_entry = listbox.curselection()
        if selected_entry:
            selected_guest_entry = listbox.get(selected_entry[0])  # Get the selected guest entry
            selected_guest_id = selected_guest_entry.split(" - ")[-1]  # Extract the guest ID
            guest_details = existing_data[selected_guest_id]  # Get details of the selected guest

            # Create labels to display guest details
            tk.Label(display_guest_window, text=f"Name: {guest_details['name']}", font=("Helvetica", 12),
                     bg="#FFFDD0").pack(pady=5)
            tk.Label(display_guest_window, text=f"Guest ID: {guest_details['guest_id']}", font=("Helvetica", 12),
                     bg="#FFFDD0").pack(pady=5)
            tk.Label(display_guest_window, text=f"Address: {guest_details['address']}", font=("Helvetica", 12),
                     bg="#FFFDD0").pack(pady=5)
            tk.Label(display_guest_window, text=f"Contact Details: {guest_details['contact_details']}",
                     font=("Helvetica", 12), bg="#FFFDD0").pack(pady=5)

    # Display a list of guest names and IDs
    listbox = tk.Listbox(display_guest_window, width=50, height=10)
    for guest_id, guest_details in existing_data.items():
        listbox.insert(tk.END, f"{guest_details['name']} - {guest_id}")
    listbox.pack(pady=20)

    # Button to display guest details
    display_button = tk.Button(display_guest_window, text="Display Guest Details", command=display_guest_details,
                               bg="#4CAF50", fg="white", font=("Helvetica", 12))
    display_button.pack(pady=10)


def show_client_page(self):
    self.client_window = tk.Toplevel(self.root)
    self.client_window.title("Client Menu")
    self.client_window.geometry("480x445")
    self.client_window.config(bg="#FFFDD0")

    label_client_menu = tk.Label(self.client_window, text="Client Menu", font=("Helvetica", 20), bg="#FFFDD0")
    label_client_menu.pack(pady=20)

    button_add_client = tk.Button(self.client_window, text="Add Client", command=self.add_client_placeholder,
                                  bg="#FFA500", fg="white", font=("Helvetica", 12), width=20)
    button_add_client.pack(pady=10)

    button_edit_client = tk.Button(self.client_window, text="Edit Client", command=self.edit_client_placeholder,
                                   bg="#FFA500", fg="white", font=("Helvetica", 12), width=20)
    button_edit_client.pack(pady=10)

    button_delete_client = tk.Button(self.client_window, text="Delete Client", command=self.delete_client_placeholder,
                                     bg="#FFA500", fg="white", font=("Helvetica", 12), width=20)
    button_delete_client.pack(pady=10)

    button_display_client = tk.Button(self.client_window, text="Display Client",
                                      command=self.display_client_placeholder, bg="#FFA500", fg="white",
                                      font=("Helvetica", 12), width=20)
    button_display_client.pack(pady=10)


def add_client_placeholder(self):
    # Create a Toplevel window for adding a client
    add_client_window = tk.Toplevel(self.client_window)
    add_client_window.title("Add Client")
    add_client_window.geometry("400x500")
    add_client_window.config(bg="#FFFDD0")

    # Labels and Entry fields for client details
    tk.Label(add_client_window, text="Name:", font=("Helvetica", 12), bg="#FFFDD0").pack(pady=20)
    entry_name = tk.Entry(add_client_window, font=("Helvetica", 12))
    entry_name.pack(pady=5)

    tk.Label(add_client_window, text="Client ID:", font=("Helvetica", 12), bg="#FFFDD0").pack(pady=5)
    entry_client_id = tk.Entry(add_client_window, font=("Helvetica", 12))
    entry_client_id.pack(pady=5)

    tk.Label(add_client_window, text="Address:", font=("Helvetica", 12), bg="#FFFDD0").pack(pady=5)
    entry_address = tk.Entry(add_client_window, font=("Helvetica", 12))
    entry_address.pack(pady=5)

    tk.Label(add_client_window, text="Contact Details:", font=("Helvetica", 12), bg="#FFFDD0").pack(pady=5)
    entry_contact_details = tk.Entry(add_client_window, font=("Helvetica", 12))
    entry_contact_details.pack(pady=5)

    tk.Label(add_client_window, text="Budget:", font=("Helvetica", 12), bg="#FFFDD0").pack(pady=5)
    entry_budget = tk.Entry(add_client_window, font=("Helvetica", 12))
    entry_budget.pack(pady=5)

    # Function to add client details
    def add_client_details():
        # Get values from entry fields
        name = entry_name.get()
        client_id = entry_client_id.get()
        address = entry_address.get()
        contact_details = entry_contact_details.get()
        budget = entry_budget.get()

        # Create an instance of the Client class with the provided details
        client = Client(client_id, name, address, contact_details, budget)

        # Add client details to the database
        client.add_to_database()

        # Show success message
        messagebox.showinfo("Success", "Client added successfully!")
        add_client_window.destroy()

    # Button to add client details
    tk.Button(add_client_window, text="Add Client", command=add_client_details, bg="#4CAF50", fg="white",
              font=("Helvetica", 12)).pack(pady=20)


def edit_client_placeholder(self):
    # Read existing client data
    try:
        existing_data = read_data("clients.pkl")
    except FileNotFoundError:
        messagebox.showerror("Error", "No client data found.")
        return

    # Create a Toplevel window for selecting a client
    select_client_window = tk.Toplevel(self.client_window)
    select_client_window.title("Select Client")
    select_client_window.geometry("400x400")
    select_client_window.config(bg="#FFFDD0")

    # Function to handle the client selection and open the Edit Client Window
    def open_edit_window():
        selected_entry = listbox.curselection()
        if selected_entry:
            selected_client_entry = listbox.get(selected_entry[0])  # Get the selected client entry
            selected_client_id = selected_client_entry.split(" - ")[-1]  # Extract the client ID
            select_client_window.destroy()  # Close the Select Client Window
            open_edit_client_window(
                existing_data[selected_client_id])  # Open the Edit Client Window with selected client data

    # Display a list of client names and IDs
    listbox = tk.Listbox(select_client_window, width=50, height=10)
    for client_id, client_details in existing_data.items():
        listbox.insert(tk.END, f"{client_details['name']} - {client_id}")
    listbox.pack(pady=20)

    # Button to select a client and open the Edit Client Window
    select_button = tk.Button(select_client_window, text="Select Client", command=open_edit_window, bg="#4CAF50",
                              fg="white", font=("Helvetica", 12))
    select_button.pack(pady=10)

    def open_edit_client_window(client_details):
        # Create a Toplevel window for editing a client
        edit_client_window = tk.Toplevel(self.client_window)
        edit_client_window.title("Edit Client")
        edit_client_window.geometry("400x500")
        edit_client_window.config(bg="#FFFDD0")

        # Function to update client details
        def update_client_details():
            # Get updated values from entry fields
            updated_client = Client(
                client_id=entry_client_id.get(),
                name=entry_name.get(),
                address=entry_address.get(),
                contact_details=entry_contact_details.get(),
                budget=entry_budget.get()
            )

            # Update the client data in the database
            existing_data[updated_client.client_id] = {
                "name": updated_client.name,
                "client_id": updated_client.client_id,
                "address": updated_client.address,
                "contact_details": updated_client.contact_details,
                "budget": updated_client.budget
            }

            # Write the updated data back to the file
            write_data("clients.pkl", existing_data)

            # Show success message
            messagebox.showinfo("Success", "Client details updated successfully!")
            edit_client_window.destroy()

        # Labels and Entry fields for client details
        tk.Label(edit_client_window, text="Name:", font=("Helvetica", 12), bg="#FFFDD0").pack(pady=5)
        entry_name = tk.Entry(edit_client_window, font=("Helvetica", 12))
        entry_name.insert(0, client_details["name"])
        entry_name.pack(pady=5)

        tk.Label(edit_client_window, text="Client ID:", font=("Helvetica", 12), bg="#FFFDD0").pack(pady=5)
        entry_client_id = tk.Entry(edit_client_window, font=("Helvetica", 12))
        entry_client_id.insert(0, client_details["client_id"])
        entry_client_id.pack(pady=5)

        tk.Label(edit_client_window, text="Address:", font=("Helvetica", 12), bg="#FFFDD0").pack(pady=5)
        entry_address = tk.Entry(edit_client_window, font=("Helvetica", 12))
        entry_address.insert(0, client_details["address"])
        entry_address.pack(pady=5)

        tk.Label(edit_client_window, text="Contact Details:", font=("Helvetica", 12), bg="#FFFDD0").pack(pady=5)
        entry_contact_details = tk.Entry(edit_client_window, font=("Helvetica", 12))
        entry_contact_details.insert(0, client_details["contact_details"])
        entry_contact_details.pack(pady=5)

        tk.Label(edit_client_window, text="Budget:", font=("Helvetica", 12), bg="#FFFDD0").pack(pady=5)
        entry_budget = tk.Entry(edit_client_window, font=("Helvetica", 12))
        entry_budget.insert(0, client_details["budget"])
        entry_budget.pack(pady=5)

        # Button to update client details
        update_button = tk.Button(edit_client_window, text="Update Client", command=update_client_details, bg="#4CAF50",
                                  fg="white", font=("Helvetica", 12))
        update_button.pack(pady=20)


def delete_client_placeholder(self):
    # Read existing client data
    existing_data = Client.read_client_data()

    # Create a Toplevel window for deleting a client
    delete_client_window = tk.Toplevel(self.client_window)
    delete_client_window.title("Delete Client")
    delete_client_window.geometry("400x400")
    delete_client_window.config(bg="#FFFDD0")

    # Function to handle the client selection and deletion
    def delete_client():
        selected_entry = listbox.curselection()
        if selected_entry:
            selected_client_entry = listbox.get(selected_entry[0])  # Get the selected client entry
            selected_client_id = selected_client_entry.split(" - ")[-1]  # Extract the client ID
            confirm_delete = messagebox.askyesno("Confirmation", "Are you sure you want to delete this client?")
            if confirm_delete:
                del existing_data[selected_client_id]  # Delete the selected client
                Client.write_client_data(existing_data)  # Write the updated data back to the file
                messagebox.showinfo("Success", "Client deleted successfully!")
                delete_client_window.destroy()

    # Display a list of client names and IDs
    listbox = tk.Listbox(delete_client_window, width=50, height=10)
    for client_id, client_details in existing_data.items():
        listbox.insert(tk.END, f"{client_details['name']} - {client_id}")
    listbox.pack(pady=20)

    # Button to delete the selected client
    delete_button = tk.Button(delete_client_window, text="Delete Client", command=delete_client, bg="#4CAF50",
                              fg="white", font=("Helvetica", 12))
    delete_button.pack(pady=10)


def display_client_placeholder(self):
    # Read existing client data
    existing_data = Client.read_client_data()

    # Create a Toplevel window for displaying client details
    display_client_window = tk.Toplevel(self.client_window)
    display_client_window.title("Display Client Details")
    display_client_window.geometry("400x500")
    display_client_window.config(bg="#FFFDD0")

    # Function to handle the client selection and display details
    def display_client_details():
        selected_entry = listbox.curselection()
        if selected_entry:
            selected_client_entry = listbox.get(selected_entry[0])  # Get the selected client entry
            selected_client_id = selected_client_entry.split(" - ")[-1]  # Extract the client ID
            client_details = existing_data[selected_client_id]  # Get details of the selected client

            # Create labels to display client details
            tk.Label(display_client_window, text=f"Name: {client_details['name']}", font=("Helvetica", 12),
                     bg="#FFFDD0").pack(pady=5)
            tk.Label(display_client_window, text=f"Client ID: {client_details['client_id']}", font=("Helvetica", 12),
                     bg="#FFFDD0").pack(pady=5)
            tk.Label(display_client_window, text=f"Address: {client_details['address']}", font=("Helvetica", 12),
                     bg="#FFFDD0").pack(pady=5)
            tk.Label(display_client_window, text=f"Contact Details: {client_details['contact_details']}",
                     font=("Helvetica", 12), bg="#FFFDD0").pack(pady=5)
            tk.Label(display_client_window, text=f"Budget: {client_details['budget']}", font=("Helvetica", 12),
                     bg="#FFFDD0").pack(pady=5)

    # Display a list of client names and IDs
    listbox = tk.Listbox(display_client_window, width=50, height=10)
    for client_id, client_details in existing_data.items():
        listbox.insert(tk.END, f"{client_details['name']} - {client_id}")
    listbox.pack(pady=20)

    # Button to display client details
    display_button = tk.Button(display_client_window, text="Display Client Details", command=display_client_details,
                               bg="#4CAF50", fg="white", font=("Helvetica", 12))
    display_button.pack(pady=10)


def run(self):
    self.root.mainloop()


if __name__ == "__main__":
    company_system = CompanySystem()
    company_system.run()



#Data_storage.py file
import pickle  # Importing the pickle library 


def write_data(filename, data):  # Defining a function named 'write_data' to write data to a file.
    with open(filename, 'wb') as file:  # Opening the file in binary write mode.
        pickle.dump(data, file)  # writing the file.


def read_data(filename):  # Defining a function named 'read_data' to read data from a file.
    with open(filename, 'rb') as file:  # Opening the file in binary read mode.
        data = pickle.load(file)  # Loading data from the file.
    return data  # Returning data.



#Employee.py file
import data_storage  # Importing a module data_storage which handles data storage operations.
from tkinter import messagebox  # Import module of messagebox from tkinter


class Employee:  # Defining a class named 'Employee' to represent employee data.
    def __init__(self, name, employee_id, department, job_title, basic_salary, age, dob, passport_details, manager_id):
        # Initializing the attributes of the Employee class with provided values.
        self.name = name  # employee name.
        self.employee_id = employee_id  # employee ID.
        self.department = department  # employee department.
        self.job_title = job_title  # employee job title.
        self.basic_salary = basic_salary  # employee basic salary.
        self.age = age  # employee age.
        self.dob = dob  # employee date of birth.
        self.passport_details = passport_details  # employee passport details.
        self.manager_id = manager_id  # employee manager.

    def add_to_database(self):  # Defining a method to add employee data to a database.
        try:
            existing_data = data_storage.read_data("employees.pkl")  # Trying to read existing data from a file.
        except FileNotFoundError:  # Handling the case where the file is not found.
            existing_data = {}  # If file is not found, initializing an empty dictionary.

        employee_data = {  # Creating a dictionary containing employee data.
            "name": self.name,
            "employee_id": self.employee_id,
            "department": self.department,
            "job_title": self.job_title,
            "basic_salary": self.basic_salary,
            "age": self.age,
            "dob": self.dob,
            "passport_details": self.passport_details,
            "manager_id": self.manager_id
        }

        existing_data[self.employee_id] = employee_data  # Adding employee data to the existing data dictionary.
        data_storage.write_data("employees.pkl", existing_data)  # Writing the updated data back to the file.

    @staticmethod
    def read_employee_data():  # Defining a static method to read employee data from the database.
        try:
            return data_storage.read_data("employees.pkl")  # Trying to read employee data from a file.
        except FileNotFoundError:  # Handling the case where the file is not found.
            messagebox.showerror("Error", "No employee data found.")  # Displaying an error message if no data is found.
            return {}  # Returning an empty dictionary.

    @staticmethod
    def write_employee_data(data):  # Defining a static method to write employee data to the database.
        data_storage.write_data("employees.pkl", data)  # Writing employee data to a file.



#Event.py file
import data_storage  # Importing a module data_storage which handles data storage operations.
from tkinter import messagebox  # Import module of messagebox from tkinter


class Event:  # Defining a class named 'Event' to represent event data.
    def __init__(self, event_id, event_type, theme, date, time, duration, venue_address, client_id, guest_list,
                 company_supplier, invoice):
        # Initializing attributes of Event class with provided values.
        self.event_id = event_id  # event ID.
        self.event_type = event_type  # type of event.
        self.theme = theme  # theme of event.
        self.date = date  # date of event.
        self.time = time  # time of event.
        self.duration = duration  # duration of event.
        self.venue_address = venue_address  # address of event venue.
        self.client_id = client_id  # ID of client associated with event.
        self.guest_list = guest_list  # guest list for event.
        self.company_supplier = company_supplier  # company supplier for event.
        self.invoice = invoice  # invoice details for event.

    def add_to_database(self):  # Defining a method to add event data to a database.
        try:
            existing_data = data_storage.read_data("events.pkl")  # Trying to read existing data from a file.
        except FileNotFoundError:  # Handling case where file is not found.
            existing_data = {}  # If file is not found, initializing an empty dictionary.

        event_data = {  # Creating a dictionary containing event data.
            "event_id": self.event_id,
            "event_type": self.event_type,
            "theme": self.theme,
            "date": self.date,
            "time": self.time,
            "duration": self.duration,
            "venue_address": self.venue_address,
            "client_id": self.client_id,
            "guest_list": self.guest_list,
            "company_supplier": self.company_supplier,
            "invoice": self.invoice
        }

        existing_data[self.event_id] = event_data  # Adding event data to existing data dictionary.
        data_storage.write_data("events.pkl", existing_data)  # Writing updated data back to file.

    @staticmethod
    def read_event_data():  # Defining a static method to read event data from database.
        try:
            return data_storage.read_data("events.pkl")  # Trying to read event data from a file.
        except FileNotFoundError:  # Handling case where file is not found.
            messagebox.showerror("Error", "No event data found.")  # Displaying an error message if no data is found.
            return {}  # Returning an empty dictionary.

    @staticmethod
    def write_event_data(data):  # Defining a static method to write event data to database.
        data_storage.write_data("events.pkl", data)  # Writing event data to a file.



#Client.py file
import data_storage  # Importing a module data_storage which handles data storage operations.

from tkinter import messagebox  # Import module of messagebox from tkinter


class Client:  # Defining a class named 'Client' to represent client data.
    def __init__(self, client_id, name, address, contact_details, budget):
        # Initializing the attributes of the Client class with values.
        self.client_id = client_id  # client ID.
        self.name = name  # client name.
        self.address = address  # client address.
        self.contact_details = contact_details  # client contact details.
        self.budget = budget  # client budget.

    def add_to_database(self):  # Defining a method to add client data to a database.
        try: \
                existing_data = data_storage.read_data("clients.pkl")  # Trying to read existing data from a file.
        except FileNotFoundError:  # Handling the case where the file is not found.
            existing_data = {}  # If file is not found, initializing an empty dictionary.

        client_data = {  # Creating a dictionary containing client data.
            "client_id": self.client_id,
            "name": self.name,
            "address": self.address,
            "contact_details": self.contact_details,
            "budget": self.budget
        }

        existing_data[self.client_id] = client_data  # Adding client data to the existing data dictionary.
        data_storage.write_data("clients.pkl", existing_data)  # Writing the updated data back to the file.

    @staticmethod
    def read_client_data():  # Defining a static method to read client data from the database.
        try:
            return data_storage.read_data("clients.pkl")  # Trying to read client data from a file.
        except FileNotFoundError:  # Handling the case where the file is not found.
            messagebox.showerror("Error", "No client data found.")  # Displaying an error message if no data is found.
            return {}  # Returning an empty dictionary.

    @staticmethod
    def write_client_data(data):  # Defining a static method to write client data to the database.
        data_storage.write_data("clients.pkl", data)  # Writing client data to a file.



#Venue.py file
import data_storage  # Importing a module data_storage which handles data storage operations.
from tkinter import messagebox  # Import module of messagebox from tkinter


class Venue:  # Defining a class Venue to represent venue data.
    def __init__(self, venue_id, name, address, contact, min_guests, max_guests):
        # Initializing the attributes of the Venue class with provided values.
        self.venue_id = venue_id  # venue ID.
        self.name = name  # venue name.
        self.address = address  # venue address.
        self.contact = contact  # venue contact details.
        self.min_guests = min_guests  # minimum number of guests 
        self.max_guests = max_guests  # maximum number of guests 

    def add_to_database(self):  # Defining a method to add venue data to a database.
        try:
            existing_data = data_storage.read_data("venues.pkl")  # Trying to read existing data from a file.
        except FileNotFoundError:  # Handling the case where the file is not found.
            existing_data = {}  # If file is not found, initializing an empty dictionary.

        venue_data = {  # Creating a dictionary containing venue data.
            "venue_id": self.venue_id,
            "name": self.name,
            "address": self.address,
            "contact": self.contact,
            "min_guests": self.min_guests,
            "max_guests": self.max_guests
        }

        existing_data[self.venue_id] = venue_data  # Adding venue data to the existing data dictionary.
        data_storage.write_data("venues.pkl", existing_data)  # Writing the updated data back to the file.

    @staticmethod
    def read_venue_data():  # Defining a static method to read venue data from the database.
        try:
            return data_storage.read_data("venues.pkl")  # Trying to read venue data from a file.
        except FileNotFoundError:  # Handling the case where the file is not found.
            messagebox.showerror("Error", "No venue data found.")  # Displaying an error message if no data is found.
            return {}  # Returning an empty dictionary.

    @staticmethod
    def write_venue_data(data):  # Defining a static method to write venue data to the database.
        data_storage.write_data("venues.pkl", data)  # Writing venue data to a file.



#Supplier.py file
import data_storage  # Importing a module data_storage which handles data storage operations.
from tkinter import messagebox  # Import module of messagebox from tkinter


class Supplier:  # Defining a class Supplier to represent supplier data.
    def __init__(self, supplier_id, name, address, contact_details, menu=None, min_guests=None, max_guests=None):
        # Initializing the attributes of the Supplier class with provided values.
        self.supplier_id = supplier_id  # supplier ID.
        self.name = name  # supplier name.
        self.address = address  # supplier address.
        self.contact_details = contact_details  # supplier contact details.
        self.menu = menu  # supplier menu 
        self.min_guests = min_guests  # minimum number of guests 
        self.max_guests = max_guests  # maximum number of guests 

    def add_to_database(self):  # Defining a method to add supplier data to a database.
        try:
            existing_data = data_storage.read_data("suppliers.pkl")  # Trying to read existing data from a file.
        except FileNotFoundError:  # Handling the case where the file is not found.
            existing_data = {}  # If file is not found, initializing an empty dictionary.

        supplier_data = {  # Creating a dictionary containing supplier data.
            "supplier_id": self.supplier_id,
            "name": self.name,
            "address": self.address,
            "contact_details": self.contact_details,
            "menu": self.menu,
            "min_guests": self.min_guests,
            "max_guests": self.max_guests
        }

        existing_data[self.supplier_id] = supplier_data  # Adding supplier data to the existing data dictionary.
        data_storage.write_data("suppliers.pkl", existing_data)  # Writing the updated data back to the file.

    @staticmethod
    def read_supplier_data():  # Defining a static method to read supplier data from the database.
        try:
            return data_storage.read_data("suppliers.pkl")  # Trying to read supplier data from a file.
        except FileNotFoundError:  # Handling the case where the file is not found.
            messagebox.showerror("Error", "No supplier data found.")  # Displaying an error message if no data is found.
            return {}  # Returning an empty dictionary.

    @staticmethod
    def write_supplier_data(data):  # Defining a static method to write supplier data to the database.
        data_storage.write_data("suppliers.pkl", data)  # Writing supplier data to a file.



#Guest.py file
import data_storage  # Importing a module data_storage which handles data storage operations.
from tkinter import messagebox  # Import module of messagebox from tkinter


class Guest:  # Defining a class Guest to represent guest data.
    def __init__(self, guest_id, name, address, contact_details):
        # Initializing the attributes of the Guest class with provided values.
        self.guest_id = guest_id  # guest ID.
        self.name = name  # guest name.
        self.address = address  # guest address.
        self.contact_details = contact_details  # guest contact details.

    def add_to_database(self):  # Defining a method to add guest data to a database.
        try:
            existing_data = data_storage.read_data("guests.pkl")  # Trying to read existing data from a file.
        except FileNotFoundError:  # Handling the case where the file is not found.
            existing_data = {}  # If file is not found, initializing an empty dictionary.

        guest_data = {  # Creating a dictionary containing guest data.
            "guest_id": self.guest_id,
            "name": self.name,
            "address": self.address,
            "contact_details": self.contact_details
        }

        existing_data[self.guest_id] = guest_data  # Adding guest data to the existing data dictionary.
        data_storage.write_data("guests.pkl", existing_data)  # Writing the updated data back to the file.

    @staticmethod
    def read_guest_data():  # Defining a static method to read guest data from the database.
        try:
            return data_storage.read_data("guests.pkl")  # Trying to read guest data from a file.
        except FileNotFoundError:  # Handling the case where the file is not found.
            messagebox.showerror("Error", "No guest data found.")  # Displaying an error message if no data is found.
            return {}  # Returning an empty dictionary.

    @staticmethod
    def write_guest_data(data):  # Defining a static method to write guest data to the database.
        data_storage.write_data("guests.pkl", data)  # Writing guest data to a file.





