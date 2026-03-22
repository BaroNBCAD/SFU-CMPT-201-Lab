#define _POSIX_C_SOURCE 200809L
#include <stdio.h>
#include <stdlib.h>
#include <string.h>
#include <unistd.h>

int main(void) {
  printf("Please enter some text: ");
  char *buff = NULL;
  size_t size = 0;
  ssize_t num_char = getline(&buff, &size, stdin);
  if (num_char != -1) {
    char *saveptr;
    char *ret;
    char *str = buff;
    while ((ret = strtok_r(str, " ", &saveptr))) {
      printf("%s\n", ret);
      str = NULL;
    }
    free(buff);
  } else {
    perror("getline failed");
    exit(EXIT_FAILURE);
  }
  return 0;
}
