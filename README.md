import sqlite3

# Database connection
connection = sqlite3.connect("students.db")
cursor = connection.cursor()


# Create table
cursor.execute("""
CREATE TABLE IF NOT EXISTS students (
    student_id INTEGER PRIMARY KEY AUTOINCREMENT,
    name TEXT NOT NULL,
    registration_number TEXT UNIQUE NOT NULL,
    father_name TEXT NOT NULL,
    mother_name TEXT NOT NULL,
    age INTEGER,
    department TEXT,
    phone TEXT,
    email TEXT
)
""")

connection.commit()


# Add student
def add_student():
    name = input("Enter student name: ")
    registration_number = input("Enter registration number: ")
    father_name = input("Enter father name: ")
    mother_name = input("Enter mother name: ")
    age = int(input("Enter age: "))
    department = input("Enter department: ")
    phone = input("Enter phone number: ")
    email = input("Enter email: ")

    try:
        cursor.execute("""
        INSERT INTO students
        (name, registration_number, father_name, mother_name,
         age, department, phone, email)
        VALUES (?, ?, ?, ?, ?, ?, ?, ?)
        """,
        (name, registration_number, father_name, mother_name,
         age, department, phone, email))

        connection.commit()
        print("Student added successfully!")

    except sqlite3.IntegrityError:
        print("Registration number already exists!")


# View students
def view_students():
    cursor.execute("SELECT * FROM students")
    students = cursor.fetchall()

    if not students:
        print("No students found.")
    else:
        for student in students:
            print("\n-----------------------------")
            print("Student ID:", student[0])
            print("Name:", student[1])
            print("Registration Number:", student[2])
            print("Father Name:", student[3])
            print("Mother Name:", student[4])
            print("Age:", student[5])
            print("Department:", student[6])
            print("Phone:", student[7])
            print("Email:", student[8])


# Search student
def search_student():
    registration_number = input("Enter registration number: ")

    cursor.execute(
        "SELECT * FROM students WHERE registration_number = ?",
        (registration_number,)
    )

    student = cursor.fetchone()

    if student:
        print("\n===== Student Details =====")
        print("Student ID:", student[0])
        print("Name:", student[1])
        print("Registration Number:", student[2])
        print("Father Name:", student[3])
        print("Mother Name:", student[4])
        print("Age:", student[5])
        print("Department:", student[6])
        print("Phone:", student[7])
        print("Email:", student[8])
    else:
        print("Student not found.")


# Update student
def update_student():
    registration_number = input("Enter registration number: ")

    cursor.execute(
        "SELECT * FROM students WHERE registration_number = ?",
        (registration_number,)
    )

    student = cursor.fetchone()

    if not student:
        print("Student not found.")
        return

    print("\n===== Update Student =====")
    print("1. Name")
    print("2. Father Name")
    print("3. Mother Name")
    print("4. Age")
    print("5. Department")
    print("6. Phone")
    print("7. Email")
    print("8. Cancel")

    choice = input("Enter what you want to update: ")

    if choice == "1":
        new_value = input("Enter new name: ")

        cursor.execute(
            "UPDATE students SET name = ? WHERE registration_number = ?",
            (new_value, registration_number)
        )

    elif choice == "2":
        new_value = input("Enter new father name: ")

        cursor.execute(
            "UPDATE students SET father_name = ? WHERE registration_number = ?",
            (new_value, registration_number)
        )

    elif choice == "3":
        new_value = input("Enter new mother name: ")

        cursor.execute(
            "UPDATE students SET mother_name = ? WHERE registration_number = ?",
            (new_value, registration_number)
        )

    elif choice == "4":
        new_value = int(input("Enter new age: "))

        cursor.execute(
            "UPDATE students SET age = ? WHERE registration_number = ?",
            (new_value, registration_number)
        )

    elif choice == "5":
        new_value = input("Enter new department: ")

        cursor.execute(
            "UPDATE students SET department = ? WHERE registration_number = ?",
            (new_value, registration_number)
        )

    elif choice == "6":
        new_value = input("Enter new phone: ")

        cursor.execute(
            "UPDATE students SET phone = ? WHERE registration_number = ?",
            (new_value, registration_number)
        )

    elif choice == "7":
        new_value = input("Enter new email: ")

        cursor.execute(
            "UPDATE students SET email = ? WHERE registration_number = ?",
            (new_value, registration_number)
        )

    elif choice == "8":
        print("Update cancelled.")
        return

    else:
        print("Invalid choice.")
        return

    connection.commit()
    print("Student information updated successfully!")


# Delete student
def delete_student():
    registration_number = input("Enter registration number: ")

    cursor.execute(
        "DELETE FROM students WHERE registration_number = ?",
        (registration_number,)
    )

    connection.commit()

    if cursor.rowcount > 0:
        print("Student deleted successfully!")
    else:
        print("Student not found.")


# Main menu
while True:
    print("\n===== Student Management System =====")
    print("1. Add Student")
    print("2. View Students")
    print("3. Search Student")
    print("4. Update Student")
    print("5. Delete Student")
    print("6. Exit")

    choice = input("Enter your choice: ")

    if choice == "1":
        add_student()

    elif choice == "2":
        view_students()

    elif choice == "3":
        search_student()

    elif choice == "4":
        update_student()

    elif choice == "5":
        delete_student()

    elif choice == "6":
        print("Thank you!")
        break

    else:
        print("Invalid choice.")


# Close database connection
connection.close()
