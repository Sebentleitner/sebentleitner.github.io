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
      printf("Hinugefuegt %d\n", value);
   }

   else {
      printf("Stack overflow! Cannot push %d\n", value);
   }
}

// Pop Methode,
int pop() {
   if (top >= 0) {
      printf("Geloescht %d\n", stack[top]);
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

**Die Ausgabe des Codes schaut wie folgt aus.**
![Stack01](/img/AusgabeStack.png)

## Queue

```c
#include <stdio.h>

//Konstante Variable
#define SIZE 10

//Array von Ganzzahlen
int queue[SIZE];

//Wichtigesten Werte
int front = 0;
int rear = -1;
int count = 0;


void enqueue(int value) {
    if (count < SIZE) {
        rear = (rear + 1) % SIZE;
        queue[rear] = value;
        count++;
        printf("Enqueued %d\n", value);
    } else {
        printf("Queue overflow! Cannot enqueue %d\n", value);
    }
}

int dequeue() {
    if (count > 0) {
        int value = queue[front];
        front = (front + 1) % SIZE;
        count--;
        printf("Dequeued %d\n", value);
        return value;
    }
    printf("Queue underflow! Nothing to dequeue.\n");
    return -1;
}

void printQueue() {
    if (count == 0) {
        printf("Queue is empty.\n");
        return;
    }

    printf("Current queue: ");
    for (int i = 0; i < count; i++) {
        printf("%d ", queue[(front + i) % SIZE]);
    }
    printf("\n");
}

int main() {
    // Einfügen (enqueue)
    enqueue(5);
    enqueue(9);
    enqueue(2);

    printQueue(); // → 5 9 2

    // Entfernen (dequeue)
    dequeue();
    dequeue();

    printQueue();

    dequeue();
    dequeue();

    return 0;
}



```

![Queue01](/img/AusgabeQueue.png)


## Stack (Advanced)

```c
#include <stdio.h>
#include <string.h>

#define MAX 100
#define STRLEN 100

typedef struct {
    char data[MAX][STRLEN];
    int top;
} Stack;

// --- Stack functions ---
void init(Stack *s) {
    s->top = -1;
}

int isEmpty(Stack *s) {
    return s->top == -1;
}

int isFull(Stack *s) {
    return s->top == MAX - 1;
}

void push(Stack *s, const char *item) {
    if (!isFull(s)) {
        strcpy(s->data[++s->top], item);
    }
}

char* pop(Stack *s) {
    if (!isEmpty(s)) {
        return s->data[s->top--];
    }
    return NULL;
}

char* peek(Stack *s) {
    if (!isEmpty(s)) {
        return s->data[s->top];
    }
    return NULL;
}

// --- Application example ---
int main() {
    Stack undoStack, redoStack;
    init(&undoStack);
    init(&redoStack);

    push(&undoStack, "Hello");
    push(&undoStack, "Hello World");
    push(&undoStack, "Hello World!!!");

    printf("Current text: %s\n", peek(&undoStack));

    printf("\nUndo:\n");
    push(&redoStack, pop(&undoStack));
    printf("Text is now: %s\n", peek(&undoStack));

    printf("\nRedo:\n");
    push(&undoStack, pop(&redoStack));
    printf("Text is now: %s\n", peek(&undoStack));

    return 0;
}


```


## Queue (Advanced)


```c
#include <stdio.h>
#include <string.h>

#define SIZE 5
#define STRLEN 100

typedef struct {
    char data[SIZE][STRLEN];
    int front, rear, count;
} Queue;

void initQueue(Queue *q) {
    q->front = 0;
    q->rear = -1;
    q->count = 0;
}

int isFullQueue(Queue *q) {
    return q->count == SIZE;
}

int isEmptyQueue(Queue *q) {
    return q->count == 0;
}

void enqueue(Queue *q, const char *job) {
    if (isFullQueue(q)) {
        printf("Queue full: cannot add '%s'\n", job);
        return;
    }
    q->rear = (q->rear + 1) % SIZE;
    strcpy(q->data[q->rear], job);
    q->count++;
    printf("Added job: %s\n", job);
}

void dequeue(Queue *q) {
    if (isEmptyQueue(q)) {
        printf("Queue empty: no job to process.\n");
        return;
    }
    printf("Printing: %s\n", q->data[q->front]);
    q->front = (q->front + 1) % SIZE;
    q->count--;
}

void printQueue(Queue *q) {
    printf("Current print queue: ");
    if (isEmptyQueue(q)) {
        printf("[empty]\n");
        return;
    }
    for (int i = 0; i < q->count; i++) {
        printf("%s ", q->data[(q->front + i) % SIZE]);
    }
    printf("\n");
}

int main() {
    Queue q;
    initQueue(&q);

    enqueue(&q, "DocumentA.pdf");
    enqueue(&q, "Report.docx");
    enqueue(&q, "Invoice.png");

    printQueue(&q);

    dequeue(&q);
    dequeue(&q);

    enqueue(&q, "Slides.pptx");
    enqueue(&q, "Homework.cpp");
    enqueue(&q, "TooMuch.txt"); // overflow test

    printQueue(&q);

    return 0;
}


```




