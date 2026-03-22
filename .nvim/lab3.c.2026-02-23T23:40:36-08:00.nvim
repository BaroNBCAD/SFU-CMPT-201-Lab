#define _POSIX_C_SOURCE 200809L
#include <stdio.h>
#include <stdlib.h>
#include <string.h>

char *getInput() {
  printf("Enter input: ");
  char *buff = NULL;
  size_t size = 0;
  ssize_t num_char = getline(&buff, &size, stdin);
  if (num_char != -1) {
    return buff;
  } else {
    free(buff);
    perror("getline failed");
    exit(EXIT_FAILURE);
  }
  return NULL;
}

void printHistory(char *history[], int index) {
  while (history[index] == NULL) {
    index = (index + 1) % 5;
  }
  for (int i = 0; history[index] != NULL && i < 5; i++) {
    printf("%s", history[index]);
    free(history[index]); // we free the character buff here
    history[index] = NULL;
    index = (index + 1) % 5;
  }
}

int main() {
  char *history[5] = {NULL};
  int index = 0;
  while (1) {
    char *buff = getInput();
    history[index] = buff;
    index = (index + 1) % 5;
    if (strcmp(buff, "print\n") == 0) {
      printHistory(history, index);
    }
  }
  return 0;
}
