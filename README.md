Hm

```css 
#include <stdint.h>
#include <string.h>
#include <stdio.h>
#include "message.h"

extern char *MESSAGE[32];
extern char *PREFIX[4];

void print_character_array(uint8_t length, char data[]) {
    for (uint8_t i = 0; i < length; i++) {
        putchar(data[i]);
    }
}

uint8_t category(void *ptr) {
    uint8_t *byte_ptr = (uint8_t *)ptr;
    return (*byte_ptr) & 0x1;
}

uint8_t prefix(void *ptr) {
    uint8_t *byte_ptr = (uint8_t *)ptr;
    return ((*byte_ptr) >> 1) & 0x3;
}

uint8_t predefined(void *ptr) {
    uint8_t *byte_ptr = (uint8_t *)ptr;
    return ((*byte_ptr) >> 3) & 0x1f;
}

size_t length(void *ptr) {
    if (category(ptr) == 1) {
        uint8_t index = predefined(ptr);
        return strlen(MESSAGE[index]);
    } else {
        uint8_t *byte_ptr = (uint8_t *)ptr;
        return byte_ptr[1];
    }
}
