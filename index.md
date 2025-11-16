---
Author: Sebastian
Title: GINF Praesentation
---

# Stack 

```c
#include <stdio.h>

//Konstante Variable
#define SIZE 10

//Array bestehend aus Integer
int stack[SIZE];

//"Oberster" Wert ist bei Stack wichtig!
int top = -1;

//Funktion (void) --> ohne Rückgabewert
void push(int value) {

   if (top < SIZE - 1) {
      stack[++top] = value;
      printf("Pushed %d\n", value);
   }

   else {
      printf("Stack overflow! Cannot push %d\n", value);
   }
}

// Pop Methode,
int pop() {
   if (top >= 0) {
      printf("Popped %d\n", stack[top]);
      return stack[top--];
   }
   printf("Stack underflow! Nothing to pop.\n");
   return -1; // leer
}

//Methode um ein Stack auszugeben
void printStack() {
   if (top == -1) {
      printf("Stack is empty.\n");
      return;
   }
   printf("Current stack: ");
   for (int i = 0; i <= top; i++) {
      printf("%d ", stack[i]);
   }
   printf("\n");
}

int main() {
   push(5);
   push(9);
   push(2);

   printStack();

   pop();
   pop();

   printStack();

   pop();
   pop();

   return 0;
}

```

