import mysql.connector

def connect_to_database():
    # Database connection details (replace with your actual credentials)
    db_config = {
        "host": "localhost",
        "user": "your_username",
        "password": "your_password",
        "database": "your_database"
    }

    connection = mysql.connector.connect(**db_config)
    return connection

def close_connection(connection):
    if connection:
        connection.close()

def execute_query(query, params=None):
    connection = connect_to_database()
    cursor = connection.cursor()
    cursor.execute(query, params)
    connection.commit()  # Assuming you want to commit changes (modify as needed)
    result = cursor.fetchall()  # Fetch all results (modify as needed)
    close_connection(connection)
    return result

# Example usage (replace with your specific queries)
def check_user_exists(username):
    query = "SELECT role FROM users WHERE user = %s"
    return execute_query(query, (username,))

def add_user(username, role, fname=None, lname=None, email=None):
    query = "INSERT INTO users (user, role, fname, lname, email) VALUES (%s, %s, %s, %s, %s)"
    params = (username, role, fname, lname, email)
    execute_query(query, params)

def get_user_role(username):
    query = "SELECT role FROM users WHERE user = %s"
    result = execute_query(query, (username,))
    if result:
        return result[0][0]  # Assuming role is the first column in the first row
    else:
        return None
....................................
from flask import Flask, request
from database_utils import check_user_exists, add_user, get_user_role

app = Flask(__name__)

# ... your Flask routes and logic here ...

@app.route('/check_user', methods=['POST'])
def check_user_handler():
    username = request.json.get('username')

    if not check_user_exists(username):
        return {
            "message": "User not found"
        }

    user_data = check_user_exists(username)
    user_role = user_data[0][0]  # Assuming role is the first column in the first row

    return {
        "message": "User exists",
        "role": user_role
    }

@app.route('/get_user_role', methods=['POST'])
def get_user_role_handler():
    username = request.json.get('username')

    role = get_user_role(username)

    if role:
        return {
            "message": "User role retrieved successfully",
            "role": role
        }
    else:
        return {
            "message": "User not found or role not available"
        }

# ... other routes and logic ...

if __name__ == '__main__':
    app.run(debug=True)
