Java Programming Tasks & Instructions — CodeAlpha

TASK 1: Student Grade Tracker
● Build a Java program to input and manage student grades.
● Calculate average, highest, and lowest scores.
● Use arrays or ArrayLists to store and manage data.
● Display a summary report of all students.
● Make the interface console-based or GUI-based as desired.

Source Code:
import java.util.ArrayList;
import java.util.Scanner;

class Student {
    String name;
    double grade;

    Student(String name, double grade) {
        this.name = name;
        this.grade = grade;
    }
}

public class StudentGradeTracker {

    static ArrayList<Student> students = new ArrayList<>();
    static Scanner scanner = new Scanner(System.in);

    public static void main(String[] args) {
        int choice;

        do {
            System.out.println("\n==== Student Grade Tracker ====");
            System.out.println("1. Add Student Grade");
            System.out.println("2. View Summary Report");
            System.out.println("3. Exit");
            System.out.print("Enter your choice: ");
            choice = scanner.nextInt();
            scanner.nextLine(); // Clear newline

            switch (choice) {
                case 1 -> addStudent();
                case 2 -> showReport();
                case 3 -> System.out.println("Exiting... Goodbye!");
                default -> System.out.println("Invalid choice. Try again.");
            }
        } while (choice != 3);
    }

    public static void addStudent() {
        System.out.print("Enter student name: ");
        String name = scanner.nextLine();
        System.out.print("Enter student grade: ");
        double grade = scanner.nextDouble();

        students.add(new Student(name, grade));
        System.out.println("Student added successfully!");
    }

    public static void showReport() {
        if (students.isEmpty()) {
            System.out.println("No student records found.");
            return;
        }

        double total = 0, max = Double.MIN_VALUE, min = Double.MAX_VALUE;
        String topStudent = "", lowStudent = "";

        System.out.println("\n--- Student Grades ---");
        for (Student s : students) {
            System.out.printf("%s: %.2f\n", s.name, s.grade);
            total += s.grade;

            if (s.grade > max) {
                max = s.grade;
                topStudent = s.name;
            }
            if (s.grade < min) {
                min = s.grade;
                lowStudent = s.name;
            }
        }

        double average = total / students.size();
        System.out.printf("\nAverage Score: %.2f\n", average);
        System.out.printf("Highest Score: %.2f (%s)\n", max, topStudent);
        System.out.printf("Lowest Score: %.2f (%s)\n", min, lowStudent);
    }
}

How to Run:

Save this code as StudentGradeTracker.java

Compile with: javac StudentGradeTracker.java

Run with: java StudentGradeTracker


TASK 2: Stock Trading Platform 

Simulate a basic stock trading environment.

Add features for market data display and buy/sell operations.

Allow users to track portfolio performance over time.

Use Object-Oriented Programming (OOP) to manage stocks, users and transactions.

Optionally, include file I/O or database to persist portfolio data.

source code:

public class Stock {
    String symbol;
    String name;
    double price;

    public Stock(String symbol, String name, double price) {
        this.symbol = symbol;
        this.name = name;
        this.price = price;
    }

    @Override
    public String toString() {
        return symbol + " - " + name + " @ $" + price;
    }
}
👤 User.java

import java.util.HashMap;

public class User {
    String username;
    double balance;
    HashMap<String, Integer> portfolio = new HashMap<>();

    public User(String username, double balance) {
        this.username = username;
        this.balance = balance;
    }

    public void buyStock(Stock stock, int quantity) {
        double totalCost = stock.price * quantity;
        if (balance >= totalCost) {
            balance -= totalCost;
            portfolio.put(stock.symbol, portfolio.getOrDefault(stock.symbol, 0) + quantity);
            System.out.println("Bought " + quantity + " of " + stock.symbol);
        } else {
            System.out.println("Insufficient funds.");
        }
    }

    public void sellStock(Stock stock, int quantity) {
        int owned = portfolio.getOrDefault(stock.symbol, 0);
        if (owned >= quantity) {
            balance += stock.price * quantity;
            portfolio.put(stock.symbol, owned - quantity);
            System.out.println("Sold " + quantity + " of " + stock.symbol);
        } else {
            System.out.println("You don’t own enough of this stock.");
        }
    }

    public void showPortfolio() {
        System.out.println("\n📊 Portfolio of " + username + ":");
        for (String symbol : portfolio.keySet()) {
            System.out.println(symbol + ": " + portfolio.get(symbol) + " shares");
        }
        System.out.printf("💰 Balance: $%.2f\n", balance);
    }
}
🚀 StockMarketApp.java

import java.util.*;

public class StockMarketApp {
    static Scanner scanner = new Scanner(System.in);
    static ArrayList<Stock> market = new ArrayList<>();
    static User currentUser;

    public static void main(String[] args) {
        setupMarket();
        login();

        int choice;
        do {
            System.out.println("\n📈 Stock Trading Menu:");
            System.out.println("1. View Market");
            System.out.println("2. Buy Stock");
            System.out.println("3. Sell Stock");
            System.out.println("4. View Portfolio");
            System.out.println("5. Exit");
            System.out.print("Your choice: ");
            choice = scanner.nextInt();
            scanner.nextLine();

            switch (choice) {
                case 1 -> showMarket();
                case 2 -> buyFlow();
                case 3 -> sellFlow();
                case 4 -> currentUser.showPortfolio();
                case 5 -> System.out.println("Exiting... Goodbye!");
                default -> System.out.println("Invalid option.");
            }
        } while (choice != 5);
    }

    static void setupMarket() {
        market.add(new Stock("AAPL", "Apple Inc.", 185.32));
        market.add(new Stock("GOOGL", "Alphabet Inc.", 2732.18));
        market.add(new Stock("TSLA", "Tesla Inc.", 731.23));
        market.add(new Stock("AMZN", "Amazon Inc.", 134.25));
    }

    static void login() {
        System.out.print("Enter username: ");
        String name = scanner.nextLine();
        currentUser = new User(name, 10000); // Initial balance
        System.out.println("Welcome, " + name + "! You have $10,000 to trade.");
    }

    static void showMarket() {
        System.out.println("\n📊 Market Stocks:");
        for (Stock s : market) {
            System.out.println(s);
        }
    }

    static Stock getStockBySymbol(String symbol) {
        for (Stock s : market) {
            if (s.symbol.equalsIgnoreCase(symbol)) return s;
        }
        return null;
    }

    static void buyFlow() {
        System.out.print("Enter stock symbol to buy: ");
        String symbol = scanner.nextLine();
        Stock stock = getStockBySymbol(symbol);

        if (stock == null) {
            System.out.println("Stock not found.");
            return;
        }

        System.out.print("Enter quantity: ");
        int qty = scanner.nextInt();
        currentUser.buyStock(stock, qty);
    }

    static void sellFlow() {
        System.out.print("Enter stock symbol to sell: ");
        String symbol = scanner.nextLine();
        Stock stock = getStockBySymbol(symbol);

        if (stock == null) {
            System.out.println("Stock not found.");
            return;
        }

        System.out.print("Enter quantity: ");
        int qty = scanner.nextInt();
        currentUser.sellStock(stock, qty);
    }
}
📁 Optional: File I/O for Saving Portfolio

Saving/loading user portfolio to a file (portfolio.txt)

Extending User with saveToFile() and loadFromFile() methods

🏁 How to Run:

Save files: Stock.java, User.java, StockMarketApp.java

Compile all: javac *.java

Run main class: java StockMarketApp

