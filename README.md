import java.util.ArrayList;
import java.util.List;
import java.util.Scanner;

public class ATMTransactionSimulator {
    private double balance;
    private int pin;
    private List<String> transactionHistory;

    public ATMTransactionSimulator(double initialBalance, int initialPin) {
        this.balance = initialBalance;
        this.pin = initialPin;
        this.transactionHistory = new ArrayList<>();
    }

    public void showMenu() {
        Scanner scanner = new Scanner(System.in);
        
        // Authenticate the user before accessing the ATM
        if (!authenticate(scanner)) {
            System.out.println("Authentication failed. Exiting...");
            return;
        }

        int choice;
        do {
            System.out.println("\n=== ATM Menu ===");
            System.out.println("1. Check Balance");
            System.out.println("2. Deposit Money");
            System.out.println("3. Withdraw Money");
            System.out.println("4. Change PIN");
            System.out.println("5. Transaction History");
            System.out.println("6. Exit");
            System.out.print("Enter your choice: ");
            choice = scanner.nextInt();

            switch (choice) {
                case 1:
                    checkBalance();
                    break;
                case 2:
                    depositMoney(scanner);
                    break;
                case 3:
                    withdrawMoney(scanner);
                    break;
                case 4:
                    changePin(scanner);
                    break;
                case 5:
                    showTransactionHistory();
                    break;
                case 6:
                    System.out.println("Exiting... Thank you for using our ATM.");
                    break;
                default:
                    System.out.println("Invalid choice. Please try again.");
            }
        } while (choice != 6);

        scanner.close();
    }

    private boolean authenticate(Scanner scanner) {
        System.out.print("Enter your PIN to access the ATM: ");
        int enteredPin = scanner.nextInt();

        if (enteredPin == pin) {
            System.out.println("Authentication successful.");
            return true;
        } else {
            System.out.println("Incorrect PIN. Try again.");
            return false;
        }
    }

    private void checkBalance() {
        System.out.println("Your current balance is: $" + balance);
        transactionHistory.add("Checked Balance: $" + balance);
    }

    private void depositMoney(Scanner scanner) {
        System.out.print("Enter amount to deposit: ");
        double amount = scanner.nextDouble();
        if (amount > 0) {
            balance += amount;
            System.out.println("Successfully deposited $" + amount);
            transactionHistory.add("Deposited: $" + amount);
        } else {
            System.out.println("Invalid amount. Please enter a positive number.");
        }
    }

    private void withdrawMoney(Scanner scanner) {
        System.out.print("Enter amount to withdraw: ");
        double amount = scanner.nextDouble();
        if (amount > 0 && amount <= balance) {
            balance -= amount;
            System.out.println("Successfully withdrew $" + amount);
            transactionHistory.add("Withdrew: $" + amount);
        } else if (amount > balance) {
            System.out.println("Insufficient balance.");
        } else {
            System.out.println("Invalid amount. Please enter a positive number.");
        }
    }

    private void changePin(Scanner scanner) {
        System.out.print("Enter your current PIN: ");
        int currentPin = scanner.nextInt();

        if (currentPin == pin) {
            System.out.print("Enter new PIN: ");
            int newPin = scanner.nextInt();
            pin = newPin;
            System.out.println("PIN changed successfully.");
            transactionHistory.add("Changed PIN.");
        } else {
            System.out.println("Incorrect current PIN. Cannot change PIN.");
        }
    }

    private void showTransactionHistory() {
        if (transactionHistory.isEmpty()) {
            System.out.println("No transactions found.");
        } else {
            System.out.println("\nTransaction History:");
            for (String transaction : transactionHistory) {
                System.out.println(transaction);
            }
        }
    }

    public static void main(String[] args) {
        ATMTransactionSimulator atm = new ATMTransactionSimulator(1000.0, 1234); // Initial balance & PIN
        atm.showMenu();
    }
}
