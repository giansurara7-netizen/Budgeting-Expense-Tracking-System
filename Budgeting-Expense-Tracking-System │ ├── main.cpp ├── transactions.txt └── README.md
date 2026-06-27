#include <iostream>
#include <fstream>
#include <vector>
#include <iomanip>
using namespace std;

struct Transaction {
    string type;
    string category;
    string description;
    double amount;
};

vector<Transaction> records;

void saveToFile() {
    ofstream file("transactions.txt");

    for (auto t : records) {
        file << t.type << "|"
             << t.category << "|"
             << t.description << "|"
             << t.amount << endl;
    }

    file.close();
}

void loadFromFile() {
    ifstream file("transactions.txt");

    if (!file)
        return;

    Transaction t;
    string amount;

    while(getline(file, t.type, '|')) {
        getline(file, t.category, '|');
        getline(file, t.description, '|');
        getline(file, amount);

        t.amount = stod(amount);

        records.push_back(t);
    }

    file.close();
}

void addTransaction() {
    Transaction t;

    cout << "\nType (Income/Expense): ";
    cin >> t.type;

    cout << "Category: ";
    cin >> t.category;

    cout << "Description: ";
    cin.ignore();
    getline(cin, t.description);

    cout << "Amount: ";
    cin >> t.amount;

    records.push_back(t);

    saveToFile();

    cout << "\nTransaction Added!\n";
}

void viewTransactions() {

    cout << "\n========== TRANSACTIONS ==========\n";

    for(int i = 0; i < records.size(); i++) {

        cout << i+1 << ". "
             << records[i].type
             << " | "
             << records[i].category
             << " | "
             << records[i].description
             << " | ₱"
             << fixed << setprecision(2)
             << records[i].amount
             << endl;
    }
}

void showBalance() {

    double income = 0;
    double expense = 0;

    for(auto t : records) {

        if(t.type == "Income")
            income += t.amount;

        else if(t.type == "Expense")
            expense += t.amount;
    }

    cout << "\n========== SUMMARY ==========\n";
    cout << "Total Income : ₱"
         << income << endl;

    cout << "Total Expense: ₱"
         << expense << endl;

    cout << "Balance      : ₱"
         << income-expense << endl;
}

void categoryReport(){

    string category;
    double total = 0;

    cout << "\nEnter category: ";
    cin >> category;


    for(auto t: records){

        if(t.category == category &&
           t.type == "Expense")
        {
            total += t.amount;
        }
    }


    cout << "Spent on "
         << category
         << ": ₱"
         << total << endl;
}


int main(){

    loadFromFile();

    int choice;

    do{

        cout << "\n==============================";
        cout << "\n  BUDGET & EXPENSE TRACKER";
        cout << "\n==============================";

        cout << "\n1. Add Transaction";
        cout << "\n2. View Transactions";
        cout << "\n3. Show Balance";
        cout << "\n4. Category Report";
        cout << "\n5. Exit";

        cout << "\n\nChoose: ";
        cin >> choice;


        switch(choice){

            case 1:
                addTransaction();
                break;

            case 2:
                viewTransactions();
                break;

            case 3:
                showBalance();
                break;

            case 4:
                categoryReport();
                break;

            case 5:
                cout << "Thank you!";
                break;

            default:
                cout << "Invalid choice!";
        }

    }while(choice != 5);


    return 0;
}
