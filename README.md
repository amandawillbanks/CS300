// ProjectTwo.cpp
// Step 1: Include all the necessary libraries
#include <iostream>
#include <fstream>
#include <sstream>
#include <vector>
#include <string>

using namespace std;

// ANSI escape codes for terminal colors
const string RESET = "\033[0m";
const string RED = "\033[31m";
const string GREEN = "\033[32m";

// Step 2: Define what a Course looks like
struct Course {
    string courseId;  // Like 'CS101'
    string courseName;  // Full name of the course
    vector<string> prerequisites;  // List of course IDs that are prerequisites
};

// Step 3: Create a Node to hold our course inside the Binary Search Tree
struct Node {
    Course course;
    Node* left;
    Node* right;

    Node(Course c) : course(c), left(nullptr), right(nullptr) {}
};

// Step 4: Build the Binary Search Tree structure
class CourseBST {
private:
    Node* root;

    // Insert helper
    Node* insert(Node* node, Course course) {
        if (node == nullptr) return new Node(course);

        if (course.courseId < node->course.courseId) {
            node->left = insert(node->left, course);
        }
        else {
            node->right = insert(node->right, course);
        }
        return node;
    }

    // In-order traversal helper for sorted printing
    void inOrder(Node* node) {
        if (node == nullptr) return;
        inOrder(node->left);
        cout << node->course.courseId << ": " << node->course.courseName << endl;
        inOrder(node->right);
    }

    // Search helper
    Node* search(Node* node, string courseId) {
        if (node == nullptr || node->course.courseId == courseId) return node;

        if (courseId < node->course.courseId) {
            return search(node->left, courseId);
        }
        else {
            return search(node->right, courseId);
        }
    }

public:
    CourseBST() : root(nullptr) {}

    // Step 5: Insert a course into the tree
    void insert(Course course) {
        root = insert(root, course);
    }

    // Step 6: Print all courses in order
    void printInOrder() {
        inOrder(root);
    }

    // Step 7: Find and return a specific course
    Course* findCourse(string courseId) {
        Node* found = search(root, courseId);
        return found ? &found->course : nullptr;
    }
};

// Step 8: Helper function to split a string into parts using a comma
vector<string> split(const string& str, char delim) {
    vector<string> tokens;
    stringstream ss(str);
    string item;
    while (getline(ss, item, delim)) {
        tokens.push_back(item);
    }
    return tokens;
}

// Step 9: Load course data from the file the user gives us
void LoadCourseData(string fileName, CourseBST& courseTree) {
    ifstream file(fileName);
    string line;

    while (getline(file, line)) {
        vector<string> parts = split(line, ',');
        if (parts.size() >= 2) {
            Course course;
            course.courseId = parts[0];
            course.courseName = parts[1];
            for (size_t i = 2; i < parts.size(); ++i) {
                course.prerequisites.push_back(parts[i]);
            }
            courseTree.insert(course);
        }
    }
    file.close();
}

// Step 10: Let the user search for a course and show its info
void PrintCourseDetails(CourseBST& courseTree) {
    string courseId;
    cout << "Enter course ID: ";
    cin >> courseId;

    Course* course = courseTree.findCourse(courseId);
    if (course) {
        cout << course->courseId << ": " << course->courseName << endl;
        if (!course->prerequisites.empty()) {
            cout << "Prerequisites: ";
            for (string pre : course->prerequisites) {
                cout << pre << " ";
            }
            cout << endl;
        }
        else {
            cout << "Prerequisites: None" << endl;
        }
    }
    else {
        cout << RED << "Course not found." << RESET << endl;
    }
}

// Step 11: Main menu where everything runs
int main() {
    CourseBST courseTree;
    bool dataLoaded = false;
    int userOption = 0;

    while (userOption != 9) {
        // Show the menu
        cout << "\nMenu:" << endl;
        cout << "  1. Load course data" << endl;
        cout << "  2. Print sorted list of courses" << endl;
        cout << "  3. Print course title and prerequisites" << endl;
        cout << "  9. Exit" << endl;
        cout << "Enter option: ";
        cin >> userOption;

        // Step 12: Handle user choices
        if (userOption == 1) {
            string fileName;
            cout << "Enter file name: ";
            cin >> fileName;
            LoadCourseData(fileName, courseTree);
            dataLoaded = true;
            cout << GREEN << "Course data loaded successfully." << RESET << endl;

        }
        else if (userOption == 2) {
            if (!dataLoaded) {
                cout << RED << "You must load data first." << RESET << endl;
            }
            else {
                courseTree.printInOrder();
            }

        }
        else if (userOption == 3) {
            if (!dataLoaded) {
                cout << RED << "You must load data first." << RESET << endl;
            }
            else {
                PrintCourseDetails(courseTree);
            }

        }
        else if (userOption == 9) {
            cout << "Exiting program..." << endl;
        }
        else {
            cout << RED << "Invalid option. Please try again." << RESET << endl;
        }
    }

    return 0;
}
