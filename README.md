#include <iostream>
#include <fstream>
#include <string>
#include <iomanip>
#include <windows.h>
#include <cstdlib>

using namespace std;

struct voter {
    string firstName;
    string lastName;
    int cnic;
    string address;
};

class node {
public:
    voter data;
    node* next;

    node(voter value) {
        data = value;
        next = NULL;
    }
};

class voterlist {
public:
    node* head;

    voterlist() {
        head = NULL;
    }

    void insertnode(voter data) {
        node* new_node = new node(data);
        if (head == NULL) {
            head = new_node;
        }
        else {
            node* temp = head;
            while (temp->next != NULL) {
                temp = temp->next;
            }
            temp->next = new_node;
        }
    }

    void display() {
        node* temp = head;
        while (temp != NULL) {
            cout << "Name: " << temp->data.firstName << " " << temp->data.lastName;
            cout << "\tCNIC number: " << temp->data.cnic;
            cout << "\tAddress: " << temp->data.address;
            temp = temp->next;
            cout << endl;
        }
    }

    void saveToFile(const string& filename) {
        ofstream file(filename, ios::out);
        if (!file.is_open()) {
            cout << "Error opening file for writing." << endl;
            return;
        }

        node* temp = head;
        while (temp != NULL) {
            file << temp->data.firstName << "," << temp->data.lastName << ","
                << temp->data.cnic << "," << temp->data.address << endl;
            temp = temp->next;
        }

        file.close();
        cout << "Voter data has been saved to file." << endl;
    }

    void loadFromFile(const string& filename) {
        ifstream file(filename);
        if (!file.is_open()) {
            cout << "Error opening file for reading." << endl;
            return;
        }

        string line, firstName, lastName, address;
        int cnic;
        while (getline(file, line)) {
            size_t pos1 = line.find(",");
            size_t pos2 = line.find(",", pos1 + 1);
            size_t pos3 = line.find(",", pos2 + 1);

            firstName = line.substr(0, pos1);
            lastName = line.substr(pos1 + 1, pos2 - pos1 - 1);
            cnic = stoi(line.substr(pos2 + 1, pos3 - pos2 - 1));
            address = line.substr(pos3 + 1);

            voter data{ firstName, lastName, cnic, address };
            insertnode(data);
        }

        file.close();
        cout << "Voter data has been loaded from file." << endl;
    }
};

class VoterQueue {
public:
    struct Node {
        voter data;
        Node* next;

        Node(voter v) : data(v), next(nullptr) {}
    };

    Node* front;
    Node* rear;

    VoterQueue() {
        front = NULL;
        rear = NULL;
    }
    void enqueue(voter v) {
        Node* newNode = new Node(v);
        if (rear == nullptr) {
            front = rear = newNode;
        }
        else {
            rear->next = newNode;
            rear = newNode;
        }
    }

    voter dequeue() {
        if (front == nullptr) {
            cout<<"Queue is empty!";
        }

        Node* temp = front;
        voter v = temp->data;
        front = front->next;
        if (front == nullptr) {
            rear = nullptr;
        }
        delete temp;
        return v;
    }

    bool isEmpty() {
        return front == nullptr;
    }
};
int pti = 0, pml = 0, jui = 0, ppp = 0;

void getvoterdata(voterlist& list, VoterQueue& queue) {
    string firstName, lastName;
    int cnic;
    int age;
    string fullAddress;

    cin.ignore();  
    cout << "\nEnter your full name: ";
    cin >> firstName >> lastName;
    cout << "\nEnter your age for checking if you are eligible to vote or not: ";
    cin >> age;
    if (age < 18) {
        cout << "\nSorry! You are not eligible to cast a vote." << endl;
        return;
    }
    cout << "\nEnter your CNIC number for vote: ";
    cin >> cnic;
    cin.ignore();  
    cout << "\nEnter your full address to register the vote: ";
    getline(cin, fullAddress);

    node* temp = list.head;
    while (temp != NULL) {
        if (temp->data.firstName == firstName && temp->data.lastName == lastName &&
            temp->data.cnic == cnic && temp->data.address == fullAddress) {
            cout << "Sorry! You are unable to cast the vote because you have already cast a vote. Multiple votes are not allowed.\n";
            return;
        }
        temp = temp->next;
    }

    cout << "\n\t\tYour vote has been cast " << endl;
    cout << "\n\t*****THANK YOU*****" << endl;

    voter data{ firstName, lastName, cnic, fullAddress };
    list.insertnode(data);
    queue.enqueue(data);  
    string party;
    cout << "Enter the party you are voting for (PTI, PML-N, JUI-F, PPP): ";
    cin >> party;

    if (party == "PTI") pti++;
    else if (party == "PML-N") pml++;
    else if (party == "JUI-F") jui++;
    else if (party == "PPP") ppp++;
    else cout << "Invalid party name." << endl;
}

void displayVoteCounts() {
    cout << "\nVote Counts:\n";
    cout << "PTI: " << pti << "\n";
    cout << "PML-N: " << pml << "\n";
    cout << "JUI-F: " << jui << "\n";
    cout << "PPP: " << ppp << "\n";
}

void displayvoterdata(voterlist& list) {
    cout << "Information of the voters are : ";
    list.display();
}

void findLeadingCandidate() {
    if (pti > pml && pti > jui && pti > ppp) {
        cout << "PTI is the leading candidate with " << pti << " votes.\n";
    }
    else if (pml > pti && pml > jui && pml > ppp) {
        cout << "PML-N is the leading candidate with " << pml << " votes.\n";
    }
    else if (jui > pti && jui > pml && jui > ppp) {
        cout << "JUI-F is the leading candidate with " << jui << " votes.\n";
    }
    else if (ppp > pti && ppp > pml && ppp > jui) {
        cout << "PPP is the leading candidate with " << ppp << " votes.\n";
    }
    else {
        cout << "There is a tie between candidates.\n";
    }
}

int main() {
    system("color 07");
    cout << setw(70) << "\n\n\n\n\n\n";
    cout << setw(70) << "       *             *             *  \n";
    cout << setw(70) << "       *             *             *  \n";
    cout << setw(70) << "       *             *             *  \n";
    cout << setw(70) << "       *             *             *  \n";
    cout << setw(70) << "       *             *             *  \n";
    cout << setw(70) << "       *             *             *  \n";
    cout << setw(70) << "       *             *             *  \n";
    cout << setw(70) << "       *             *             *  \n";
    cout << setw(70) << "       *             *             *  \n";
    cout << setw(70) << "       *             *             *  \n";
    cout << setw(70) << "        *           * *           *   \n";
    cout << setw(70) << "         *         *   *         *    \n";
    cout << setw(70) << "          *       *     *       *     \n";
    cout << setw(70) << "           *     *       *     *      \n";
    cout << setw(70) << "            *   *         *   *       \n";
    cout << setw(70) << "             * *           * *        \n";
    system("color 07");
    Sleep(750);
    system("cls");


    cout << setw(70) << "\n\n\n\n\n\n";
    cout << setw(70) << "  ******  \n";
    cout << setw(70) << "  *                 \n";
    cout << setw(70) << "  *                 \n";
    cout << setw(70) << "  *                 \n";
    cout << setw(70) << "  *                 \n";
    cout << setw(70) << "  *                 \n";
    cout << setw(70) << "  *                 \n";
    cout << setw(70) << "  *                 \n";
    cout << setw(70) << "  *                 \n";
    cout << setw(70) << "  *                 \n";
    cout << setw(70) << "  *****       \n";
    cout << setw(70) << "  *                 \n";
    cout << setw(70) << "  *                 \n";
    cout << setw(70) << "  *                 \n";
    cout << setw(70) << "  *                 \n";
    cout << setw(70) << "  *                 \n";
    cout << setw(70) << "  *                 \n";
    cout << setw(70) << "  *                 \n";
    cout << setw(70) << "  *                 \n";
    cout << setw(70) << "  *                 \n";
    cout << setw(70) << "  *****   \n";
    system("color 07");
    Sleep(750);
    system("cls");


    cout << setw(70) << "\n\n\n\n\n\n";
    cout << setw(70) << "  *                    \n";
    cout << setw(70) << "  *                    \n";
    cout << setw(70) << "  *                    \n";
    cout << setw(70) << "  *                    \n";
    cout << setw(70) << "  *                    \n";
    cout << setw(70) << "  *                    \n";
    cout << setw(70) << "  *                    \n";
    cout << setw(70) << "  *                    \n";
    cout << setw(70) << "  *                    \n";
    cout << setw(70) << "  *                    \n";
    cout << setw(70) << "  *                    \n";
    cout << setw(70) << "  *                    \n";
    cout << setw(70) << "  *                    \n";
    cout << setw(70) << "  *                    \n";
    cout << setw(70) << "  *                    \n";
    cout << setw(70) << "  *                    \n";
    cout << setw(70) << "  *                    \n";
    cout << setw(70) << "  *                    \n";
    cout << setw(70) << "  ******   \n";
    system("color 07");
    Sleep(750);
    system("cls");

    cout << setw(70) << "\n\n\n\n\n\n";
    cout << setw(70) << "   **** \n";
    cout << setw(70) << "  *            \n";
    cout << setw(70) << " *             \n";
    cout << setw(70) << "*              \n";
    cout << setw(70) << "*              \n";
    cout << setw(70) << "*              \n";
    cout << setw(70) << "*              \n";
    cout << setw(70) << "*              \n";
    cout << setw(70) << "*              \n";
    cout << setw(70) << "*              \n";
    cout << setw(70) << "*              \n";
    cout << setw(70) << "*              \n";
    cout << setw(70) << "*              \n";
    cout << setw(70) << "*              \n";
    cout << setw(70) << "*              \n";
    cout << setw(70) << "*              \n";
    cout << setw(70) << "*              \n";
    cout << setw(70) << " *             \n";
    cout << setw(70) << "  *            \n";
    cout << setw(70) << "   **** \n";
    system("color 07");
    Sleep(750);
    system("cls");


    cout << setw(70) << "\n\n\n\n\n\n";
    cout << setw(70) << "      ***     \n";
    cout << setw(70) << "   *        *   \n";
    cout << setw(70) << "  *           *  \n";
    cout << setw(70) << " *             * \n";
    cout << setw(70) << "*               *\n";
    cout << setw(70) << "*               *\n";
    cout << setw(70) << "*               *\n";
    cout << setw(70) << "*               *\n";
    cout << setw(70) << "*               *\n";
    cout << setw(70) << "*               *\n";
    cout << setw(70) << "*               *\n";
    cout << setw(70) << "*               *\n";
    cout << setw(70) << "*               *\n";
    cout << setw(70) << "*               *\n";
    cout << setw(70) << "*               *\n";
    cout << setw(70) << "*               *\n";
    cout << setw(70) << " *             * \n";
    cout << setw(70) << "  *           *  \n";
    cout << setw(70) << "   *        *   \n";
    cout << setw(70) << "      ***     \n";
    system("color 07");
    Sleep(750);
    system("cls");

    cout << setw(70) << "\n\n\n\n\n\n";
    cout << setw(70) << "*               *\n";
    cout << setw(70) << "**             *\n";
    cout << setw(70) << "* *           * *\n";
    cout << setw(70) << "*  *         *  *\n";
    cout << setw(70) << "*   *       *   *\n";
    cout << setw(70) << "*    *     *    *\n";
    cout << setw(70) << "*     *   *     *\n";
    cout << setw(70) << "*      * *      *\n";
    cout << setw(70) << "*       *       *\n";
    cout << setw(70) << "*               *\n";
    cout << setw(70) << "*               *\n";
    cout << setw(70) << "*               *\n";
    cout << setw(70) << "*               *\n";
    cout << setw(70) << "*               *\n";
    cout << setw(70) << "*               *\n";
    cout << setw(70) << "*               *\n";
    cout << setw(70) << "*               *\n";
    cout << setw(70) << "*               *\n";
    cout << setw(70) << "*               *\n";
    cout << setw(70) << "*               *\n";
    system("color 07");
    Sleep(750);
    system("cls");

    cout << setw(70) << "\n\n\n\n\n\n";
    cout << setw(70) << "  *****  \n";
    cout << setw(70) << "  *                 \n";
    cout << setw(70) << "  *                 \n";
    cout << setw(70) << "  *                 \n";
    cout << setw(70) << "  *                 \n";
    cout << setw(70) << "  *                 \n";
    cout << setw(70) << "  *                 \n";
    cout << setw(70) << "  *                 \n";
    cout << setw(70) << "  *                 \n";
    cout << setw(70) << "  *                 \n";
    cout << setw(70) << "  *****       \n";
    cout << setw(70) << "  *                 \n";
    cout << setw(70) << "  *                 \n";
    cout << setw(70) << "  *                 \n";
    cout << setw(70) << "  *                 \n";
    cout << setw(70) << "  *                 \n";
    cout << setw(70) << "  *                 \n";
    cout << setw(70) << "  *                 \n";
    cout << setw(70) << "  *                 \n";
    cout << setw(70) << "  *                 \n";
    cout << setw(70) << "  ******   \n";
    system("color 07");
    Sleep(1000);
    system("cls");
    cout << setw(70) << "\n\n\n\n\n\n";
    cout << setw(70) << "        *        \n";
    cout << setw(70) << "       * *       \n";
    cout << setw(70) << "      *   *      \n";
    cout << setw(70) << "     *     *     \n";
    cout << setw(70) << "    *       *    \n";
    cout << setw(70) << "   *  * * *  *   \n";
    cout << setw(70) << "  *           *  \n";
    cout << setw(70) << " *             * \n";
    cout << setw(70) << "*****************\n";
    cout << setw(70) << "*               *\n";
    cout << setw(70) << "*               *\n";
    cout << setw(70) << "*    Online     *\n";
    cout << setw(70) << "*    Voting     *\n";
    cout << setw(70) << "*    SYSTEM     *\n";
    cout << setw(70) << "*               *\n";
    cout << setw(70) << "*               *\n";
    cout << setw(70) << "*               *\n";
    cout << setw(70) << "*               *\n";
    cout << setw(70) << "*****************\n";
    system("color 07");
    Sleep(700);
    system("color 07");
    Sleep(700);
    system("color 07");
    Sleep(700);
    system("color 07");
    Sleep(700);
    system("color 07");
    Sleep(700);
    system("cls");
    
    voterlist list;
    VoterQueue queue;

    int choice;
    do {
        cout << "\nMenu:\n";
        cout << "1. Register Vote\n";
        cout << "2. Display Voter Data\n";
        cout << "3. Save Voter Data to File\n";
        cout << "4. Load Voter Data from File\n";
        cout << "5. Display Vote Counts\n";
        cout << "6. Find Leading Candidate\n";
        cout << "7. Exit\n";
        cout << "Enter your choice: ";
        cin >> choice;

        switch (choice) {
        
        case 1:
            getvoterdata(list, queue);
            break;
        case 2:
            displayvoterdata(list);
            break;
        case 3:
            list.saveToFile("voters.txt");
            break;
        case 4:
            list.loadFromFile("voters.txt");
            break;
        case 5:
            displayVoteCounts();
            break;
        case 7:
            findLeadingCandidate();
            break;
        case 6:
            cout << "Exiting the program.\n";
            break;
        default:
            cout << "Invalid choice! Please try again.\n";
        }
    } while (choice != 7);

    return 0;
}
