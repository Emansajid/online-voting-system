#include <iostream>
#include <fstream>
#include <string>
#include <iomanip>
#include<windows.h>
#include<cstdlib>

using namespace std;

struct voter
{
    string firstName;
    string lastName;
    int cnic;
    string address;
};

class node
{
public:
    voter data;
    node* next;

    node(voter value)
    {
        data = value;
        next = NULL;
    }
};

class voterlist
{
public:
    node* head;

    voterlist()
    {
        head = NULL;
    }

    void insertnode(voter data)
    {
        node* new_node = new node(data);
        if (head == NULL)
        {
            head = new_node;
        }
        else
        {
            node* temp = head;
            while (temp->next != NULL)
            {
                temp = temp->next;
            }
            temp->next = new_node;
        }
    }

    void display()
    {
        node* temp = head;
        while (temp != NULL)
        {
            cout << "Name: " << temp->data.firstName << " " << temp->data.lastName;
            cout << "\tCNIC number: " << temp->data.cnic;
            cout << "\tAddress: " << temp->data.address;
            temp = temp->next;
            cout << endl;
        }
    }

    void saveToFile(const string& filename)
    {
        ofstream file("Voters.txt", ios::out | ios::app);
        if (!file.is_open())
        {
            cout << "Error opening file for writing." << endl;
            return;
        }

        node* temp = head;
        while (temp != NULL)
        {
            file << temp->data.firstName << "," << temp->data.lastName << ","
                << temp->data.cnic << "," << temp->data.address << endl;
            temp = temp->next;
        }

        file.close();
        cout << "Voter data has been saved to file." << endl;
    }

    void loadFromFile(const string& filename)
    {
        ifstream file("Voters.txt");
        if (!file.is_open())
        {
            cout << "Error opening file for reading." << endl;
            return;
        }

        voter data;
        while (file >> data.firstName >> data.lastName >> data.cnic >> ws && getline(file, data.address))
        {
            insertnode(data);
        }

        file.close();
        cout << "Voter data has been loaded from file." << endl;
    }
};


void getvoterdata(voterlist& list)
{
    string firstName, lastName;
    int cnic;
    int age;
    string fullAddress;

    cin.ignore();
    cout << "\nEnter your full name: ";
    cin >> firstName >> lastName;
    cout << "\nEnter your age for checking if you are eligible to vote or not: ";
    cin >> age;
    if (age < 18)
    {
        cout << "\nSorry! You are not eligible to cast a vote." << endl;
        return;
    }
    cout << "\nEnter your CNIC number for vote: ";
    cin >> cnic;
    cin.ignore();
    cout << "\nEnter your full address to register the vote: ";
    getline(cin, fullAddress);

    node* temp = list.head;
    while (temp != NULL)
    {
        if (temp->data.firstName == firstName && temp->data.lastName == lastName &&
            temp->data.cnic == cnic && temp->data.address == fullAddress)
        {
            cout << "Sorry! You are unable to cast the vote because you have already cast a vote. Multiple votes are not allowed.\n";
            return;
        }
        temp = temp->next;
    }

    cout << "\n\t\tYour vote has been cast " << endl;
    cout << "\n\t*****THANK YOU*****" << endl;

    voter data{ firstName, lastName, cnic, fullAddress };
    list.insertnode(data);
}

void votingoption()
{
    cout << "\t\n********WELCOME TO ELECTION-VOTING SYSTEM********\n\n\n";
    cout << "\t\t |1.Cast the vote          |" << endl;
    cout << "\t\t |2.Find vote count        |" << endl;
    cout << "\t\t |3.Find leading candidate |" << endl;
    cout << "\t\t |4.Exit                   |" << endl;
}

int pti = 0, pml = 0, jui = 0, ppp = 0;

void candidates()
{
    cout << "\n           The Candidates in the election are :         \n\n";
    cout << "                         MP election                        \n\n";
    cout << "*********************\n";
    cout << "|           1.PTI             |          2.PML-N         |\n";
    cout << "*********************\n";
    cout << "|           3.JUI-F           |          4.PPP           |\n";
    cout << "*********************\n\n";
    cout << "Press 1 to vote PTI\tPress 2 to vote PML-N\n";
    cout << "Press 3 to vote JUI-F\tPress 4 to vote PPP\n";
}

void calculateVote(int vote)
{
    switch (vote)
    {
    case 1:
        pti += 1;
        break;
    case 2:
        pml += 1;
        break;
    case 3:
        jui += 1;
        break;
    case 4:
        ppp += 1;
        break;
    }
}

void displayVoteCounts()
{
    cout << "\nVote Counts:\n";
    cout << "PTI: " << pti << "\n";
    cout << "PML-N: " << pml << "\n";
    cout << "JUI-F: " << jui << "\n";
    cout << "PPP: " << ppp << "\n";
}

void displayvoterdata(voterlist& list)
{
    cout << "Information of the voters are : ";
    list.display();
}

void findLeadingCandidate()
{
    if (pti > pml && pti > jui && pti > ppp)
    {
        cout << "PTI is the leading candidate with " << pti << " votes.\n";
    }
    else if (pml > pti && pml > jui && pml > ppp)
    {
        cout << "PML-N is the leading candidate with " << pml << " votes.\n";
    }
    else if (jui > pti && jui > pml && jui > ppp)
    {
        cout << "JUI-F is the leading candidate with " << jui << " votes.\n";
    }
    else if (ppp > pti && ppp > pml && ppp > jui)
    {
        cout << "PPP is the leading candidate with " << ppp << " votes.\n";
    }
    else
    {
        cout << "There is a tie between candidates.\n";
    }
}

int main()
{
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
    cout << setw(70) << "******\n";
    cout << setw(70) << "*               *\n";
    cout << setw(70) << "*               *\n";
    cout << setw(70) << "*    Online     *\n";
    cout << setw(70) << "*    Voting     *\n";
    cout << setw(70) << "*    SYSTEM     *\n";
    cout << setw(70) << "*               *\n";
    cout << setw(70) << "*               *\n";
    cout << setw(70) << "*               *\n";
    cout << setw(70) << "*               *\n";
    cout << setw(70) << "*******\n";
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
    int choice = 0;

mainmenu:
    system("cls");
    votingoption();
    cout << "\n\tPlease enter your choice: ";
    cin >> choice;
    system("cls");
    int n;
    switch (choice)
    {
    case 1:
        candidates();
        int candidateoption;
        cout << "Enter the number for the party you want to vote: ";
        cin >> candidateoption;
        system("cls");
        calculateVote(candidateoption);
        getvoterdata(list);
        cout << "Press 1 to back to the main menu.\nPress 2 to exit\n";
        cin >> n;
        if (n == 1)
        {
            goto mainmenu;
        }
        else if (n == 2)
        {
            cout << "Exiting the program " << endl;
        }
        else
        {
            cout << "Invalid input " << endl;
        }
        break;
    case 2:
        cout << "The counting of the voters is : " << endl;
        displayVoteCounts();
        cout << "Press 1 to go back to the main menu\nPress 2 to exit " << endl;
        cin >> n;
        if (n == 1)
        {
            goto mainmenu;
        }
        else if (n == 2)
        {
            cout << "Exiting the program " << endl;
        }
        else
        {
            cout << "Invalid input " << endl;
        }
        break;
    case 3:
        cout << "Finding the leading candidate:\n";
        findLeadingCandidate();
        cout << "Press 1 to back to the main menu.\n Press 2 to exit\n";
        cin >> n;
        if (n == 1)
        {
            goto mainmenu;
        }
        else if (n == 2)
        {
            cout << "Exiting the program " << endl;
        }
        else
        {
            cout << "Invalid input " << endl;
        }
        break;
    case 4:
        cout << "Exiting the program.\n";
        n = 2;
        break;
    default:
        cout << "Invalid choice. Please enter a valid option.\n";
        n = 1;
    }


    return 0;
}
