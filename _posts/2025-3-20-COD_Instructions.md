---
layout: single
title: "[컴퓨터 구조] MIPS 기반 컴퓨터 명령어 체계"
categories: ComputerArchitecture
tag: [ComputerArchitecture, CS]
toc: true
---

# MIPS 
 
## 명령어 집합(Instruction Set) 소개
 컴퓨터의 명령어 집합(Instruction Set)은 컴퓨터가 실행할 수 있는 명령어들의 모음으로, 컴퓨터 마다 서로 다른 명령어 집합을 가지지만 여러 공통적인 특징이 존재한다. 초기 컴퓨터의 명령어 집합은 단순한 명령어로 구성되어 있었으며, 이를 통해 구현이 간단해지고 하드웨어 설계가 용이해지는 장점이 있었다. 현대의 컴퓨터 역시 많은 경우 단순한 명령어 집합을 유지하고 있으며, 이는 하드웨어 최적화의 성능 향상을 고려한 설계 방식으로 이어지고 있다.


## MIPS 산술 연산(Arithmetic Operations)
 MIPS의 산술 연산은 기본적으로 **덧셈(Add)**와 **뺄셈(Subtract)** 연산을 포함하며, 세 개의 피연산자(Operands)를 사용한다. 연산 형식은 **두 개의 Source Register**와 **하나의 Destination Register**로 구성된다.

### 기본 연산 형식
 - 모든 산술 연산은 아래와 같은 형태를 가진다.

```
add a, b, c  # a gets b + c
```
 
 - b와 c를 더한 결과를 a에 저장한다.
 - sub 연산도 동일한 방식으로 동작한다.

 - **MIPS 설계 원칙 1 : 단순함이 규칙성을 만든다(Simplicity favors regularity)**
    - 규칙성이 구현을 단순하게 함 -> 명령어 구조가 일관되면 하드웨어 설계가 쉬워진다.
    - 단순함이 높은 성능과 낮은 비용을 가능하게 한다. -> 복잡한 명령어보다 빠르게 처리 가능하다.

 - **연산 예제**

```c
f = (g + h) - (i + j);
// g+h의 결과와 i+j의 결과를 각각 계산한 후, 두 값을 뺸다.
```

```
add t0, g, h   # t0 = g + h
add t1, i, j   # t1 = i + j
sub f, t0, t1  # f = t0 - t1
```

## MIPS 명령어의 피연산자(Operands)와 메모리 구조

### 레지스터 피연산자(Register Operands)
 - 산술 연산(Arithmetic Instruction)은 **레지스터 피연산자**를 사용한다.
 - MIPS는 32개의 32비트 레지스터를 제공하며, 이를 **레지스터 파일(Register File)**이라고 한다.
 - 레지스터는 주로 **자주 접근하는 데이터**를 저장하는 용도로 사용된다.
 - 레지스터 번호 : 0 ~ 31
 - 32비트 데이터를 **워드(word)**라고 부른다.

 - **레지스터 명칭**
    - **임시 변수 저장용(temporary values)** : $t0 ~ $t9
    - **저장된 변수용(Saved Variables)** : $s0 ~ $s7

 - **MIPS 설계 원칙 2 : 작은 것이 빠르다(Smaller is Faster)**
    - 메인 메모리는 수백만 개의 주소를 가짐 -> 접근 속도가 느리다.
    - 반면, 레지스터는 빠르게 접근이 가능하여 성능 최적화에 유리하다.

 - **연산 예제**

```c
f = (g + h) - (i + j);
```

```
add $t0, $s1, $s2  
add $t1, $s3, $s4   
sub $s0, $t0, $t1  
```

  - $s1 ~ $s4에 각각 g,h,i,j 변수들이 저장됨
  - g+h, i+j의 연산은 임시 레지스터에 저장

### 메모리 피연산자(Memory Operands)
 - **배열, 구조체, 동적 데이터** 등의 **복합 데이터**는 **메인 메모리에 저장**한다.
 - MIPS 연산을 수행하려면
    1. 메모리에서 값을 레지스터로 불러온다(Load)
    2. 레지스터에서 연산을 수행한다.
    3. 결과를 다시 메모리에 저장한다.(Store)

 - **MIPS 메모리 구조**
    - 바이트 단위 주소 지정(Byte Addressed Memory) : 메모리는 8비트(1바이트) 단위로 주소가 지정된다.
    - 워드(Word) 정렬(Aligned) : 32비트(4바이트) 기준으로 정렬된다. -> 주소는 항상 4의 배수여야 한다.
    - MIPS는 빅 엔디안(Big Endian) 방식 사용

 - **메모리 연산 예제**

```c
g = h + A[8];
```

 - g -> $s1, h- > $s2, A의 베이스 주소 -> $s3
 - 배열 인덱스 8은 4 * 8 = 32 바이트 오프셋 필요 

 ![Alt text](/assets/CAimages/Baseregister.png)

```
lw $t0, 32($s3)  # 배열 A[8]의 값을 $t0로 로드
add $s1, $s2, $t0 # g = h + A[8]
```

 - $s3 Base register, 32는 offset이 됨

 ```c
 A[12] = h + A[8];
 ```

 - h -> $s2, A의 베이스 주소 -> $s3

```
lw $t0, 32($s3) # load word
add $t0, $s2, $t0
sw $t0, 48($s3) # store word
```

### 레지스터 vs 메모리
 - 레지스터 접근 속도가 메모리보다 훨씬 빠르다.
 - 메모리에서 연산하려면 Load 및 Store 명령어가 필요 -> 추가적인 명령어 실행 부담이 발생한다.
 - 컴파일러는 가능한 한 변수를 레지스터에 저장하고, 필요할 때만 메모리에 저장한다.(Spilling)

 - **최적화 원칙**
    - 자주 사용하는 변수는 **레지스터**에 저장한다.
    - 필요할 때만 메모리를 사용하여 불필요한 접근을 줄인다.

### Immediate Operands
 - 즉시 값(Constant)을 직접 명령어에 포함이 가능하다.

```
addi $s3, $s3, 4  # $s3 += 4
# addi : add immediate
```

 - MIPS에서는 subi(Substract Immediate) 명령어가 없으므로, **음수를 사용하여 해결**한다.

```
addi $s2, $s1, -1  # $s2 = $s1 - 1
```

 - **설계 원칙 3 : 일반적인 경우를 빠르게 처리(Make the common case fast)**
    - 작은 상수(Constant)는 프로그램에서 자주 사용된다.
    - 즉시 값(Immediate Operand)을 사용하면 **메모리 접근 없이 연산 가능** -> 속도 향상

### MIPS의 0번 레지스터($zero)
 - MIPS의 **$zero** 레지스터는 **항상 0을 저장**한다.
 - 덧셈, 이동, 초기화 등에 활용한다.

```
add $t2, $s1, $zero  # $t2 = $s1 (Move 연산)
```

 - $s1 값을 $s2로 복사하는 데 $zero 활용
 - MIPS에는 move 연산이 없다

## MIPS 명령어 표현 및 논리 연산

### 명령어 표현(Representing Instructions)

**기계어와 명령어 인코딩**
 - 명령어는 이진수(Binary)로 표현되며, 이를 기계어(Machine Code)라고 한다.
 - MIPS 명령어는 **32비트 길이의 명령어 워드(Instruction Word)**로 인코딩된다.
 - 소수의 형식(Format)만 사용하여 일관성을 유지하면서도 명령어 해석이 용이하도록 설계되었다.

**MIPS 레지스터 번호**
 - MIPS 레지스터는 숫자로 식별된다.
    - **$t0 ~ $t7** -> 레지스터 8 ~ 15
    - **$t8 ~ $t9** -> 레지스터 24 ~ 25
    - **$s0 ~ $s7** -> 레지스터 16 ~ 23

### MIPS 명령어 형식 R-format Instructions

 - **연산(Arithmetic), 논리 연산(Logical Operations), 쉬프트 연산(Shift Operations)** 등에 사용된다.

![Alt text](/assets/CAimages/R_format.png)

 - **op** : 연산 코드(opcode)
 - **rs** : 첫 번째 소스 레지스터
 - **rt** : 두 번째 소스 레지스터
 - **rd** : 결과가 저장될 레지스터
 - **shamt** : 쉬프트 연산 시 이동할 비트 수(기본값 00000)
 - **funct** : 기능 코드(opcode 확장)

### MIPS 명령어 형식 I-format Instructions

 - **즉시 값 연산(Immediate Arithmetic) 및 메모리 접근(Load/Store)**에 사용

![Alt text](/assets/CAimages/I_format.png)

 - **op** : 연산 코드(opcode)
 - **rs** : 기준 레지스터(Base Register)
 - **rt** : 목적지 레지스터(Destination Register)
 - **constant** : 16비트 즉시 값 또는 주소 오프셋(offset)

 - **설계 원칙4 : 좋은 설계는 타협을 필요로 한다.**
    - 다양한 형식을 사용하면 명령어 해석이 복잡해진다. 하지만 32비트 명령어를 유지하기 위해 필수적이다.

 ![Alt text](/assets/CAimages/formatEx.png)

### 논리 연산(Logical Operations)
 - 비트 단위 조작을 위한 명령어 제공

 ![Alt text](/assets/CAimages/LogicalOperations.png)

### 쉬프트 연산(Shift Operations)

![Alt text](/assets/CAimages/shmat.png)

 - **shmat** : 얼만큼 shift할 것인지 
 - **왼쪽 쉬프트(Shift Left Logical, sll)** : **sll $t0, $t1, 2 -> $t0 = $t1 * 2^2**
 - **오른쪽 쉬프트(Shift Right Logical, srl)** :  **srl $t0, $t1, 2 -> $t0 = $t1 / 2^2**(부호 없는 값)

### AND 연산
 - 특정 비트를 선택(마스킹)할 때 사용
 - **and $t0, $t1, $t2 → $t0 = $t1 & $t2**

```
$t1 = 0000 0000 0000 0000 0000 1101 1100 0000
$t2 = 0000 0000 0000 0000 0011 1100 0000 0000
결과 ($t0) = 0000 0000 0000 0000 0000 1100 0000 0000
```

### OR 연산
 - 특정 비트를 1로 설정할 때 사용
 - **or $t0, $t1, $t2 → $t0 = $t1 | $t2**

```
$t1 = 0000 0000 0000 0000 0000 1101 1100 0000
$t2 = 0000 0000 0000 0000 0011 1100 0000 0000
결과 ($t0) = 0000 0000 0000 0000 0011 1101 1100 0000
```

### NOT 연산
 - 비트를 반전(Invert)하는 연산
 - **nor $t0, $t1, $zero → $t0 = ~($t1 | 0) = ~ $t1**

```
$t1 = 0000 0000 0000 0000 0011 1100 0000 0000
결과 ($t0) = 1111 1111 1111 1111 1100 0011 1111 1111
```