---
layout: single
title: "[자료구조] 스택(Stack)"
categories: DataStructure
tag: [Data Structure, Stack, CS]
toc: true
---

## 스택(Stack)이란?
- 스택(Stack)은 **후입선출(LIFO, Last In First Out)**방식으로 동작하는 선형 자료구조이다. 즉, 스택에 가장 마지막에 추가된 데이터가 가장 먼저 제거된다. 

### 스택의 ADT(추상 자료형)

```cpp
void StackInit(Stack *pstack);
- 스택의 초기화 진행
- 스택 생성 후 제일 먼저 호출되어야 하는 함수

int SIsEmpty(Stack *pstack);
- 스택이 빈 경우 TRUE(1)을, 그렇지 않은경우 FALSE(0)를 반환한다.

void SPush(Stack *pstack, Data data);
- 스택에  데이터를 저장한다. 매개 변수 data로 전달된 값을 저장한다.

Data SPop(Stack *pstack);
- 마지막에 저장된 요소를 삭제한다.
- 삭제된 데이터는 반환이 된다.
- 본 함수의 호출을 위해서는 데이터가 하나 이상 존재함이 보장되어야 한다.

Data SPeek(Stack *pstack);
- 마지막에 저장된 요소를 반환하되 삭제하지 않는다.
- 본 함수의 호출을 위해서는 데이터가 하나 이상 존재함이 보장되어야 한다.
```

### 스택의 특징

- **후입선출(LIFO)** : 마지막으로 삽입된 데이터가 가장 먼저 제거된다.
- **제한적인 접근** : 스택의 맨 위에 있는 데이터만 접근할 수 있다.
- **동적 크기** : 배열이나 연결 리스트로 구현될 경우, 크기가 동적으로 확장될 수 있댜.

### 스택의 동작(Push, Pop)

![Alt text](/assets/DSimages/stackworks.png)

## 스택의 배열 기반 구현
배열 기반 스택은 배열을 사용하여 데이터를 저장하고, **topIndex** 변수를 통해 스택의 현재 상태를 관리한다. 이 구현에서는 정적 배열의 크기를 **STACK_LEN**으로 설정한다. **topIndex**는 스택의 가장 마지막 데이터의 위치를 관리하며, 데이터를 삽입(push)할 때는 증가하고, 삭제는(pop)할 때는 감소한다. 이를 통해 효율적이고 간단한 스택 연산을 구현할 수 있다.

### 헤더 파일의 정의(ArrayBaseStack.h)

```c
#ifdef __AB_STACK_H
#define __AB_STACK_H

#define TRUE 1
#define FALSE 0
#define STACK_LEN 100

typedef int Data;

typedef struct _arrayStack{
    Data stackArr[STACK_LEN];
    int topIndex;
} ArrayStack;

typedef ArrayStack Stack;

void stackInit(Stack *pstack); // 스택의 초기화
int SIsEmpty(Stack *pstack); // 스택이 비어있는지 확인

void SPush(Stack *pstack, Data data);// 스택의 push 연산
Data SPop(Stack *pstack);//스택의 pop 연산
Data SPeek(Stack *pstack);//스택의 peek 연산

#endif
```

### 배열 기반 스택의 구현

스택을 표현한 다음 구조체의 정의를 보면 **StackInit** 함수를 무엇으로 채워야하는지 알 수 있다.

```c
typedef struct _arrayStack{
    Data stackArr[STACK_LEN];
    int topIndex;
} ArrayStack;
```

이 중에서 초기화할 멤버는 가장 마지막에 저장된 데이터의 위치를 가리키는 topIndex 하나이다. 따라서 StackInit 함수는 다음과 같이 정의된댜.

```c
void StackInit(Stack *pstack){
    pstack->topIndex = -1 //빈 상태를 의미
}
```

topindex에 0이 저장되면, 이는 인덱스 0의 위치에 마지막 데이터가 저장되었음을 의미한다. 따라서 topIndex를 0이 아닌 -1로 초기화 해야한다.

이어서 SIsEmpty 함수의 정의이다. 이 함수는 스택이 비어있는지 확인할 때 호출하는 함수이다. 스택이 비어있는 경우 topIndex의 값이 -1 이다.

```c
int SIsEmpty(Stack *pstack){
    if(pstack->topIndex == -1) //스택이 비어있다면
        return TRUE;
    else
        return FALSE;
}
```

다음은 스택의 핵심인 SPush 와 SPop 함수이다.

```c
void SPush(Stack *pstack, Data data) // push 연산 함수
{
    pstack->topIndex += 1; // 데이터 추가를 위해 인덱스 값 증가
    pstack->stackArr[pstack->topIndex] = data; //데이터 저장
}

Data SPop(Stack *stack) //pop연산 함수
{
    int rIdx;

    if(IsEmpty(pstack)){
        printf("Stack Memory Error!");
        exit(-1);
    }

    rIdx = pstack->topIndex;//삭제할 데이터가 저장된 인덱스 값 저장
    pstack->topIndex -= 1;//pop 연산의 결과로 topIndex 값 하나 감소

    return pstack->stackArr[rIdx];//삭제되는 데이터 반환
}
```

현재 구현 중인 스택은 topIndex 값을 기반으로 데이터를 관리하므로, 삭제 시 **pstack->topIndex -= 1;**로 해당 값을 감소시키는 것만으로도 데이터가 삭제된 것으로 처리된다.

다음은 SPeek 함수이다. 해당 함수는 SPop 함수와 달리 반환한 데이터를 소멸시키지 않으므로 다음과 같이 간단히 정의가 가능하다.

```c
Data SPeek(Stack *pstack) {
    if(SIsEmpty(pstack)){
        printf("Stack Memory Error!");
        exit(-1);
    }

    return pstack->stackArr[pstack->topIndex];//멘 위에 저장된 데이터 반환
}
```

### 전체 코드(배열 기반 스택)

**ArrayBaseStack.c**

```c
#include <stdio.h>
#include <stdlib.h>
#include "ArrayBaseStack.h"//위에서 정의한 헤더파일

void StackInit(Stack *pstack){
    pstack->topIndex = -1 //빈 상태를 의미
}

int SIsEmpty(Stack *pstack){
    if(pstack->topIndex == -1) //스택이 비어있다면
        return TRUE;
    else
        return FALSE;
}

void SPush(Stack *pstack, Data data) // push 연산 함수
{
    pstack->topIndex += 1; // 데이터 추가를 위해 인덱스 값 증가
    pstack->stackArr[pstack->topIndex] = data; //데이터 저장
}

Data SPop(Stack *stack) //pop연산 함수
{
    int rIdx;

    if(IsEmpty(pstack)){
        printf("Stack Memory Error!");
        exit(-1);
    }

    rIdx = pstack->topIndex;//삭제할 데이터가 저장된 인덱스 값 저장
    pstack->topIndex -= 1;//pop 연산의 결과로 topIndex 값 하나 감소

    return pstack->stackArr[rIdx];//삭제되는 데이터 반환
}

Data SPeek(Stack *pstack) {
    if(SIsEmpty(pstack)){
        printf("Stack Memory Error!");
        exit(-1);
    }

    return pstack->stackArr[pstack->topIndex];//멘 위에 저장된 데이터 반환
}
```

**ArrayBaseStackMain.c**

```c
#include <stdio.h>
#include "ArrayBaseStack.h"

int main(void){
    
    Stack stack;
    StackInit(&stack);

    SPush(&stack, 1); SPush(&stack, 2);
    Spush(&stack, 3); Spush(&stack, 4);
    Spush(&stack, 5);

    while(!SIsEmpty(&stack))
        printf("%d", SPop(&stack));

    return 0;
}
```

## 스택의 연결 리스트 기반 구현
연결 리스트를 사용하여 스택을 구현하면, 스택의 크기가 동적으로 조정이 가능하여, 배열 기반 스택에서 발생하는 고정된 크기 제한 문제를 해결할 수 있다. 

이 구현에서는 연결 리스트의 노드(Node)를 생성하고, 스택의 주요 연산인 push, pop, peek를 수행하는 메서드를 작성한다. 각 노드는 데이터와 다음 노드를 가리키는 포인터를 포함하며, 스택의 맨 위(Top)는 항상 가장 최근에 추가된 노드를 가리킨다. 

### 헤더 파일의 정의(ListBaseStack.h)

```c
#ifndef __LB_STACK_H_
#define __LB_STACK_H_

#define TRUE 1
#define FALSE 0

typedef int Data;

typedef struct _node{ //연결 리스트의 노드를 표현한 구조체
    Data data;
    struct _node *next;
} Node;

typedef struct _listStack{ // 연결 리스트 기반 스택을 표현한 구조체
    Node *head;
} ListStack;

typedef ListStack Stack;

void StackInit(Stack *pstack);//스택 초기화
int SIsEmpty(Stack *pstack);//스택이 비어있는지 확인

void SPush(Stack *pstack, Data data);//스택의 push 연산
Data SPop(Stack *pstack);//스택의 pop 연산
Data SPeek(Stack *pstack);//스택의 peek 연산

#endif
```

### 연결 리스트 기반 스택의 구현

포인터 변수 head는 새로 추가된 노드를 가리켜야 하므로, 비어 있는 상태를 표현하기 위해서 NULL로 초기화를 진행한다.

```c
void StackInit(Stack *pstack){
    pstack->head=NULL;
}

int SIsEmpty(Stack *pstack){
    if(pstack->head == NULL) //스택이 비어있는 경우
        return TRUE;
    else
        return FALSE;
}
```

```c
void SPush(Stack *pstack, Data data){
    Node *newNode = (Node*)malloc(sizeof(Node)); // 새 노드 생성

    newNode->data = data; //새 노드에 데이터 저장
    newNode->next = pstack->head; // 새 노드가 최근에 추가된 노드를 가리킴

    pstack->head = newNode; // 포인터 변수 head가 새 노드를 가리킴
}
```

SPop 함수는 포인터 변수 head가 가리키는 노드를 소멸시키고, 소멸된 노드의 데이터를 반환해야 한다.

```c
Data SPop(Stack *pstack){
    Data rdata;
    Node *rnode;

    if(SIsEmpty(pstack)){
        printf("Stack Memory Error!");
        exit(-1);
    }

    rdata = pstack->head->data;//삭제할 노드의 데이터를 임시로 저장
    rnode = pstack->head;//삭제할 노드의 주소 값을 임시로 저장

    pstack->head = pstack->head->next;//삭제할 노드의 다음 노드를 head가 가리킴
    free(node);//노드 삭제
    return data;//삭제된 노드의 데이터 반환
}

Data SPeek(Stack *pstack){
    if(SIsEmpty(pstack)){
        printf("Stack Memory Error!");
        exit(-1);
    }

    return pstack->head->data; //head가 가리키는 노드에 저장된 데이터 반환
}
```

### 전체 코드(연결 리스트 기반 스택)

**ListBaseStack.c**

```c
#include <stdio.h>
#include <stdlib.h>
#include "ListBaseStack.h"

void StackInit(Stack *pstack){
    pstack->head=NULL;
}

int SIsEmpty(Stack *pstack){
    if(pstack->head == NULL) //스택이 비어있는 경우
        return TRUE;
    else
        return FALSE;
}

void SPush(Stack *pstack, Data data){
    Node *newNode = (Node*)malloc(sizeof(Node)); // 새 노드 생성

    newNode->data = data; //새 노드에 데이터 저장
    newNode->next = pstack->head; // 새 노드가 최근에 추가된 노드를 가리킴

    pstack->head = newNode; // 포인터 변수 head가 새 노드를 가리킴
}

Data SPop(Stack *pstack){
    Data rdata;
    Node *rnode;

    if(SIsEmpty(pstack)){
        printf("Stack Memory Error!");
        exit(-1);
    }

    rdata = pstack->head->data;//삭제할 노드의 데이터를 임시로 저장
    rnode = pstack->head;//삭제할 노드의 주소 값을 임시로 저장

    pstack->head = pstack->head->next;//삭제할 노드의 다음 노드를 head가 가리킴
    free(node);//노드 삭제
    return data;//삭제된 노드의 데이터 반환
}

Data SPeek(Stack *pstack){
    if(SIsEmpty(pstack)){
        printf("Stack Memory Error!");
        exit(-1);
    }

    return pstack->head->data; //head가 가리키는 노드에 저장된 데이터 반환
}
```

**ListBaseStackMain.c**

```c
#include <stdio.h>
#include "ListBaseStack.h"

int main(void){
    Stack stack;
    Stack Init(&stack);

    SPush(&stack, 1); SPush(&stack, 2);
    Spush(&stack, 3); Spush(&stack, 4);
    Spush(&stack, 5);

    while(!SIsEmpty(&stack))
        printf("%d", SPop(&stack));

    return 0;
}
```



