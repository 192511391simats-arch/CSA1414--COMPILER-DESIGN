```c
#include <stdio.h>
#include <string.h>
#include <ctype.h>

char expr[100];
int pos = 0, temp = 1;

char getTemp() {
    return temp++;
}

void expression();
void term();
void factor();

void factor() {
    if (isalnum(expr[pos])) {
        printf("Operand: %c\n", expr[pos]);
        pos++;
    }
    else if (expr[pos] == '(') {
        pos++;
        expression();
        if (expr[pos] == ')')
            pos++;
    }
}

void term() {
    factor();

    while (expr[pos] == '*' || expr[pos] == '/') {
        char op = expr[pos++];
        factor();
        printf("Apply operator: %c\n", op);
    }
}

void expression() {
    term();

    while (expr[pos] == '+' || expr[pos] == '-') {
        char op = expr[pos++];
        term();
        printf("Apply operator: %c\n", op);
    }
}

int main() {
    char lhs;

    printf("Enter expression (e.g., a=b+c*d): ");
    scanf("%99s", expr);

    char *p = strchr(expr, '=');

    if (p == NULL) {
        printf("Invalid expression!\n");
        return 1;
    }

    lhs = expr[0];
    pos = (int)(p - expr) + 1;

    printf("\nThree Address Code:\n");

    if (expr[pos] == '\0') {
        printf("Invalid expression!\n");
        return 1;
    }

    expression();
    printf("%c = result\n", lhs);

    return 0;
}
```
