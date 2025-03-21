---
layout: single
title: "[알고리즘] 소수 구하기 - 에라토스테네스의 체"
categories: Algorithm
tag: [Math, PrimeNumber, Algorithm, C++, CS]
toc: true
---

# 소수 구하기 - 에라토스테네스의 체

**소수(prime number)**는 자신보다 작은 2개의 자연수를 곱해 만들 수 없는 1보다 큰 자연수를 말한다. 즉, 1과 자기 자신 외에 약수가 존재하지 않는 수를 의미한다.

소수를 구하는 대표적인 방법으로는 **에라토스테네스의 체**가 있다.

## 에라토스테네스의 체 원리

1. **구하고자 하는 소수의 범위만큼 1차원 배열을 생성한다.**

 - 예시로 1-20까지의 자연수 중 소수를 구해보겠다.

 ![Alt text](/assets/Alimages/e1.png)
 
 - 먼저 주어진 범위까지 배열을 생성한다. 1은 소수가 아니므로 삭제하고, 2부터 시작한다.

2. **2부터 시작하고 현재 숫자가 지워진 상태가 아닌 경우 현재 선택된 숫자의 배수에 해당하는 수를 배열에서 끝까지 탐색하면서 지운다. 이때 처음으로 선택된 숫자는 지우지 않는다.**

 ![Alt text](/assets/Alimages/e2.png)

 - 선택한 수(2)의 배수를 모두 삭제한다. 2를 제외하고 2의 배수를 모두 삭제한다.

3. **배열의 끝까지 (2)를 반복한 후 배열에 남은 모든 수를 출력한다.**
 
 ![Alt text](/assets/Alimages/e3.png)

 - 다음 3으로 넘어가 3을 제외하고 3의 배수를 모두 삭제한다. (계속 반복)
 
 ![Alt text](/assets/Alimages/e4.png)

 - 배열 끝까지 반복한 후 남은 결과를 출력한다.
 - 즉, 1부터 20까지의 수 중 소수는 2,3,5,7,11,13,17,19이다.

## 코드 구현

 ```cpp
 #include <iostream>
 #include <cmath>
 using namespace std;

 int main(void){
    int A[21];

    for(int i=2; i<=20; i++){
        A[i]=i;
    }

    for(int i=2; i<=sqrt(20); i++){ //제곱근까지만 수행
        if(A[i]==0) continue;
        for(int j=i+i; j<=20; j=j+i){//배수 지우기
            A[j]=0;
        }
    }

    for(int i=2; i<=20; i++){
        if(A[i]!=0) cout<<A[i]<<"\n";
    }

    return 0;
 }
 ```

 ```
 //출력 결과
 2 3 5 7 11 13 17 19
 ```

 - **제곱근 까지만 탐색하는 이유** : 범위가 1~N까지라고 하면, N의 제곱근이 n일 때 N=a*b를 만족하는 a와 b가 모두 n보다 클 수는 없다. a가 n보다 크면 b는 n보다 작아야한다. 즉, N보다 작은 수 가운데 소수가 아닌 수는 항상 n보다 작은 약수를 가진다. 따라서 에라토스테네스의 체로 n이하의 수의 배수를 모두 제거하면 1부터 N 사이의 소수를 구할 수 있다.