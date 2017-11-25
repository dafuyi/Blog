## 由头

之前在《深入理解PHP内核》一书中看到了用函数作为结构体的属性，突发奇想用C语言来模拟实现面对对象。

## 具体实现

先贴上第一个版本的代码。

```c
#include <stdio.h>

void init() {
    printf("this is construct function\n");
}

void doSomething() {
    printf("this is a function to do something\n");
}

void destory() {
    printf("this is detory function\n");
}
struct Class {
    int pram;
    struct Class *this;
    void ( * _construct )();
    void ( * function )();
    void ( * _destory )();
};

int main() {
    struct Class Object = { 666, NULL, init, doSomething, destory };
    Object.this = &Object;
    Object._construct();
    Object.function();
    Object._destory();
    return 0;
}
#include <stdio.h>

typedef struct Class {
    int pram;
    void ( * _construct )();
    void ( * function )();
    void ( * _destory )();
} Class;

static Class *this;

void init() {
    printf("this is construct function\n");
}

void doSomething() {
    printf("this is a function to do something\n");
    this->_destory();
}

void destory() {
    printf("this is detory function\n");
}

int main() {
    struct Class Object = { 666, init, doSomething, destory };
    this = &Object;

    Object._construct();
    Object.function();
//    Object._destory();
    return 0;
}

```
## TODO

这里只是简单地，做了把函数设为了结构体的属性。

模拟了this指针。

标签: pointer, c
