#include <stdint.h>
#include <stdio.h>
#include <stdlib.h>

struct header {
  uint64_t size;
  struct header *next;
  int id;
};

void initialize_block(struct header *block, uint64_t size, struct header *next,
                      int id) {
  block->size = size;
  block->next = next;
  block->id = id;
}

int find_first_fit(struct header *free_list_ptr, uint64_t size) {
  while (free_list_ptr != NULL) {
    if (free_list_ptr->size >= size) {
      return free_list_ptr->id;
    }
    free_list_ptr = free_list_ptr->next; // this free_list_ptr is different from
                                         // one in main so no memory leaks
  }
  return -1;
}

int find_best_fit(struct header *free_list_ptr, uint64_t size) {
  int best_fit_id = -1;
  int best_size = -1;
  while (free_list_ptr != NULL) {
    if (free_list_ptr->size >= size) {
      if (best_size == -1) {
        best_fit_id = free_list_ptr->id;
        best_size = free_list_ptr->size;
      } else if (free_list_ptr->size < best_size) {
        best_fit_id = free_list_ptr->id;
        best_size = free_list_ptr->size;
      }
    }
    free_list_ptr = free_list_ptr->next;
  }
  return best_fit_id;
}

int find_worst_fit(struct header *free_list_ptr, uint64_t size) {
  int worst_fit_id = -1;
  int worst_size = -1;
  while (free_list_ptr != NULL) {
    if (free_list_ptr->size >= size) {
      if (worst_size == -1) {
        worst_fit_id = free_list_ptr->id;
        worst_size = free_list_ptr->size;
      } else if (free_list_ptr->size > worst_size) {
        worst_fit_id = free_list_ptr->id;
        worst_size = free_list_ptr->size;
      }
    }
    free_list_ptr = free_list_ptr->next;
  }
  return worst_fit_id;
}

int main(void) {

  struct header *free_block1 = (struct header *)malloc(sizeof(struct header));
  struct header *free_block2 = (struct header *)malloc(sizeof(struct header));
  struct header *free_block3 = (struct header *)malloc(sizeof(struct header));
  struct header *free_block4 = (struct header *)malloc(sizeof(struct header));
  struct header *free_block5 = (struct header *)malloc(sizeof(struct header));

  initialize_block(free_block1, 6, free_block2, 1);
  initialize_block(free_block2, 12, free_block3, 2);
  initialize_block(free_block3, 24, free_block4, 3);
  initialize_block(free_block4, 8, free_block5, 4);
  initialize_block(free_block5, 4, NULL, 5);

  struct header *free_list_ptr = free_block1;

  int first_fit_id = find_first_fit(free_list_ptr, 7);
  int best_fit_id = find_best_fit(free_list_ptr, 7);
  int worst_fit_id = find_worst_fit(free_list_ptr, 7);

  printf("The ID for First-Fit algorithm is: %d\n", first_fit_id);
  printf("The ID for Best-Fit algorithm is: %d\n", best_fit_id);
  printf("The ID for Worst-Fit algorithm is: %d\n", worst_fit_id);

  free(free_block1);
  free(free_block2);
  free(free_block3);
  free(free_block4);
  free(free_block5);
  return 0;
}

// When z is freed:
// If left side of z is a free block (in this case is m)
//    combine m and z into a big free block, with m is the ptr of the new block
// If right side of z is a free block (in this case is n)
//    combine z into n, with z is the ptr of the new block
//  in this right case: if n is head, set head to z
// Notice: these are not if else but continuos if: so say m combine z, then m
// combine n, then set head to m
