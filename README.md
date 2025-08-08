#include <iostream>
#include <direct.h>    // For _mkdir, _chdir, _getcwd
#include <io.h>        // For _findfirst, _findnext functions
#include <cstring>     // For strlen
using namespace std;

// Function to list files and folders in the current directory
void listFiles() {
    struct _finddata_t fileInfo;
    intptr_t handle;

    // Start searching in the current directory with wildcard
    handle = _findfirst("*", &fileInfo);

    if (handle == -1) {
        cout << "No files found.\n";
        return;
    }

    cout << "Files and directories in current directory:\n";
    do {
        cout << fileInfo.name << endl;
    } while (_findnext(handle, &fileInfo) == 0);

    _findclose(handle);
}

// Function to create a new directory
void createDirectory() {
    string dirname;
    cout << "Enter the name of the new directory: ";
    cin >> dirname;

    if (_mkdir(dirname.c_str()) == 0) {
        cout << "Directory created successfully.\n";
    } else {
        perror("Error creating directory");
    }
}

// Function to change the working directory
void changeDirectory() {
    string dirname;
    cout << "Enter the directory to change to: ";
    cin >> dirname;

    if (_chdir(dirname.c_str()) == 0) {
        char buffer[FILENAME_MAX];
        _getcwd(buffer, FILENAME_MAX);
        cout << "Current directory changed to: " << buffer << endl;
    } else {
        perror("Error changing directory");
    }
}

int main() {
    int choice;
    while (true) {
        cout << "\nDirectory Management System\n";
        cout << "1. List files in current directory\n";
        cout << "2. Create a new directory\n";
        cout << "3. Change working directory\n";
        cout << "4. Exit\n";
        cout << "Enter your choice: ";
        cin >> choice;

        switch(choice) {
            case 1:
                listFiles();
                break;
            case 2:
                createDirectory();
                break;
            case 3:
                changeDirectory();
                break;
            case 4:
                cout << "Exiting program.\n";
                return 0;
            default:
                cout << "Invalid choice. Please try again.\n";
        }
    }
    return 0;
}
