#include<iostream>
#include<fstream>
#include <string>

using namespace std;

//functions
    void addStudent();
    void displayStudent(int);
    void displayAllStudents();
    void updateStudent(int);
    void deleteStudent(int);

    void printMenu();
    void selectMenu(char ch);

    void printMenu1();
    void selectMenu1(char ch);

    void printMenu2();
    void selectMenu2(int ch);

    void academic_advisement();
    void financial_advisement ();
    void counselling();
    void tutorial();

class student
{
    private:
        int id;
        char name[50];
        double gpa;

    public:
        void insertData();
        void printData();
        int getId();
};

void student::insertData()
{
    cout << "\n\nEnter student's ID: ";
    cin >> id;
    cout << "\nEnter student's name: ";
    cin.ignore();
    cin.getline(name,50);
    cout << "\nEnter student's GPA: ";
    cin >> gpa;
}

void student::printData()
{
    cout << "\nStudent ID: " << id;
    cout << "\nStudent name : " << name;
    cout <<" \nStudent GPA : "<< gpa;
}

int  student::getId()
{
    return id;
}

void addStudent()
{
    student stud;
    stud.insertData();
    ofstream oFile;
    oFile.open("student.dat",ios::binary|ios::app);
    oFile.write(reinterpret_cast<char *> (&stud), sizeof(student));
    oFile.close();
    cout<<"\n\nNew student added. \nPress Enter key to return to the menu. ";
    cin.ignore();
    cin.get();
}

void displayStudent(int n)
{
    student stud;
    ifstream iFile;
    iFile.open("student.dat",ios::binary);

    if(!iFile)
    {
        cout<<"\n\nFile could not be opened! \nPress Enter key to return to the menu.";
        cin.ignore();
        cin.get();
        return;
    }

    bool flag = false;
    while(iFile.read(reinterpret_cast<char *> (&stud), sizeof(student)))
    {
        if(stud.getId() == n)
        {
            stud.printData();
            cout<<"\n\n====================================\n";
            flag = true;
        }
    }

    if(flag == false)
        cout << "\n\nStudent does not exist!";

    cout << "\nPress Enter key to return to the menu. ";
    iFile.close();
    cin.ignore();
    cin.get();
}

void displayAllStudents()
{
    student stud;
    ifstream inFile;
    inFile.open("student.dat",ios::binary);

    if(!inFile)
    {
        cout<<"\n\nFile could not be opened! \nPress Enter key to return to the menu.";
        cin.ignore();
        cin.get();
        return;
    }

    cout<<"\n\n\n\t\tDISPLAYING ALL STUDENTS\n\n";

    while(inFile.read(reinterpret_cast<char *> (&stud), sizeof(student)))
    {
        stud.printData();
        cout<<"\n\n====================================\n";
    }

    cout << "\nPress Enter key to return to the menu. ";
    inFile.close();
    cin.ignore();
    cin.get();
}

void updateStudent(int n)
{
    bool found = false;
    student stud;
    fstream file;
    file.open("student.dat",ios::binary|ios::in|ios::out);

    if(!file)
    {
        cout << "\n\nFile could not be opened! \nPress Enter key to return to the menu.";
        cin.ignore();
        cin.get();
        return;
    }

    while(!file.eof() && found==false)
    {
        file.read(reinterpret_cast<char *> (&stud), sizeof(student));
        if(stud.getId() == n)
        {
            stud.printData();
            cout << "\n\nEnter new student details:" << endl;
            stud.insertData();
            int pos=(-1)*sizeof(stud);
            file.seekp(pos,ios::cur);
            file.write(reinterpret_cast<char *> (&stud), sizeof(student));
            cout << "\n\nStudent Updated. \nPress Enter key to return to the menu. ";
            found = true;
        }
    }

    file.close();
    if(found == false)
    cout<<"\n\nRecord Not Found. \nPress Enter key to return to the menu. ";
    cin.ignore();
    cin.get();
}

void deleteStudent(int n)
{
    student stud;
    ifstream iFile;
    iFile.open("student.dat",ios::binary);

    if(!iFile)
    {
        cout << "\n\nFile could not be opened! \nPress Enter key to return to the menu.";
        cin.ignore();
        cin.get();
        return;
    }

    ofstream oFile;
    oFile.open("Temp.dat",ios::out);
    while(iFile.read(reinterpret_cast<char *> (&stud), sizeof(student)))
    {
        if(stud.getId() != n)
        {
            oFile.write(reinterpret_cast<char *> (&stud), sizeof(student));
        }
    }

    oFile.close();
    iFile.close();
    remove("student.dat");
    rename("Temp.dat","student.dat");
    cout<<"\n\nStudent Deleted. \nPress Enter key to return to the menu. ";
    cin.ignore();
    cin.get();
}

int main() {

    char ch;

    //int num, value;

    do {
        printMenu();

        cout << "\n\n\nEnter your Choice (1 - 7) ";
        cin >> ch;
        selectMenu(ch);
    }while(ch!='7');

    return 0;
}

//functions
void printMenu()
{
        //system("clear");
        cout << "\n\n\n========== MENU ==========";
        cout << "\n\n\t1.Add new student";
        cout << "\n\n\t2.Search student";
        cout << "\n\n\t3.Display all students";
        cout << "\n\n\t4.Update student";
        cout << "\n\n\t5.Delete student";
        cout << "\n\n\t6.Other options!";
        cout << "\n\n\t7.Exit";
}

void selectMenu(char ch)
{
    switch(ch){
    int num;
    case '1':
        addStudent();
    break;

    case '2':
        cout << "\n\n\tEnter student's ID: ";
        cin >> num;
        displayStudent(num);
    break;

    case '3':
        displayAllStudents();
    break;

    case '4':
        cout << "\n\n\tEnter student's ID:  ";
        cin >> num;
        updateStudent(num);
    break;

    case '5':
        cout << "\n\n\tEnter student's ID: ";
        cin >> num;
        deleteStudent(num);
    break;

    case '6':
        cout << "Other options!";
        char choice;
        printMenu1();
        cin >> choice;
        selectMenu1(choice);
    break;

    case '7':
        cout << "Exiting, Thank you!";
        exit(0);
    }
}

// Function that display Academic advisement informations
void printMenu2()
{
    int numb;
    cout << "1. Display" << endl;
    cout << "2. Back to the main menu." << endl;
    cout << "\nSelect a value between 1 and 2 : " << endl;
}

// Main menu that displays informations options
void printMenu1()
{
    cout << "\n1.Search for academic advisement" << endl ;
    cout << "2.Search for financial advisement" << endl ;
    cout << "3.Search for Counselling" << endl ;
    cout << "4.Search for tutorial" << endl ;
    cout << "\nSelect a value between 1 and 4 : " << endl;
}

void selectMenu1(char ch)
{
    switch(ch)
    {
        int numb;
        case '1':;
            printMenu2();
            cin >> numb;
                if(numb == 1)
                {
                    academic_advisement();
                }
                else if(numb == 2)
                {
                    printMenu();
                }
                else
                {
                    cout << "Please enter a value between 1 and 2.";
                }
            break;

        case '2':
            printMenu2();
            cin >> numb;
                if(numb == 1)
                {
                    financial_advisement();
                }
                else if(numb == 2)
                {
                    printMenu();
                }
                else
                {
                    cout << "Please enter a value between 1 and 2.";
                }
            break;

        case '3':
            printMenu2();
            cin >> numb;
                if(numb == 1)
                {
                    counselling ();
                }
                else if(numb == 2)
                {
                    printMenu();
                }
                else
                {
                    cout << "Please enter a value between 1 and 2.";
                }
            break;

        case '4':
            printMenu2();
            cin >> numb;
                if(numb == 1)
                {
                    tutorial ();
                }
                else if(numb == 2)
                {
                    printMenu();
                }
                else
                {
                    cout << "Please enter a value between 1 and 2.";
                }
            break;

        default:
            cout << "\nSelect a number between 1 and 4" << endl;
    }
}

// Academic advisement
void academic_advisement()
{
  cout << "Department of Advisor should be found, on your cuny account or in the main building office room S-108."<< endl;
  cout << "In order to be advised to register for classes you have to meet with your faculty advisor." << endl ;
  cout<<" "<<endl;
}

// Financial Advisement
void financial_advisement()
{
  cout << "Bursars Offices located at Main Building Room 505, Contact +1 (502-001-8453) " << endl ;
}

void counselling ()
{
  cout << "The Counselling center is located at the main building in room S--343. " << endl ;
  cout << "You can reach the counselling by emailling at counsellingcenter@bmcc.cuny.edu to schedule at time to meet with an available counsellor eitheir on zoom or in person." << endl ;
}

void tutorial ()
{
  cout << "Tutorial is located at the main building at 199 Chambers       St, 5th Floor , room S-510 and F-511 (Fiterman Hall) " << endl ;
}
//function to select
