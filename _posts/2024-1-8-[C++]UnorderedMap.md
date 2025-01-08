---
layout: single
title: "[C++] STL unordered_map 사용법"
categories: Coding
tag: [C++]
toc: true
---

# unordered_map 사용법

## unordered_map

- unordered_map은 C++ 표준 라이브러리(STL) <unordered_map>에 포함된 컨테이너로, 키-값 쌍(key-value pairs)을 저장하는 해시 테이블이다. 키를 기준으로 빠르게 데이터를 삽입, 삭제, 검색할 수 있다.

### 성능
 - unordered_map은 해시 테이블 기반이라서 대부분의 연산(삽입, 삭제, 검색)이 평균적으로 **O(1)**이다.
 - 해시 충돌이 발생하면 최악의 경우 **O(n)**의 시간 복잡도를 가질 수 있다.

### 주의 사항
 - unordered_map은 키의 순서를 보장하지 않는다. 만약 키를 정렬된 순서를 저장하고 싶다면 std::map을 사용하면된다.
 - unordered_map의 키는 해시 가능한 데이터여야한다. 기본적으로 **int, string, float**같은 데이터는 지원되지만, 사용자 정의 타입을 키로 사용하려면 해시 함수와 비교 함수를 정의해야한다.

## 기본 사용법

```cpp
#include <iostream>
#include <unordered_map>
using namespace std;

int main() {
    // unordered_map 선언
    unordered_map<string, int> umap;

    // 데이터 삽입
    umap["apple"] = 3;
    umap["banana"] = 5;
    umap["orange"] = 2;

    // 키를 이용한 값 접근
    cout << "apple: " << umap["apple"] << endl;

    // 키가 존재하는지 확인
    if (umap.find("banana") != umap.end()) {
        cout << "banana exists in the map." << endl;
    }

    // 데이터 삭제
    umap.erase("orange");

    // 데이터 순회
    for (const auto &pair : umap) {
        cout << pair.first << ": " << pair.second << endl;
    }

    return 0;
}
```

## 주요 메서드

```cpp
umap[key] // 키를 통해 값에 접근하거나, 새로운 키-값 쌍을 삽입한다.

umap.at(key) // 키를 통해 값에 접근한다. 키가 존재하지 않으면 예외를 던진다.

umap.insert({key, value}) // 키-값 쌍을 삽입한다.

umap.erase(key) // 특정 키를 가진 요소를 삭제한다.

umap.find(key) // 특정 키를 찾고, 존재하면 해당 요소의 반복자를 반환한다.

umpa.size() //요소의 개수를 반환한다.

umap.clear() // 모든 요소를 삭제한다.

umap.empty() // unordered_map가 비어 있는지 확인한다.
```

## 사용 예제

### 1. 값 삽입 및 접근

```cpp
unordered_map<string, int> umap;
umap["apple"] = 10; // 키가 "apple"인 값에 10 저장
umap["banana"] = 20;

// 값 접근
cout << umap["apple"] << endl; // 출력: 10
cout << umap.at("banana") << endl; // 출력: 20

// 없는 키에 접근하면 새로 추가 (초기값 0)
cout << umap["orange"] << endl; // 출력: 0 (추가됨)
```

### 2. 키 존재 여부 확인

```cpp
if (umap.find("apple") != umap.end()) {
    cout << "apple exists!" << endl;
} else {
    cout << "apple does not exist!" << endl;
}
```

### 3. 데이터 순회

- 출력 순서는 삽입 순서와 관계없이 랜덤이다.(해시 테이블 기반)

```cpp
unordered_map<string, int> umap = {{"apple", 3}, {"banana", 5}, {"cherry", 7}};

for (const auto &pair : umap) {
    cout << pair.first << ": " << pair.second << endl;
}
```

### 4. 데이터 삭제

```cpp
unordered_map<string, int> umap = {{"apple", 3}, {"banana", 5}, {"cherry", 7}};

// 특정 키 삭제
umap.erase("banana");

// 모든 데이터 삭제
umap.clear();

```

## 예제: 최빈값 찾기

```cpp
#include <iostream>
#include <unordered_map>
#include <vector>
using namespace std;

int main() {
    vector<int> nums = {1, 2, 2, 3, 3, 3, 4};

    unordered_map<int, int> freq;
    for (int num : nums) {//벡터 nums의 모든 요소를 반복
        freq[num]++;
        /*
        - 현재 숫자 num의 빈도를 1 증가시킨다.
        - freq에 해당 숫자가 없으면 자동으로 초기값 0이 할당된 후 1증가한다.

        freq[1]->1
        freq[2]->2
        freq[3]->3
        freq[4]->1
        */
    }

    // 최빈값 찾기
    int maxFreq = 0;//현재 까지 발견된 최대 빈도 저장 (초기값은 0)
    int mode = 0;//최빈값을 저장 (초기값은 0)
    for (const auto &pair : freq) {//freq의 모든 키-값 쌍을 반복
        if (pair.second > maxFreq) {//현재 숫자의 빈도가 maxFreq보다 크면 최빈값 후보 업데이트
            maxFreq = pair.second;//해당 숫자의 빈도, maFreq로 업데이트
            mode = pair.first;//숫자, 최빈값으로 업데이트
        }
    }

    cout << "Mode: " << mode << " (Frequency: " << maxFreq << ")" << endl;

    return 0;
}

```