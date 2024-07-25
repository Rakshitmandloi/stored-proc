Python
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
    query = "SELECT EXISTS(SELECT * FROM users WHERE user = %s)"
    return execute_query(query, (username,))

def add_user(username, role, fname=None, lname=None, email=None):
    query = "INSERT INTO users (user, role, fname, lname, email) VALUES (%s, %s, %s, %s, %s)"
    params = (username, role, fname, lname, email)
    execute_query(query, params)
Use code with caution.

Explanation:

This file (database_utils.py) defines functions for connecting to the database, executing queries, and closing connections.
The connect_to_database function establishes a connection and returns it.
The close_connection function closes the connection if it's open.
The execute_query function takes a query string and optional parameters, executes the query, commits changes (modify as needed), fetches results (modify as needed), and closes the connection.
Example functions check_user_exists and add_user demonstrate how to use execute_query for specific tasks.
2. server.py (your Flask app):

Python
from flask import Flask, request
from database_utils import check_user_exists, add_user  # Import functions from database_utils

app = Flask(__name__)

# ... your Flask routes and logic here ...

@app.route('/add_user', methods=['POST'])
def add_user_handler():
    username = request.json.get('username')
    role = request.json.get('role')
    fname = request.json.get('fname')
    lname = request.json.get('lname')
    email = request.json.get('email')

    if check_user_exists(username):
        return {
            "message": "User already exists"
        }
    else:
        add_user(username, role, fname, lname, email)
        return {
            "message": "User added successfully"
        }

# ... other routes and logic ...

if __name__ == '__main__':
    app.run(debug=True)
