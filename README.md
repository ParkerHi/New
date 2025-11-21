Hm

```css 
#include <stdint.h>
#include <stddef.h>
#include <string.h>
#include <stdio.h>
#include "message.h"

extern char *MESSAGE[32];
extern char *PREFIX[4];

uint8_t CATEGORY_MASK   = (0x1 << 0);
uint8_t PREFIX_MASK     = (0x3 << 1);
uint8_t PREDEFINED_MASK = (0x1f << 3);

void print_character_array(uint8_t length, char data[]) {
    for (uint8_t i = 0; i < length; i++) {
        putchar(data[i]);
    }
}

uint8_t category(void *ptr) {
    uint8_t *byte = (uint8_t *)ptr;
    return (*byte & CATEGORY_MASK);
}

uint8_t prefix(void *ptr) {
    uint8_t *byte = (uint8_t *)ptr;
    return (*byte & PREFIX_MASK) >> 1;
}

uint8_t predefined(void *ptr) {
    uint8_t *byte = (uint8_t *)ptr;
    return (*byte & PREDEFINED_MASK) >> 3;
}

size_t length(void *ptr) {
    if (category(ptr) == 1) {
        uint8_t index = predefined(ptr);
        return strlen(MESSAGE[index]);
    } else {
        uint8_t *bytes = (uint8_t *)ptr;
        return bytes[1];
    }
}
