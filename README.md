package ATMMachine;
import java.util.ArrayList;
import java.util.Scanner;

class Transaction {
    private String type;
    private float amount;

    public Transaction(String type, float amount) {
        this.type = type;
        this.amount = amount;
    }

    public String getType() {
        return type;
    }

    public float getAmount() {
        return amount;
    }

    @Override
    public String toString() {
        return "Type: " + type + ", Amount: " + amount;
    }
}

class ATM {
    private float balance;
    private final int PIN = 2003;
    private ArrayList<Transaction> transactions;

    public ATM() {
        balance = 0;
        transactions = new ArrayList<>();
    }

    public void checkPin() {
        Scanner scanner = new Scanner(System.in);
        System.out.println("Enter your PIN: ");
        int enteredPin = scanner.nextInt();
        if (enteredPin == PIN) {
            menu();
        } else {
            System.out.println("Invalid PIN. Please try again.");
            checkPin();
        }
    }

    private void menu() {
        Scanner scanner = new Scanner(System.in);
        System.out.println("\n==== ATM Menu for OASIS INFOBYTE====");
        System.out.println("1. Check Balance");
        System.out.println("2. Withdraw Money");
        System.out.println("3. Deposit Money");
        System.out.println("4. View Transaction History");
        System.out.println("5. Transfer Money");
        System.out.println("6. Exit");

        System.out.print("Enter your choice: ");
        int choice = scanner.nextInt();
        switch (choice) {
            case 1:
                checkBalance();
                break;
            case 2:
                withdrawMoney();
                break;
            case 3:
                depositMoney();
                break;
            case 4:
                viewTransactionHistory();
                break;
            case 5:
                transferMoney();
                break;
            case 6:
                System.out.println("Thank you for using the ATM. Goodbye!");
                break;
            default:
                System.out.println("Invalid choice. Please try again.");
                menu();
        }
    }

    private void checkBalance() {
        System.out.println("Your current balance: " + balance);
        menu();
    }

    private void withdrawMoney() {
        Scanner scanner = new Scanner(System.in);
        System.out.print("Enter the amount to withdraw: ");
        float amount = scanner.nextFloat();
        if (amount > balance) {
            System.out.println("Insufficient balance.");
        } else {
            balance -= amount;
            System.out.println(" " + amount + " withdrawn successfully.");
            transactions.add(new Transaction("Withdrawal", amount));
        }
        menu();
    }

    private void depositMoney() {
        Scanner scanner = new Scanner(System.in);
        System.out.print("Enter the amount to deposit: ");
        float amount = scanner.nextFloat();
        balance += amount;
        System.out.println(" " + amount + " deposited successfully.");
        transactions.add(new Transaction("Deposit", amount));
        menu();
    }

    private void viewTransactionHistory() {
        System.out.println("\n==== Transaction History ====");
        for (Transaction transaction : transactions) {
            System.out.println(transaction);
        }
        menu();
    }

    private void transferMoney() {
        Scanner scanner = new Scanner(System.in);
        System.out.print("Enter the amount to transfer: ");
        float amount = scanner.nextFloat();
        if (amount > balance) {
            System.out.println("Insufficient balance for transfer.");
        } else {
            balance -= amount;
            System.out.print("Enter the recipient's account number: ");
            String recipientAccount = scanner.next();
            System.out.println(" " + amount + " transferred to account " + recipientAccount + " successfully.");
            transactions.add(new Transaction("Transfer to " + recipientAccount, amount));
        }
        menu();
    }

    public void start() {
        checkPin();
    }
}

public class Main {
    public static void main(String[] args) {
        ATM atm = new ATM();
        atm.start();
    }
}

