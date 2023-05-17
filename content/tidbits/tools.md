+++
title = "Programming Tools"
+++

## Compare Without Overflow

```c
#include <stdio.h>
#include <stdlib.h>

#define CMP(a, b, key) (((a key) > (b key)) - ((a key) < (b key)))

int int_lt(const void *a, const void *b)
{
  return CMP(*(const int *)a,*(const int *)b,);
}

int main(void)
{
  int a = 3;
  int b = 5;

  switch (CMP(a,b,))
  {
    case -1:
      printf("a less than b\n");
      break;
    case 0:
      printf("a not greater, nor less than b\n");
      break;
    case 1:
      printf("a greater than b\n");
      break;
  }

  int arr[] = { 3, 1, 4, 1, 5, 9, 2, 6, 5, 3, 5 };
  const int arr_len = sizeof(arr) / sizeof(int);

  qsort(arr, arr_len, sizeof(int), int_lt);

  for (int i = 0; i < arr_len; ++i)
  {
    printf("%d,", arr[i]);
  }

  printf("\n");

  return 0;
}
```
