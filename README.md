Hm

```css 
#include <stdint.h>
#include <stdio.h>
#include <string.h>
#include "message.h"

extern char *MESSAGE[32];
extern char *PREFIX[4];

extern uint8_t CATEGORY_MASK;
extern uint8_t PREFIX_MASK;
extern uint8_t PREDEFINED_MASK;

void print_character_array(uint8_t length, char data[]) {
    for (uint8_t i = 0; i < length; i++) {
        putchar(data[i]);
    }
}

uint8_t category(void *ptr) {
    return (*(uint8_t *)ptr) & CATEGORY_MASK;
}

uint8_t prefix(void *ptr) {
    return ((*(uint8_t *)ptr) & PREFIX_MASK) >> 1;
}

uint8_t predefined(void *ptr) {
    return ((*(uint8_t *)ptr) & PREDEFINED_MASK) >> 3;
}

size_t length(void *ptr) {
    if (category(ptr) == 1) {
        return strlen(MESSAGE[predefined(ptr)]);
    } else {
        uint8_t *bytes = (uint8_t *)ptr;
        return bytes[1];
    }
}
