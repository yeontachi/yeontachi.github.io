---
layout: single
title: "[자료구조] 연결 리스트(Linked List)-(1)"
categories: DataStructure
tag: [Data Structure, List, Linked List, CS]
toc: true
---

## 연결 리스트 (Linked List)

 연결리스트(Linked List)는 **데이터를 순차적으로 저장**하지만, **배열과 달리 물리적인 메모리 상에서 연속적으로 배치되지 않는 자료구조**이다. 연결리스트는 각 데이터 요소(Node)가 **데이터 값과 다음 노드를 가리키는 포인터(참조)**를 포함하고 있는 구조로 구성된다.

 앞서 설명한 **배열 기반 리스트**는 메모리의 특성이 정적이어서(길이 변경이 불가능) 메모리의 길이를 변경하는 것이 불가능 헀다. 즉, 정적인 배열은 필요로 하는 메모리의 크기에 유연하게 대처하지 못한다. 이 문제를 해결하기 위해 등장한 것이 **동적인 메모리 구성**이다.

### 노드(Node)
 - 연결리스트의 기본 단위로, 다음 두 가지 요소로 구성된다.
    - **데이터(data)** : 저장할 값
    - **포인터(next)** : 다음 노드의 주소를 저장
 - 노드는 연결리스트의 **구조를 유지하는 핵심 요소**로, 각각의 노드가 서로 연결되며 전체 리스트를 형성한다.

    ![Alt text](/assets/DSimages/linkedlistnode.png)

 - 다음은 노드를 표현한 구조체의 정의이다.

```c
typedef struct _node{ //typedef int LData
    LData data; // 현재 노드가 저장하는 실제 값
    struct _node *next; // 다음 노드를 가리키는 포인터(다음 노드의 주소 저장)
}
```

#### Head와 Tail 노드
 - **Head 노드**
    - 연결리스트의 **시작점**을 나타낸다.
    - head포인터는 첫 번째 노드를 가리키며, 이를 통해 연결리스트를 탐색하거나 조작할 수 있다.
    - 헤드 자체는 노드가 아닐 수도 있고, 연결리스트의 첫 번째 노드를 의미하는 경우도 있다.
 - **Tail 노드**
    - 연결리스트의 **끝 지점**을 나타낸다.
    - 마지막 노드의 next는 항상 NULL을 가리킨다.(단일 연결리스트의 경우)
    - 효율적인 추가를 위해 tail 포인터를 따로 관리하여 마지막 노드에 직접 접근할 수 있도록 한다.

- **노드의 배치와 연결 방식**
    - 메모리 상에서 **비연속적**으로 저장되지만, next 포인터를 통해 각 노드가 연결된다.

    ![Alt text](/assets/DSimages/LinkedLIstNode2.png)

    - **head** : 첫 번째 노드의 주소를 저장하는 포인터
    - **[ data | next ]** : 데이터를 저장하고 다음 노드를 가리킨다.
    - 마지막 노드의 next는 **NULL**을 가리켜 끝임을 나타낸다. 

- 삽입과 삭제, 조회 등은 아래 코드 구현하기에서 설명할 예정

#### 노드 동작 방식 예제 : 노드 생성 및 연결

```c
#include <stdio.h>
#include <stdlib.h>

typedef int LData;

typedef struct _node {
    LData data;
    struct _node *next;
} Node;

int main() {
    // 1. 노드 생성
    Node *head = (Node *)malloc(sizeof(Node));  // 첫 번째 노드
    Node *second = (Node *)malloc(sizeof(Node)); // 두 번째 노드
    Node *third = (Node *)malloc(sizeof(Node));  // 세 번째 노드

    // 2. 데이터 저장
    head->data = 10;
    head->next = second;

    second->data = 20;
    second->next = third;

    third->data = 30;
    third->next = NULL; // 마지막 노드

    // 3. 순회 및 출력
    Node *current = head;
    while (current != NULL) {
        printf("%d -> ", current->data);
        current = current->next; // 다음 노드로 이동
    }
    printf("NULL\n");

    // 4. 메모리 해제
    free(third);
    free(second);
    free(head);

    return 0;
}
```

```
출력결과 : 10 -> 20 -> 30 -> NULL
```

- **노드 관리의 중요성**
    - 연결리스트의 구조를 유지하려면 각 노드의 next 포인터를 올바르게 관리해야 한다.
    - 동적 메모리 할당으로 노드를 효율적으로 추가하거나 제거할 수 있다.
    - 삽입과 삭제 위치를 자유롭게 지정할 수 있어 다양한 알고리즘과 데이터 구조를 구현하는 데 적합하다.

## 연결리스트 ADT

```c
void ListInit(List *plist);
- 리스트를 초기화한다.
- 리스트 생성 후 가장 먼저 호출되어야 한다.

void LInsert(List *plist, LData data);
- 리스트에 데이터를 저장한다. 매개변수 data에 전달된 값을 저장한다.

int LFirst(List *plist, LData *pdata);
- 첫 번째 데이터를 참조하여 pdata가 가리키는 메모리에 저장한다.
- 참조 성공 시 TRUE(1), 실패 시 FALSE(0) 반환.

int LNext(List *plist, LData *pdata);
- 참조된 데이터의 다음 데이터를 pdata가 가리키는 메모리에 저장한다.
- 순차적인 참조를 위해 반복 호출 가능.
- 참조를 새로 시작하려면 LFirst를 호출해야 한다.
- 참조 성공 시 TRUE(1), 실패 시 FALSE(0) 반환.

LData LRemove(List *plist);
- 마지막으로 반환된 데이터를 삭제하고, 삭제된 데이터를 반환한다.
- 연속적인 호출이 불가능하다.

int LCount(List *plist);
- 리스트에 저장된 데이터의 개수를 반환한다.
```

## 연결리스트 구현하기

### 헤더 파일 정의

```c
#ifndef __LINKED_LIST_H__
#define __LINKED_LIST_H__

#define TRUE  1
#define FALSE 0

typedef int LData;

// 리스트 구조체 및 함수 선언
typedef struct _node {
    LData data;
    struct _node *next;
} Node;

typedef struct _linkedList {
    Node *head;
    Node *tail;
    Node *cur;
    int numOfData;
} LinkedList;

typedef LinkedList List;

// 함수 선언
void ListInit(List *plist);
void LInsert(List *plist, LData data);  // 리스트 끝에 데이터 삽입
int LFirst(List *plist, LData *pdata); // 첫 번째 데이터 참조
int LNext(List *plist, LData *pdata);  // 다음 데이터 참조
LData LRemove(List *plist);            // 마지막 참조 데이터를 삭제
int LCount(List *plist);               // 데이터 개수 반환

#endif
```

### 연결리스트에서의 데이터 삽입

**리스트 초기화** 

```c
void ListInit(List *plist) {
    plist->head = NULL;  // 리스트의 처음을 NULL로 설정
    plist->tail = NULL;  // 리스트의 끝도 NULL로 설정
    plist->cur = NULL;   // 현재 탐색 위치 초기화
    plist->numOfData = 0; // 데이터 개수 초기화
}
```

- 프로그램이 처음 시작되었을 때 구조체 Node의 포인터 변수 head와 tail이 NULL을 가리키고 있는 상황을 볼 수 있다.

![Alt text](/assets/DSimages/insert1.png)

**LInsert(데이터 삽입)**

- 저장되는 데이터를 2로 가정할 때 위 그림은 아래 세 문장을 실행한 결과이다.

```c
    Node *newNode = (Node *)malloc(sizeof(Node)); // 새 노드 생성
    newNode->data = data;  // 데이터 저장
    newNode->next = NULL;  // 새 노드는 리스트의 끝을 나타냄
```

- 위 상태에서 포인터 변수 head가 가리키는 것이 NULL이므로, 다음 구문을 실행하면

```c
    if (plist->head == NULL) { // 리스트가 비어 있는 경우
        plist->head = newNode; // 새 노드를 첫 번째 노드로 설정
        plist->tail = newNode; // 새 노드가 끝 노드가 됨
    } else { // 리스트에 데이터가 있는 경우
        plist->tail->next = newNode; // 끝 노드의 next가 새 노드를 가리킴
        plist->tail = newNode;       // tail이 새 노드를 가리키도록 업데이트
    }
```

- 다음 그림과 같이 head가 새로운 노드를 가리키는 상태가 된다.

![Alt text](/assets/DSimages/insert2.png)

- 이어서 plist->tail = newNode; 문장을 실행하면

![Alt text](/assets/DSimages/insert3.png)

- 이렇게 첫 번째 노드의 추가가 완료된 상태이다.

![Alt text](/assets/DSimages/insert4.png)

- 위 그림에서 주목할 사실은 head와 tail이 첫 번쨰 노드를 가리키고 있다는 점이다. head는 리스트의 머리를 가리키고 tail은 리스트의 꼬리를 가리킨다. 노드가 몇 개가 더 추가되건 변함이 없어야하는 사실이다.

- 두 번째 노드는 연결 리스트의 끝, 즉, tail이 가리키는 노드의 뒤에 연결되어야 하기 때문에 새 노드의 생성 및 초기화 이후에는 else 문에 담긴 문장을 실행하게 된다. 

```c
    else { // 리스트에 데이터가 있는 경우
        plist->tail->next = newNode; // 끝 노드의 next가 새 노드를 가리킴
        plist->tail = newNode;       // tail이 새 노드를 가리키도록 업데이트
    }
```

- 새 노드에 저장된 값을 4로 가정할 경우 그 결과는 아래와 같다.

![Alt text](/assets/DSimages/insert5.png)

- 연결 리스트에서 head는 첫 번째 노드를, 그리고 tail은 마지막 노드를 가리킨다. 그리고 추가되는 노드들은 꼬리에 꼬리를 물어서 저장되기 때문에 2,4,6,8이 순서대로 저장이 되면 아래와 같은 모습을 띠게 된다.

![Alt text](/assets/DSimages/insert6.png)

- **데이터 삽입 전체 코드**

```c
void LInsert(List *plist, LData data) {
    Node *newNode = (Node *)malloc(sizeof(Node)); // 새 노드 생성
    newNode->data = data;  // 데이터 저장
    newNode->next = NULL;  // 새 노드는 리스트의 끝을 나타냄

    if (plist->head == NULL) { // 리스트가 비어 있는 경우
        plist->head = newNode; // 새 노드를 첫 번째 노드로 설정
        plist->tail = newNode; // 새 노드가 끝 노드가 됨
    } else { // 리스트에 데이터가 있는 경우
        plist->tail->next = newNode; // 끝 노드의 next가 새 노드를 가리킴
        plist->tail = newNode;       // tail이 새 노드를 가리키도록 업데이트
    }

    (plist->numOfData)++; // 데이터 개수 증가
}
```

### 연결 리스트에서의 데이터 조회

 연결 리스트에서 데이터를 조회하려면 순차적으로 노드를 따라가면서 원하는 데이터를 찾아야 한다. 이를 위해 다음 두 가지 함수가 사용된다.
 - **LFirst** : 리스트에서 첫 번째 데이터를 참조하는 함수
 - **LNext** : 현재 위치에서 다음 데이터를 참조하는 함수

 **LFirst 함수**

 ```c
 int LFirst(List *plist, LData *pdata) {
    if (plist->head == NULL) // 리스트가 비어 있으면 FALSE 반환
        return FALSE;

    plist->cur = plist->head; // 현재 노드를 첫 번째 노드로 설정
    *pdata = plist->cur->data; // 첫 번째 데이터 저장
    return TRUE;
}
 ```

 - 먼저 **빈 리스트인지 확인**을 한다. (plist->head==NULL)이면 빈 리스트
 - 첫 번째 노드로 초기화
    - plist->cur 에 첫 번째 노드를 설정한다.
    - plist->head 를 통해 첫 번째 노드에 접근한다.
 - 데이터 반환
    - 첫 번째 노드의 데이터를 pdata에 저장한다.
    - TRUE를 반환하여 데이터 참조 성공을 알린다.

- 위 함수가 TRUE를 반환하면 아래와 같은 상태가 된다.

![Alt text](/assets/DSimages/LinkedListFirst.png)

**LNext 함수**

```c
int LNext(List *plist, LData *pdata) {
    if (plist->cur->next == NULL) // 다음 노드가 없으면 FALSE 반환
        return FALSE;

    plist->cur = plist->cur->next; // 현재 노드를 다음 노드로 이동
    *pdata = plist->cur->data; // 다음 노드의 데이터 저장
    return TRUE;
}
```

- 위 함수의 핵심은 다음 문장에 있다. **plist->cur = plist->cur->next;**
- 위 문장으로 인해서 cur은 아래와 같이 모든 노드를 가리키며 이동할 수 있게 된다.

![Alt text](/assets/DSimages/LinkedListNext.png)

- 이렇듯 연결리스트 구조체에서 생성된 **cur 노드**는 리스트 안을 돌아다닐 때 사용이 된다.

### 연결 리스트에서의 데이터 삭제

```c
    Node *rpos = plist->cur;  // 삭제할 노드를 가리킴
    LData rdata = rpos->data; // 삭제할 노드의 데이터
```

- cur 노드가 가리키고 있는 데이터를 삭제를 할 때, 삭제할 노드를 rpos에 저장해준다.

- 아래 그림에서 볼 수 있듯이 4가 저장된 노드의 주소 값을 아는 것은 2가 저장된 노드가 유일한데, 이 노드를 그냥 소멸시키면 4가 저장된 노드의 주소 값은 어디에도 존재하지 않게 된다. 즉, cur 노드가 가리키고 있는 데이터를 그냥 삭제해 버리면, 그 다음 노드에 접근이 불가능해지기 때문에 rpos에 저장을 해주는 것이다.

![Alt text](/assets/DSimages/LinkedListerror.png)

- 삭제할 데이터는 rpos->data에 저장해준다.

![Alt text](/assets/DSimages/LinkedListrpos.png)

1. **삭제할 노드가 첫 번째 노드인 경우**

```c
    if (rpos == plist->head) { // 삭제할 노드가 첫 번째 노드인 경우
        plist->head = plist->head->next; // head를 다음 노드로 이동
        if (plist->head == NULL)         // 리스트가 비어 있으면 tail도 NULL
            plist->tail = NULL;
    } 
```
- 위 코드를 실행하면 아래와 같은 상태가 된다.

![Alt text](/assets/DSimages/LinkedListdel1.png)

2. **중간에 있는 노드를 삭제하는 경우**

```c
else { // 삭제할 노드가 첫 번째 노드가 아닌 경우
        Node *prev = plist->head; // 이전 노드를 찾음
        while (prev->next != plist->cur)
            prev = prev->next;

        prev->next = plist->cur->next; // 이전 노드의 next가 삭제할 노드의 다음 노드를 가리킴
        if (plist->cur == plist->tail) // 삭제할 노드가 끝 노드인 경우
            plist->tail = prev;        // tail을 이전 노드로 업데이트
    }
```

- prev는 head가 가리키는 노드부터 시작하여 while문을 돌면서 cur가 가리키는 노드 이전 노드까지 탐색을 한다.

![Alt text](/assets/DSimages/LinkedListdel2.png)

- cur 이전 노드에 도착했으면 prev가 가리키는 노드의 다음 노드를 cur가 가리키는 노드의 다음 노드로 설정을 해준다.

![Alt text](/assets/DSimages/LinkedListdel3.png)

- 이후 현재 노드를 첫 번째 노드로 초기화하고, 메모리 해제를 해주면 최종적으로 아래와 같은 상태가 된다.

![Alt text](/assets/DSimages/LinkedListdel4.png)

## 전체 코드

**LinkedList.c**

```c
#include <stdio.h>
#include <stdlib.h>
#include "LinkedList.h"

// 리스트 초기화
void ListInit(List *plist) {
    plist->head = NULL;  // 리스트의 처음을 NULL로 설정
    plist->tail = NULL;  // 리스트의 끝도 NULL로 설정
    plist->cur = NULL;   // 현재 탐색 위치 초기화
    plist->numOfData = 0; // 데이터 개수 초기화
}

// 리스트 끝에 데이터 삽입
void LInsert(List *plist, LData data) {
    Node *newNode = (Node *)malloc(sizeof(Node)); // 새 노드 생성
    newNode->data = data;  // 데이터 저장
    newNode->next = NULL;  // 새 노드는 리스트의 끝을 나타냄

    if (plist->head == NULL) { // 리스트가 비어 있는 경우
        plist->head = newNode; // 새 노드를 첫 번째 노드로 설정
        plist->tail = newNode; // 새 노드가 끝 노드가 됨
    } else { // 리스트에 데이터가 있는 경우
        plist->tail->next = newNode; // 끝 노드의 next가 새 노드를 가리킴
        plist->tail = newNode;       // tail이 새 노드를 가리키도록 업데이트
    }

    (plist->numOfData)++; // 데이터 개수 증가
}

// 첫 번째 데이터 참조
int LFirst(List *plist, LData *pdata) {
    if (plist->head == NULL) // 리스트가 비어 있으면 FALSE 반환
        return FALSE;

    plist->cur = plist->head; // 현재 노드를 첫 번째 노드로 설정
    *pdata = plist->cur->data; // 첫 번째 데이터 저장
    return TRUE;
}

// 다음 데이터 참조
int LNext(List *plist, LData *pdata) {
    if (plist->cur->next == NULL) // 다음 노드가 없으면 FALSE 반환
        return FALSE;

    plist->cur = plist->cur->next; // 현재 노드를 다음 노드로 이동
    *pdata = plist->cur->data; // 다음 노드의 데이터 저장
    return TRUE;
}

// 현재 데이터 삭제
LData LRemove(List *plist) {
    Node *rpos = plist->cur;  // 삭제할 노드를 가리킴
    LData rdata = rpos->data; // 삭제할 노드의 데이터

    if (rpos == plist->head) { // 삭제할 노드가 첫 번째 노드인 경우
        plist->head = plist->head->next; // head를 다음 노드로 이동
        if (plist->head == NULL)         // 리스트가 비어 있으면 tail도 NULL
            plist->tail = NULL;
    } else { // 삭제할 노드가 첫 번째 노드가 아닌 경우
        Node *prev = plist->head; // 이전 노드를 찾음
        while (prev->next != plist->cur)
            prev = prev->next;

        prev->next = plist->cur->next; // 이전 노드의 next가 삭제할 노드의 다음 노드를 가리킴
        if (plist->cur == plist->tail) // 삭제할 노드가 끝 노드인 경우
            plist->tail = prev;        // tail을 이전 노드로 업데이트
    }

    plist->cur = plist->head; // 현재 노드를 첫 번째 노드로 초기화
    free(rpos);               // 메모리 해제
    (plist->numOfData)--;     // 데이터 개수 감소

    return rdata; // 삭제된 데이터 반환
}

// 데이터 개수 반환
int LCount(List *plist) {
    return plist->numOfData;
}
```

**main.c**

```c
#include <stdio.h>
#include "LinkedList.h"

int main() {
    List list;
    int data;

    ListInit(&list);

    // 데이터 삽입
    LInsert(&list, 10);
    LInsert(&list, 20);
    LInsert(&list, 30);

    // 데이터 출력
    printf("현재 데이터 수: %d \n", LCount(&list));

    if (LFirst(&list, &data)) {  // 첫 번째 데이터 참조
        printf("%d ", data);

        while (LNext(&list, &data)) // 나머지 데이터 순차 참조
            printf("%d ", data);
    }
    printf("\n");

    // 20을 찾아 삭제
    if (LFirst(&list, &data)) {
        if (data == 20)
            LRemove(&list);

        while (LNext(&list, &data)) {
            if (data == 20)
                LRemove(&list);
        }
    }

    // 데이터 출력
    printf("현재 데이터 수: %d \n", LCount(&list));

    if (LFirst(&list, &data)) {
        printf("%d ", data);

        while (LNext(&list, &data))
            printf("%d ", data);
    }
    printf("\n");

    return 0;
}
```

