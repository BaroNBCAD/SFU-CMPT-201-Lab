#define _DEFAULT_SOURCE
#include <stdint.h>
#include <stdio.h>
#include <stdlib.h>
#include <string.h>
#include <unistd.h>

struct header {
  uint64_t size;
  struct header *next;
};
const int BUF_SIZE = 100;

void handle_error(char *format) {
  perror(format);
  exit(1);
}

void print_out(char *format, void *data, size_t data_size) {
  char buf[BUF_SIZE];
  ssize_t len = snprintf(buf, BUF_SIZE, format,
                         data_size == sizeof(uint64_t) ? *(uint64_t *)data
                                                       : *(void **)data);
  if (len < 0) {
    handle_error("snprintf");
  }
  write(STDOUT_FILENO, buf, len);
}

int main() {
  void *start_address = sbrk(256);
  if (start_address == (void *)-1) {
    perror("sbrk failed");
    exit(1);
  }
  size_t block_size = 128;
  struct header *block1 = (struct header *)start_address;
  block1->size = block_size;
  block1->next = NULL;

  struct header *block2 = (struct header *)(start_address + block_size);
  block2->size = block_size;
  block2->next = block1;

  print_out("first block: %p\n", &block1, sizeof(&block1));
  print_out("second block: %p\n", &block2, sizeof(&block2));
  print_out("first block size: %d\n", &block1->size, sizeof(&block1->size));
  print_out("first block next: %p\n", &block1->next, sizeof(&block1->next));
  print_out("second block size: %d\n", &block2->size, sizeof(&block2->size));
  print_out("second block next: %p\n", &block2->next, sizeof(&block2->next));

  memset(block1 + 1, 0, block_size - sizeof(struct header));
  memset(block2 + 1, 1, block_size - sizeof(struct header));

  for (int i = 0; i < block_size - sizeof(struct header); i++) {
    uint64_t byte = ((char *)(block1 + 1))[i];
    print_out("%d\n", &byte, sizeof(&byte));
  }
  for (int i = 0; i < block_size - sizeof(struct header); i++) {
    uint64_t byte = ((char *)(block2 + 1))[i];
    print_out("%d\n", &byte, sizeof(&byte));
  }

  return 0;
}
