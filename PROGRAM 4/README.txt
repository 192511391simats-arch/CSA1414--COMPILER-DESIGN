
#include <stdio.h>
#include <string.h>

int main() {
    char code[10][100];
    int n, i, j, live[10] = {0};

    printf("Enter number of statements: ");
    scanf("%d", &n);
    getchar();

    printf("Enter the statements:\n");
    for (i = 0; i < n; i++) {
        fgets(code[i], sizeof(code[i]), stdin);
        code[i][strcspn(code[i], "\n")] = '\0';
    }

    // Mark the final statement as live
    live[n - 1] = 1;

    // Find statements required by live statements
    for (i = n - 1; i >= 0; i--) {
        if (live[i]) {
            char *eq = strchr(code[i], '=');

            if (eq != NULL) {
                char lhs = code[i][0];

                for (j = i - 1; j >= 0; j--) {
                    if (strchr(code[j], '=') != NULL &&
                        code[j][0] == lhs) {
                        live[j] = 0;
                    }
                }

                for (j = 0; j < i; j++) {
                    if (strchr(code[i], code[j][0]) != NULL)
                        live[j] = 1;
                }
            }
        }
    }

    printf("\nCode after dead code elimination:\n");
    for (i = 0; i < n; i++) {
        if (live[i])
            printf("%s\n", code[i]);
    }

    return 0;
}
