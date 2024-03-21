ldksjflkjdlkjflkdjflkdjfljds# 9thDevelop a Program in C for the following operationson Singly Circular Linked List
(SCLL) with header nodes 
a. Represent and Evaluate a Polynomial
 P(x,y,z) = 6x2y2z-4yz5+3x3yz+2xy5z-2xyz3 
b. Find the sum of two polynomials POLY1(x,y,z)
 and POLY2(x,y,z) and store the result in POLYSUM(x,y,z) Support the 
program with appropriate functions for each of the above operations

#include <stdio.h>
#include <stdlib.h>

// Structure to represent a term in a polynomial
struct Term {
    int coefficient;
    int x_power;
    int y_power;
    int z_power;
    struct Term *next;
};

// Function prototypes
struct Term* createTerm(int coeff, int x_pow, int y_pow, int z_pow);
void insertTerm(struct Term **head, int coeff, int x_pow, int y_pow, int z_pow);
void displayPolynomial(struct Term *head);
void evaluatePolynomial(struct Term *head, int x, int y, int z);
struct Term* addPolynomials(struct Term *poly1, struct Term *poly2);

int main() {
    struct Term *poly1 = NULL, *poly2 = NULL, *poly_sum = NULL;
    int x, y, z;

    // Represent and evaluate polynomial POLY1(x,y,z) = 6x^2y^2z - 4yz^5 + 3x^3yz + 2xy^5z - 2xyz^3
    insertTerm(&poly1, 6, 2, 2, 1);
    insertTerm(&poly1, -4, 0, 1, 5);
    insertTerm(&poly1, 3, 3, 1, 1);
    insertTerm(&poly1, 2, 1, 5, 1);
    insertTerm(&poly1, -2, 1, 1, 3);

    printf("Polynomial POLY1(x,y,z) = ");
    displayPolynomial(poly1);

    printf("Enter values for x, y, and z to evaluate POLY1(x,y,z): ");
    scanf("%d %d %d", &x, &y, &z);
    evaluatePolynomial(poly1, x, y, z);

    // Represent and evaluate polynomial POLY2(x,y,z) = 3x^2y^3 - 2yz^2 + x^2yz - 3xyz^3
    insertTerm(&poly2, 3, 2, 3, 0);
    insertTerm(&poly2, -2, 0, 1, 2);
    insertTerm(&poly2, 1, 2, 1, 1);
    insertTerm(&poly2, -3, 1, 1, 3);

    printf("\nPolynomial POLY2(x,y,z) = ");
    displayPolynomial(poly2);

    printf("Enter values for x, y, and z to evaluate POLY2(x,y,z): ");
    scanf("%d %d %d", &x, &y, &z);
    evaluatePolynomial(poly2, x, y, z);

    // Add POLY1 and POLY2 to get POLYSUM
    poly_sum = addPolynomials(poly1, poly2);
    printf("\nSum of POLY1(x,y,z) and POLY2(x,y,z) = ");
    displayPolynomial(poly_sum);

    // Free memory allocated for polynomials
    struct Term *temp;
    while (poly1 != NULL) {
        temp = poly1;
        poly1 = poly1->next;
        free(temp);
    }
    while (poly2 != NULL) {
        temp = poly2;
        poly2 = poly2->next;
        free(temp);
    }
    while (poly_sum != NULL) {
        temp = poly_sum;
        poly_sum = poly_sum->next;
        free(temp);
    }

    return 0;
}

// Function to create a term in the polynomial
struct Term* createTerm(int coeff, int x_pow, int y_pow, int z_pow) {
    struct Term *newTerm = (struct Term*)malloc(sizeof(struct Term));
    if (newTerm == NULL) {
        printf("Memory allocation failed.\n");
        exit(1);
    }
    newTerm->coefficient = coeff;
    newTerm->x_power = x_pow;
    newTerm->y_power = y_pow;
    newTerm->z_power = z_pow;
    newTerm->next = NULL;
    return newTerm;
}

// Function to insert a term into the polynomial
void insertTerm(struct Term **head, int coeff, int x_pow, int y_pow, int z_pow) {
    struct Term *newTerm = createTerm(coeff, x_pow, y_pow, z_pow);
    if (*head == NULL) {
        *head = newTerm;
        (*head)->next = *head; // Point to itself for circular linked list
    } else {
        struct Term *current = *head;
        while (current->next != *head) {
            current = current->next;
        }
        current->next = newTerm;
        newTerm->next = *head;
    }
}

// Function to display the polynomial
void displayPolynomial(struct Term *head) {
    struct Term *current = head;
    do {
        printf("%dx^%dy^%dz^%d ", current->coefficient, current->x_power, current->y_power, current->z_power);
        if (current->next != head)
            printf("+ ");
        current = current->next;
    } while (current != head);
    printf("\n");
}

// Function to evaluate the polynomial for given values of x, y, and z
void evaluatePolynomial(struct Term *head, int x, int y, int z) {
    struct Term *current = head;
    int result = 0;
    do {
        result += current->coefficient * pow(x, current->x_power) * pow(y, current->y_power) * pow(z, current->z_power);
        current = current->next;
    } while (current != head);
    printf("Result of evaluation: %d\n", result);
}

// Function to add two polynomials
struct Term* addPolynomials(struct Term *poly1, struct Term *poly2) {
    struct Term *result = NULL;
    struct Term *current1 = poly1;
    struct Term *current2 = poly2;

    do {
        insertTerm(&result, current1->coefficient, current1->x_power, current1->y_power, current1->z_power);
        current1 = current1->next;
    } while (current1 != poly1);

