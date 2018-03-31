# safe-dynamic-array-c
Dynamic arrays in c including length info. Compatible with normal c arrays

``` c
#include <stdio.h>
#include <stdlib.h>
#include <limits.h>

void* Array(size_t size, int elements) {
    size_t content = size * elements;
    void* array = malloc(sizeof(size_t) + sizeof(int) + content);
    int* array_length = array;
    *array_length = elements;
    int* array_head = array + sizeof(size_t);
    *array_head = INT_MAX;
    return array + sizeof(size_t) + sizeof(int);
}

int arraylen(void* array) {
    int* array_head = array - sizeof(int);
    if (*array_head == INT_MAX) {
        int* array_length = array - sizeof(size_t) - sizeof(int);
        return *array_length;
    } else {
        return 0;
    }
    return 0;
}

void arrayfree(void* array) {
    if (!array)
        return;
    int* array_head = array - sizeof(int);
    if (*array_head == INT_MAX) {
        void* array_begin = array - sizeof(size_t) - sizeof(int);
        free(array_begin);
    }
}

int main() {
    int* integers = Array(sizeof(int), 3);
    integers[0] = 7;
    arrayfree(integers);
    return 0;
}

```
