# OBJECT-ORIENTED-PROGRAM-TEST
# CSC2105: Object-Oriented Programming — Test 1 

---

## Question 1 — Encapsulation in Daily Reality

**1. Explanation**  
Encapsulation means keeping data (variables) and methods (functions) that work on that data inside one class.  
It protects data from being changed directly from outside the class.  

**Difference from Data Hiding:**  
Encapsulation is about *grouping data and functions together*, while data hiding focuses on *restricting access* to that data using private or protected attributes.

**2. Real-life Example:**  
In my village, the water tank manager keeps the valve key to himself. Only he can open or close it to prevent wastage.  
This is like encapsulation — others can ask for water, but they cannot directly open the valve.

**UML (Text Form):**
```
+---------------------+
|   WaterTankManager  |
+---------------------+
| -__valve_status     |
+---------------------+
| +open_valve()       |
| +close_valve()      |
| +get_status()       |
+---------------------+
```

# Implementation of the WaterTankManager class using encapsulation

class WaterTankManager:
    def __init__(self):
        # Private variable: cannot be accessed directly outside the class
        self.__valve_status = "Closed"

    # Getter method to view the valve status
    def get_status(self):
        return self.__valve_status

    # Setter method to open or close the valve
    def set_valve(self, action):
        # Validation rule: Only allow 'Open' or 'Closed'
        if action in ["Open", "Closed"]:
            self.__valve_status = action
        else:
            print("Invalid action! Valve can only be 'Open' or 'Closed'.")

# --- Demonstration ---
tank = WaterTankManager()
print("Initial valve status:", tank.get_status())

tank.set_valve("Open")   # Valid action
print("After opening:", tank.get_status())

tank.set_valve("Half")   # Invalid action
print("After invalid input:", tank.get_status())

## Question 2 — Alpha MIS Simulation

**Encapsulation in Alpha MIS:**  
In a university system like Alpha MIS, encapsulation keeps student data (marks, fees, courses) secure inside classes.  
Only authorized functions (like register_course or pay_fees) can change the data, preventing unauthorized access.


# Class representing part of Alpha MIS: Course Registration

import time

class CourseRegistration:
    def __init__(self, student_name, faculty):
        self.student_name = student_name       # public
        self._faculty = faculty                # protected
        self.__credits_registered = 0          # private

    def add_course(self, credits):
        # Validation 1: Cannot register negative or zero credits
        if credits <= 0:
            print("Invalid: Credits must be positive.")
        # Validation 2: Maximum 21 credits per semester
        elif self.__credits_registered + credits > 21:
            print("Credit limit exceeded!")
        else:
            self.__credits_registered += credits
            print(f"{credits} credits added successfully.")

    def summary(self):
        print("Access Number: B30129")
        print("Faculty:", self._faculty)
        print("Timestamp:", time.ctime())
        print("Total Credits Registered:", self.__credits_registered)

# --- Demonstration ---
student = CourseRegistration("Kisambira Akram", "Computing")
student.add_course(15)    # Valid
student.add_course(10)    # Invalid (limit exceeded)
student.summary()

## Question 3 — Hostel Visitor Audit

**Explanation:**  
A digital visitor log helps hostels know who visited and when, improving accountability and safety.  
Encapsulation ensures that only valid visitor data is recorded, avoiding fake names or missing details.

# Class for Hostel Visitor Audit

import time

class HostelVisitorAudit:
    def __init__(self, hostel_name):
        self.hostel_name = hostel_name
        self.__latest_visitor = {}

    def record(self, student_id, visitor_name):
        # Validation: Names must have only letters and spaces
        if not visitor_name.replace(" ", "").isalpha():
            raise ValueError("Invalid name! Use letters and spaces only.")
        self.__latest_visitor = {
            "StudentID": student_id,
            "Visitor": visitor_name,
            "Time": time.ctime()
        }

    def update(self, student_id, new_visitor):
        try:
            self.record(student_id, new_visitor)
            print("Visitor record updated.")
        except ValueError as e:
            print("Error:", e)

    def show_line(self, student_id):
        if self.__latest_visitor.get("StudentID") == student_id:
            print(f"{student_id} | {self.hostel_name} | {self.__latest_visitor['Visitor']} | {self.__latest_visitor['Time']}")
        else:
            print("No record for that Student ID.")

# --- Demonstration ---
audit = HostelVisitorAudit("Agape Hostel")

# Valid input
audit.record("S12345", "Mary Jane")
audit.show_line("S12345")

# Invalid input
audit.update("S12345", "Mary123")  # Will raise validation error

## Question 4 — Creative Encapsulation Challenge

**System Chosen:** Mobile Money Agent  

Encapsulation helps maintain trust by hiding customer balances and allowing only controlled deposits or withdrawals.  
This prevents fraud and protects customers’ private information.

# Two classes showing encapsulation between MobileMoneyAccount and Agent

class MobileMoneyAccount:
    def __init__(self, owner, balance):
        self.owner = owner
        self.__balance = balance  # private balance

    def deposit(self, amount):
        if amount <= 0:
            print("Invalid deposit! Must be positive.")
        else:
            self.__balance += amount
            print("Deposit successful.")

    def withdraw(self, amount):
        # District rule: Cannot withdraw more than 500,000 UGX at once
        if amount > 500000:
            print("Withdrawal exceeds district limit (500,000 UGX).")
        elif amount > self.__balance:
            print("Insufficient balance.")
        else:
            self.__balance -= amount
            print("Withdrawal successful.")

    def get_balance(self):
        # Only show balance safely
        return self.__balance


class Agent:
    def __init__(self, name):
        self.name = name

    def help_customer(self, account, action, amount=0):
        if action == "deposit":
            account.deposit(amount)
        elif action == "withdraw":
            account.withdraw(amount)
        else:
            print("Unknown action.")


# --- Demonstration ---
customer = MobileMoneyAccount("David Bazanye", 100000)
agent = Agent("UCU Agent")

agent.help_customer(customer, "deposit", 200000)
agent.help_customer(customer, "withdraw", 600000)  # Invalid (limit)
print("Final Balance:", customer.get_balance())
