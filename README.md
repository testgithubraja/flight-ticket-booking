# flight-ticket-booking
Flight Ticket Booking System Using C
#include <stdio.h>
#include <stdlib.h>
#include <string.h>
#include <Windows.h>

struct IndoAir_Airlines {
    char passport[6];
    char name[15];
    char destination[15];
    int seat_num;
    char email[15];
    struct IndoAir_Airlines *following;
} *begin, *stream;
struct IndoAir_Airlines *dummy;

void details() {
    printf("\n\t Enter your name: ");
    fgets(stream->name, sizeof(stream->name), stdin);
    stream->name[strcspn(stream->name, "\n")] = 0;

    printf("\n\t Enter your email address: ");
    fgets(stream->email, sizeof(stream->email), stdin);
    stream->email[strcspn(stream->email, "\n")] = 0;

    printf("\n\t Enter the Destination: ");
    fgets(stream->destination, sizeof(stream->destination), stdin);
    stream->destination[strcspn(stream->destination, "\n")] = 0;
}

void reserve(int x) {
    stream = begin;
    if (begin == NULL) {
        begin = stream = (struct IndoAir_Airlines *)malloc(sizeof(struct IndoAir_Airlines));
        printf("\n\t Enter your passport number: ");
        fgets(stream->passport, sizeof(stream->passport), stdin);
        stream->passport[strcspn(stream->passport, "\n")] = 0;
        details();
        stream->following = NULL;
        stream->seat_num = x;
        printf("\n\t Seat booking successful!");
        printf("\n\t Your seat number is: Seat A-%d", x);
        return;
    } else if (x > 15) {
        printf("\n\t\t All seats are full.");
        return;
    } else {
        while (stream->following) {
            stream = stream->following;
        }
        stream->following = (struct IndoAir_Airlines *)malloc(sizeof(struct IndoAir_Airlines));
        stream = stream->following;
        printf("\n\t Enter your passport number: ");
        fgets(stream->passport, sizeof(stream->passport), stdin);
        stream->passport[strcspn(stream->passport, "\n")] = 0;
        details();
        stream->following = NULL;
        stream->seat_num = x;
        printf("\n\t Seat booking successful!");
        printf("\n\t Your seat number is: Seat A-%d", x);
        return;
    }
}

void savefile() {
    FILE *fpointer = fopen("raja_records.txt", "w");
    if (!fpointer) {
        printf("\n Error in opening file!");
        return;
    }
    stream = begin;
    while (stream) {
        fprintf(fpointer, "%-6s", stream->passport);
        fprintf(fpointer, "%-15s", stream->name);
        fprintf(fpointer, "%-15s", stream->email);
        fprintf(fpointer, "%-15s", stream->destination);
        fprintf(fpointer, "\n");
        stream = stream->following;
    }
    printf("\n\n\t Details have been saved to the file.");
    fclose(fpointer);
}

void display() {
    stream = begin;
    if (stream == NULL) {
        printf("\n\n No records found!");
        return;
    }
    while (stream) {
        printf("\n\n Passport Number : %-6s", stream->passport);
        printf("\n         Name : %-15s", stream->name);
        printf("\n      Email Address: %-15s", stream->email);
        printf("\n      Seat Number: A-%d", stream->seat_num);
        printf("\n     Destination: %-15s", stream->destination);
        printf("\n\n++*=====================================================*++");
        stream = stream->following;
    }
}

void cancel() {
    char passport[6];
    printf("\n\n Enter passport number to delete record: ");
    fgets(passport, sizeof(passport), stdin);
    passport[strcspn(passport, "\n")] = 0;

    if (begin == NULL) {
        printf("\n\n No records to delete.");
        return;
    }

    if (strcmp(begin->passport, passport) == 0) {
        dummy = begin;
        begin = begin->following;
        free(dummy);
        printf(" Booking has been deleted.");
        Sleep(800);
        return;
    }

    stream = begin;
    while (stream->following) {
        if (strcmp(stream->following->passport, passport) == 0) {
            dummy = stream->following;
            stream->following = stream->following->following;
            free(dummy);
            printf(" Booking has been deleted.");
            Sleep(800);
            return;
        }
        stream = stream->following;
    }
    printf(" Passport number is invalid. Please check your passport.");
}

void main() {
    void reserve(int x), cancel(), display(), savefile();
    int choice;
    begin = stream = NULL;
    int num = 1;
    do {
        printf("\n\n\t\t ********************************************************************");
        printf("\n\t\t                   Welcome to IndoAir's Airline System                   ");
        printf("\n\t\t   ********************************************************************");
        printf("\n\n\n\t\t Please enter your choice from below (1-4):");
        printf("\n\n\t\t 1. Reservation");
        printf("\n\n\t\t 2. Cancel");
        printf("\n\n\t\t 3. Display Records");
        printf("\n\n\t\t 4. Exit");
        printf("\n\n\t\t Feel free to contact at IndoAir@gmail.com ");
        printf("\n\n\t\t Enter your choice: ");

        char choice_str[10];
        fgets(choice_str, sizeof(choice_str), stdin);
        sscanf(choice_str, "%d", &choice);
        system("cls");

        switch (choice) {
            case 1:
                reserve(num);
                num++;
                break;
            case 2:
                cancel();
                break;
            case 3:
                display();
                break;
            case 4:
                savefile();
                break;
            default:
                printf("\n\n\t Invalid choice! Please choose between 1-4.");
        }
        getch();
    } while (choice != 4);
}
