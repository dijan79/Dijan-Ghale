#include <stdio.h>
#include <stdlib.h>
#include <string.h>

// ROT13 encryption/decryption function
void rot13(char *str) {
    for (int i = 0; str[i] != '\0'; i++) {
        if (str[i] >= 'A' && str[i] <= 'Z') {
            str[i] = (str[i] - 'A' + 13) % 26 + 'A';
        } else if (str[i] >= 'a' && str[i] <= 'z') {
            str[i] = (str[i] - 'a' + 13) % 26 + 'a';
        } else if (str[i] >= '0' && str[i] <= '9') {
            str[i] = (str[i] - '0' + 13) % 10 + '0';
        }
    }
}
void rot13d(char *str) {
    for (int i = 0; str[i] != '\0'; i++) {
        if (str[i] >= 'A' && str[i] <= 'Z') {
            str[i] = (str[i] - 'A' + 13) % 26 + 'A';
        } else if (str[i] >= 'a' && str[i] <= 'z') {
            str[i] = (str[i] - 'a' + 13) % 26 + 'a';
        } else if (str[i] >= '0' && str[i] <= '9') {
            str[i] = (str[i] - '0' - 13 + 10) % 10 + '0';
        }
    }
}
void again()
{
    int n1;
    printf("\nDo you want to try the program again?[1 for yes/0 for no]: ");
    scanf("%d", &n1);
    if (n1==1)
    {
        printf("\nStarting again\n");
        encrypt();
    }
    else
    {
        printf("\nExiting the program");
    }
}

void encrypt()
{
    // Ask for filename
    char filename[256];
    printf("Enter filename: ");
    scanf("%s", filename);

    // Open file
    FILE *file = fopen(filename, "r+");
    if (file == NULL) {
        printf("\nError opening file!Try again\n");
        again();
    }

    // Read file into a string
    // Move the file pointer to the end of the file
    fseek(file, 0, SEEK_END);
    // Get the current position of the file pointer (i.e., the length of the file)
    long length = ftell(file);
    // Move the file pointer back to the beginning of the file
    fseek(file, 0, SEEK_SET);
    // Allocate a string to hold the contents of the file
    char *text = malloc(length + 1);
     // Read the entire file into the string
    fread(text, 1, length, file);
    // Add a null terminator to the end of the string
    text[length] = '\0';

    // Encrypt the text
    rot13(text);

    // Overwrite file with encrypted text
    fseek(file, 0, SEEK_SET);
    fwrite(text, 1, length, file);
    fclose(file);


    //Decryption
    decrypt(text, length);
}

void decrypt(char text,int length)
{
    int n, n1;
    char filename[256];
    printf("Do you want to decrypt the file?:[1 for yes/0 for no] ");
    scanf("%d", &n);
    if (n==1)
    {
        printf("\nDo you want to write the decrpyted text to same file or new file[0 for different/anything else for same]: ");
        scanf("%d", &n1);
        if (n1==0)
        {
            printf("\nEnter filename: ");
            scanf("%s", filename);
        }
        else
        {
            printf("\nsaving to %s", filename);
        }
    }
    else if (n==0)
    {
        printf("\nExiting this session:...");
        again();
    }
    else{
        printf("\nwrong choice\n");
        printf("Exiting the session....\n");
        again();
    }


    // Open file2.txt for writing
    FILE *file2 = fopen(filename, "w");
    if (file2 == NULL) {
        printf("\nError opening file!Try again\n");
        again();
    }
    // Decrypt the text
    rot13d(text);

    // Write decrypted text to file2.txt
    fwrite(text, 1, length, file2);
    fclose(file2);
    free(text);
}

int main(void) {
    printf("Hello there. welcome to the program \n");
    //Encryption
    encrypt();
    return 0;
}
