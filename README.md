# cplus.h

**cplus.h** is a lightweight, header-only C library designed to make programming in C much easier.
The dynamic array and string builder implementations are inspired by *tsoding*.

## How to use

## Dynamic Array

```c
// Dynamic arrays must be zero-initialized,
// just like everything else in this library
DA(int) nums = {0};

// To append an item, provide a pointer to the DA and the value
da_append(&nums, 10);
da_append(&nums, 20);

// Access items
printf("%d\n", nums.items[0]);

// Free the dynamic array
da_free(&nums);
````

## Hash Table

This library provides hash tables that aim to be both **generic** and **easy to use**.

### How it works

1. Define your hash table type using `HT_DECL` (usually in a header file).
2. Implement it using `HT_IMPL` in a `.c` file.
3. You must provide:

   * a hash function (`hashf`)
   * a comparison function (`compare`)

The library provides helper functions to simplify hash implementation:

* `numhash`
* `strhash`
* `hash_combine`

```c
// HT contains both HT_DECL and HT_IMPL
HT(HashMap, char*, int);

// Implement the hash function
u32 HashMap_hashf(char *key) {
    return strhash(key);
}

// Implement the comparison function
int HashMap_compare(char *a, char *b) {
    return strcmp(a, b);
}

int main(void) {
    HashMap hm = {0};

    HashMap_add(&hm, "one", 1);
    HashMap_add(&hm, "two", 2);

    // ht_get returns a pointer to the value.
    // If the key does not exist, it returns NULL.
    int *one = HashMap_get(&hm, "one");
    int *two = HashMap_get(&hm, "two");
    int *six = HashMap_get(&hm, "six");

    if (one) printf("one: %d\n", *one);
    if (two) printf("two: %d\n", *two);
    if (six) printf("six: %d\n", *six);

    HashMap_free(&hm);
    return 0;
}
```

## String Builder

```c
StringBuilder sb = {0};

sb_append(&sb, "Hello, %s", "World");
sb_append(&sb, "!");

printf("%s\n", sb.items);

sb_free(&sb);
```

## Notes

* All structures **must be zero-initialized** before use.
* The library is **header-only** — just include `cplus.h`.
* Designed to stay close to idiomatic C while reducing boilerplate.
