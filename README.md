#include <iostream>
#include <vector>
#include <string>
#include <ctime>
#include <cstdlib>
using namespace std;

struct MenuItem {
    string name;
    double price;
};

struct OrderItem {
    string name;
    double price;
    int quantity;
};

vector<MenuItem> menu = {
    {"Kebab", 180000},
    {"Ghormeh Sabzi", 150000},
    {"Fesenjan", 200000},
    {"Zereshk Polo", 170000},
    {"Baghali Polo", 160000},
    {"Dizi", 140000},
    {"Ash Reshteh", 120000},
    {"Mirza Ghasemi", 130000},
    {"Gheymeh", 150000},
    {"Tahchin", 180000}
};

vector<OrderItem> currentOrder;

void generateInvoice(); // Function declaration added before usage

void showMenu() {
    cout << "\n--- Menu ---\n";
    for (int i = 0; i < menu.size(); i++) {
        cout << i + 1 << ". " << menu[i].name << " - " << menu[i].price << " Toman\n";
    }
}

void addOrder() {
    int itemNumber, quantity;
    char more;
    do {
        showMenu();
        cout << "\nEnter item number to order: ";
        cin >> itemNumber;
        cout << "Enter quantity: ";
        cin >> quantity;

        if (itemNumber >= 1 && itemNumber <= menu.size()) {
            OrderItem order;
            order.name = menu[itemNumber - 1].name;
            order.price = menu[itemNumber - 1].price;
            order.quantity = quantity;
            currentOrder.push_back(order);
        } else {
            cout << "Invalid item number.\n";
        }

        cout << "Do you want to order more items? (y/n): ";
        cin >> more;
    } while (more == 'y' || more == 'Y');

    generateInvoice();
}

void generateInvoice() {
    cout << "\n=========== Amir & Taha Restaurant ===========\n";
    cout << "Invoice Date: ";

    time_t now = time(0);
    tm *ltm = localtime(&now);
    cout << 1900 + ltm->tm_year << "/"
         << 1 + ltm->tm_mon << "/"
         << ltm->tm_mday << endl;

    int trackingCode = rand() % 100000;
    cout << "Tracking Code: #" << trackingCode << endl;

    double total = 0.0;
    cout << "\nItems Ordered:\n";
    for (const auto& item : currentOrder) {
        cout << item.name << " x" << item.quantity
             << " = " << item.price * item.quantity << " Toman" << endl;
        total += item.price * item.quantity;
    }

    cout << "\nTotal Amount: " << total << " Toman\n";
    cout << "=============================================\n";
    currentOrder.clear();
}

int main() {
    int choice;
    do {
        cout << "\n--- RESTAURANT MANAGEMENT ---\n";
        cout << "1. Show Menu\n";
        cout << "2. Add Order\n";
        cout << "3. Show Orders (Invoice)\n";
        cout << "0. Exit\n";
        cout << "Choose an option: ";
        cin >> choice;

        switch (choice) {
            case 1:
                showMenu();
                break;
            case 2:
                addOrder();
                break;
            case 3:
                generateInvoice();
                break;
            case 0:
                cout << "Exiting...\n";
                break;
            default:
                cout << "Invalid choice.\n";
        }
    } while (choice != 0);

    return 0;
}
