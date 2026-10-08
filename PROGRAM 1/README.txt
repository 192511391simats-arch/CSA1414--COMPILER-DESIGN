#include <stdio.h>
#include <string.h>
#include <ctype.h>

int isKeyword(char word[])
{
    char keywords[][10] = {
        "int", "float", "char", "double",
        "if", "else", "for", "while",
        "return"
    };

    int i;

    for(i = 0; i < 9; i++)
    {
        if(strcmp(word, keywords[i]) == 0)
            return 1;
    }

    return 0;
}

int main()
{
    char code[200];
    char word[50];
    int i = 0, j;

    printf("Enter C code:\n");
    fgets(code, sizeof(code), stdin);

    while(code[i] != '\0')
    {
        /* Identifier or Keyword */
        if(isalpha(code[i]))
        {
            j = 0;

            while(isalnum(code[i]))
            {
                word[j++] = code[i++];
            }

            word[j] = '\0';

            if(isKeyword(word))
                printf("%s -> Keyword\n", word);
            else
                printf("%s -> Identifier\n", word);
        }

        /* Number */
        else if(isdigit(code[i]))
        {
            j = 0;

            while(isdigit(code[i]) || code[i] == '.')
            {
                word[j++] = code[i++];
            }

            word[j] = '\0';

            if(strchr(word, '.'))
                printf("%s -> Float Constant\n", word);
            else
                printf("%s -> Integer Constant\n", word);
        }

        /* Operators */
        else if(code[i] == '=' || code[i] == '+' ||
                code[i] == '-' || code[i] == '*' ||
                code[i] == '/')
        {
            printf("%c -> Operator\n", code[i]);
            i++;
        }

        /* Special Symbols */
        else if(code[i] == ';' || code[i] == '(' ||
                code[i] == ')' || code[i] == ',' ||
                code[i] == '{' || code[i] == '}')
        {
            printf("%c -> Special Symbol\n", code[i]);
            i++;
        }

        else
        {
            i++;
        }
    }

    return 0;
}
