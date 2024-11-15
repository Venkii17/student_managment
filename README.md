# student_managment
#include <stdio.h>
#include <string.h>

#define MAX_STUDENTS 100

struct Student {
    int id;
    char name[50];
    float marks[3];
    float average;
    char grade;
};

struct Student students[MAX_STUDENTS];
int studentCount = 0;

void addStudent() {
    if (studentCount < MAX_STUDENTS) {
        struct Student s;
        s.id = studentCount + 1;
        printf("Enter name: ");
        scanf("%[^\n]", s.name);

        printf("Enter marks for three subjects:\n");
        for (int i = 0; i < 3; i++) {
            printf("Mark %d: ", i + 1);
            scanf("%f", &s.marks[i]);
        }

        // Calculate average
        s.average = (s.marks[0] + s.marks[1] + s.marks[2]) / 3;

        // Calculate grade
        if (s.average >= 90) s.grade = 'A';
        else if (s.average >= 75) s.grade = 'B';
        else if (s.average >= 50) s.grade = 'C';
        else s.grade = 'F';

        students[studentCount++] = s;
        printf("Student added with ID: %d\n\n", s.id);
    } else {
        printf("Max students reached!\n");
    }
}

void displayStudents() {
    if (studentCount == 0) {
        printf("No students to display.\n\n");
        return;
    }

    printf("ID\tName\t\tMarks\t\tAverage\tGrade\n");
    printf("\n");
    for (int i = 0; i < studentCount; i++) {
        struct Student s = students[i];
        printf("%d\t%s\t%.1f, %.1f, %.1f\t%.2f\t%c\n", s.id, s.name, s.marks[0], s.marks[1], s.marks[2], s.average, s.grade);
    }
    printf("\n");
}

void editStudent() {
    int id;
    printf("Enter student ID to edit: ");
    scanf("%d", &id);

    if (id <= 0 || id > studentCount) {
        printf("Invalid ID.\n");
        return;
    }

    struct Student *s = &students[id - 1];
    printf("Editing student %s\n", s->name);

    printf("Enter new name: ");
    scanf("%[^\n]", s->name);

    printf("Enter new marks for three subjects:\n");
    for (int i = 0; i < 3; i++) {
        printf("Mark %d: ", i + 1);
        scanf("%f", &s->marks[i]);
    }

    // Recalculate average and grade
    s->average = (s->marks[0] + s->marks[1] + s->marks[2]) / 3;
    if (s->average >= 90) s->grade = 'A';
    else if (s->average >= 75) s->grade = 'B';
    else if (s->average >= 50) s->grade = 'C';
    else s->grade = 'F';

    printf("Student record updated.\n\n");
}

void deleteStudent() {
    int id;
    printf("Enter student ID to delete: ");
    scanf("%d", &id);

    if (id <= 0 || id > studentCount) {
        printf("Invalid ID.\n");
        return;
    }

    for (int i = id - 1; i < studentCount - 1; i++) {
        students[i] = students[i + 1];
    }
    studentCount--;
    printf("Student record deleted.\n\n");
}

int main() {
    int choice;
    do {
        printf("Student Management System\n");
        printf("1. Add Student\n");
        printf("2. Display Students\n");
        printf("3. Edit Student\n");
        printf("4. Delete Student\n");
        printf("5. Exit\n");
        printf("Enter your choice: ");
        scanf("%d", &choice);

        switch (choice) {
            case 1:
                addStudent();
                break;
            case 2:
                displayStudents();
                break;
            case 3:
                editStudent();
                break;
            case 4:
                deleteStudent();
                break;
            case 5:
                printf("Exiting...\n");
                break;
            default:
                printf("Invalid choice! Please try again.\n");
        }
    } while (choice != 5);

    return 0;
}