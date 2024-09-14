# Sahil-Program-CPP_TLS24
program managemen keuangan 
#include <iostream>
#include <iomanip>
#include <vector>
#include <string>
#include <limits>  // Untuk numeric_limits

using namespace std;

class Transaction {
private:
    string date;
    string description;
    string type; // "Income" atau "Expense"
    double amount;

public:
    // Constructor
    Transaction(const string& date, const string& description, const string& type, double amount)
        : date(date), description(description), type(type), amount(amount) {}

    // Getter dan Setter
    const string& getDate() const {
        return date;
    }

    void setDate(const string& newDate) {
        date = newDate;
    }
    const string& getDescription() const {
        return description;
    }
    void setDescription(const string& newDescription) {
        description = newDescription;
    }
    const string& getType() const {
        return type;
    }
    void setType(const string& newType) {
        type = newType;
    }
    double getAmount() const {
        return amount;
    }
    void setAmount(double newAmount) {
        amount = newAmount;
    }
};

class FinancialManager {
private:
    vector<Transaction*> transactions;
    double totalIncome;
    double totalExpense;

public:
    FinancialManager() : totalIncome(0.0), totalExpense(0.0) {}

    void addTransaction() {
        string date, description, type;
        double amount;

        cout << "Masukkan tanggal transaksi (YYYY-MM-DD): ";
        cin >> date;
        cin.ignore(numeric_limits<streamsize>::max(), '\n');
        cout << "Masukkan keterangan transaksi: ";
        getline(cin, description);
        cout << "Masukkan jenis transaksi (Income/Expense): ";
        cin >> type;

        while (type != "Income" && type != "Expense") {
            cout << "Jenis transaksi tidak valid. Masukkan kembali (Income/Expense): ";
            cin >> type;
        }

        cout << "Masukkan jumlah transaksi: $";
        cin >> amount;

        Transaction* newTransaction = new Transaction(date, description, type, amount);
        transactions.push_back(newTransaction);

        if (type == "Income") {
            totalIncome += amount;
        } else if (type == "Expense") {
            totalExpense += amount;
        }

        cout << "Transaksi berhasil ditambahkan." << endl;
    }

    void displayTransactions() {
        if (transactions.empty()) {
            cout << "Belum ada transaksi." << endl;
            return;
        }

        cout << "=== Daftar Transaksi ===" << endl;
        for (const auto& transaction : transactions) {
            cout << "Tanggal: " << transaction->getDate() << "\t";
            cout << "Keterangan: " << transaction->getDescription() << "\t";
            cout << "Jenis: " << transaction->getType() << "\t";
            cout << "Jumlah: $" << transaction->getAmount() << endl;
        }

        cout << "Total Pemasukan: $" << totalIncome << "\t";
        cout << "Total Pengeluaran: $" << totalExpense << endl;
        cout << "Saldo Akhir: $" << (totalIncome - totalExpense) << endl;
    }
    void generateMonthlyReport() {
        string month;
        cout << "Masukkan bulan (MM): ";
        cin >> month;

        // Validasi input bulan
        while (month.length() != 2 || !isdigit(month[0]) || !isdigit(month[1])) {
            cout << "Format bulan tidak valid. Masukkan bulan (MM): ";
            cin >> month;
        }

        double monthlyIncome = 0.0;
        double monthlyExpense = 0.0;

        cout << "=== Laporan Keuangan Bulan " << month << " ===" << endl;

        for (auto it = transactions.begin(); it != transactions.end(); ++it) {
            if ((*it)->getDate().substr(5, 2) == month) {
                cout << "Tanggal: " << (*it)->getDate() << "\t";
                cout << "Keterangan: " << (*it)->getDescription() << "\t";
                cout << "Jenis: " << (*it)->getType() << "\t";
                cout << "Jumlah: $" << (*it)->getAmount() << endl;

                if ((*it)->getType() == "Income") {
                    monthlyIncome += (*it)->getAmount();
                } else if ((*it)->getType() == "Expense") {
                    monthlyExpense += (*it)->getAmount();
                }
            }
        }

        cout << "\nTotal Pemasukan Bulan " << month << ": $" << monthlyIncome << "\t";
        cout << "Total Pengeluaran Bulan " << month << ": $" << monthlyExpense << endl;
        cout << "Saldo Akhir Bulan " << month << ": $" << (monthlyIncome - monthlyExpense) << endl;
    }

    // Destruktor untuk membersihkan memori yang dialokasikan
    ~FinancialManager() {
        for (const auto& transaction : transactions) {
            delete transaction;
        }
        transactions.clear();
    }
};

int main() {
    FinancialManager manager;
    int choice;

    do {
        cout << "\n=== Aplikasi Manajemen Keuangan ===" << endl;
        cout << "1. Tambah Transaksi" << endl;
        cout << "2. Lihat Daftar Transaksi" << endl;
        cout << "3. Laporan Keuangan Bulanan" << endl;
        cout << "4. Keluar" << endl;
        cout << "Pilihan Anda: ";
        cin >> choice;

        switch (choice) {
            case 1:
                manager.addTransaction();
                break;
            case 2:
                manager.displayTransactions();
                break;
            case 3:
                manager.generateMonthlyReport();
                break;
            case 4:
                cout << "Terima kasih telah menggunakan aplikasi manajemen keuangan." << endl;
                break;
            default:
                cout << "Pilihan tidak valid. Silakan coba lagi." << endl;
        }

    } while (choice != 4);

    return 0;
}
