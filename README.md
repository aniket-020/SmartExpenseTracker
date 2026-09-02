# SmartExpenseTracker
# 💰 Smart Expense Tracker

A console-based **Java + MySQL** application to register/login users, log daily expenses, categorize them, and view monthly spending summaries — built as a mini project applying core Java, JDBC, and layered (Model–Repository–Service) architecture.

## ✨ Features

- 🔐 **User Registration & Login** — create an account and sign in with email/password
- ➕ **Add Expense** — log an expense with amount, category, description, and date
- 📋 **View Expenses** — list all expenses for the logged-in user
- 📊 **Monthly Summary** — total amount spent in the current month
- 🗂️ **Category-wise Monthly Report** — spending broken down by category
- 🗄️ **MySQL-backed persistence** via JDBC

## 🛠️ Tech Stack

| Layer | Technology |
|---|---|
| Language | Java (JDK 17+ recommended) |
| Database | MySQL |
| DB Driver | MySQL Connector/J 9.6.0 |
| Architecture | Layered — `model` → `repository` → `service` → `app` |
| Interface | Console (CLI) |

## 📁 Project Structure

```
SmartExpenseTracker/
├── src/main/
│   ├── app/            # Entry points (MainApp, TestConnection)
│   ├── model/          # POJOs: User, Category, Expense
│   ├── repository/     # JDBC data-access classes
│   ├── service/        # Business logic
│   └── util/           # DBConnection (JDBC connection helper)
├── lib/
│   └── mysql-connector-j-9.6.0.jar   # MySQL JDBC driver
└── README.md
```

## ✅ Prerequisites

- JDK 17 or later
- MySQL Server (running locally or reachable over a network)
- A Java IDE (Eclipse / IntelliJ IDEA / VS Code) *or* the command line

## 🗄️ Database Setup

Create the database and required tables in MySQL before running the app:

```sql
CREATE DATABASE expense_tracker;
USE expense_tracker;

CREATE TABLE users (
    id INT AUTO_INCREMENT PRIMARY KEY,
    name VARCHAR(100) NOT NULL,
    email VARCHAR(100) UNIQUE NOT NULL,
    password VARCHAR(100) NOT NULL
);

CREATE TABLE categories (
    category_id INT AUTO_INCREMENT PRIMARY KEY,
    name VARCHAR(50) NOT NULL
);

CREATE TABLE expenses (
    id INT AUTO_INCREMENT PRIMARY KEY,
    user_id INT NOT NULL,
    amount DOUBLE NOT NULL,
    category_id INT NOT NULL,
    description VARCHAR(255),
    date DATE DEFAULT (CURRENT_DATE),
    FOREIGN KEY (user_id) REFERENCES users(id),
    FOREIGN KEY (category_id) REFERENCES categories(category_id)
);

-- Sample categories
INSERT INTO categories (name) VALUES ('Food'), ('Travel'), ('Shopping'), ('Bills'), ('Other');
```

## ⚙️ Configuration

Database credentials are set in `src/main/util/DBConnection.java`:

```java
private static final String URL = "jdbc:mysql://localhost:3306/expense_tracker";
private static final String USER = "root";
private static final String PASSWORD = "student";
```

Update `URL`, `USER`, and `PASSWORD` to match your local MySQL setup before running the app.

> ⚠️ **Security note:** Credentials are currently hardcoded for simplicity (this is a student/mini project). For any real-world or public deployment, move these into environment variables or a config file that is excluded from version control (see `.gitignore` below), and never commit real passwords.

## ▶️ How to Run

### Using an IDE (Eclipse / IntelliJ)
1. Import the project as a Java project.
2. Add `lib/mysql-connector-j-9.6.0.jar` to the project's build path / classpath.
3. Run `src/main/app/TestConnection.java` first to verify the database connection.
4. Run `src/main/app/MainApp.java` to start the application.

### Using the command line
```bash
# Compile
javac -cp "lib/mysql-connector-j-9.6.0.jar" -d out $(find src/main -name "*.java")

# Run (Windows uses ; instead of : as the classpath separator)
java -cp "out:lib/mysql-connector-j-9.6.0.jar" app.MainApp
```

## 🖥️ Usage

```
===== Smart Expense Tracker =====
1. Register
2. Login
3. Exit
```

After logging in, you'll land on the dashboard to add expenses, view them, or see your monthly summary.

## 🚧 Known Limitations / Ideas for Improvement

- Logged-in `userId` is currently hardcoded to `1` in `MainApp` after login — should be replaced with the actual authenticated user's ID.
- Passwords are stored in plain text — should be hashed (e.g., BCrypt) before storing.
- No input validation on menu choices beyond basic type checks.
- Could be extended with a GUI (JavaFX/Swing) or REST API + web frontend.

## 👤 Author

**Aniket Kumar Gupta**
📧 14aniketani@gmail.com

## 📄 License

This project is open for educational/personal use. Add a license of your choice (e.g., MIT) if you plan to make the repository public and want to define reuse terms explicitly.
