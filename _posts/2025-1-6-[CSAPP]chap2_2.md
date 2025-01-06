---
layout: single
title: "[CS:APP] Chap2 A Representing and Manipulating Information(2)"
categories: ComputerSystems
tag: [CS, CSAPP]
toc: true
---

## 2.2 Integer Representations

 이 섹션은 정수를 표현하는 두 가지 비트 기반 방식 **(Unsigned, Signed)**을 소개하고, 비트 확장 및 축소가 값에 미치는 영향을 다룬다. 아래는 용어를 정리한 표이다.

 ![Alt text](/assets/images/chap2_2.png)

### 2.2.1 Integral Data Types

 C언어는 다양한 정수형 데이터 타입을 지원하며, 각 데이터 타입은 특정한 크기와 범위를 가진다. 이 데이터 타입들은 char, short, int, long과 같은 키워드를 통해 크기를 지정할 수 있고, 부호 있는 값(signed)과 부호 없는 값(unsigned)을 표현할 수 있다. C 표준은 각 데이터 타입이 표현할 수 있는 최소한의 범위를 정의하며, 이는 모든 구현에서 반드시 충족되어야 한다. 표준에서 요구하는 범위는 양수와 음수의 범위가 대칭적이지만, 실제 구현에서는 음수 범위가 양수 범위보다 하나 더 크다(2의 보수 표현 방식에서 발생)

 ![Alt text](/assets/images/GuarantedRanges.png)

 위 표는 C 표준에서 요구하는 범위이다. 즉, C 표준에서 명시적으로 규정한 데이터 타입의 최소 범위이다.

 ![Alt text](/assets/images/typicalranges.png)

 C 언어의 데이터 타입 크기와 값의 범위는 프로그램이 어느 환경에서(32비트 or 64비트) 실행되느냐에 따라 달라진다. 위에 표는 32비트와 64비트 환경에서의 일반적인 범위를 나타낸다. (**일반적인 범위(typical range)** : 특정 데이터 타입이 실제로 구현된 범위) 
 
 위 표에서 보여주는 일반적인 범위는 **C표준에서 요구하는 최소 범위보다 크거나 같으며**, **음수와 양수의 범위가 비대칭적**인 특징을 갖는다. 
 
 **데이터 타입 크기는 프로그램의 환경에 따라 달라질 수도 있다.** 예를 들어, long 타입의 경우 32비트 환경에서는 4바이트로 제한되어 값의 범위가 좁지만, 64비트 환경에서는 8바이트로 구현되어 매우 넓은 값을 표현할 수 있다. 

 고정 크기 데이터 타입(int32_t, uint32_t...)은 정확히 정해진 크기와 값을 보장한다. 음수와 양수의 비대칭성이 유지되며, 데이터 타입의 크기에 따라 숫자의 표현 범위가 달라진다. 이러한 범위는 바이트 할당량에 의해 결정된다. (C 표준에서 요구하는 범위를 나타낸 표와 실제 구현된 범위를 나타낸 표를 비교해보면 고정 크기 데이터 타입은 최소,최대 크기가 비대칭성을 그대로 유지하는 것을 확인할 수 있다.)

 실제 구현된 값의 범위가 대칭적이지 않은 이유는 2의 보수 표현 방식 때문이다. 음수를 표현할 때 부호 비트를 포함해 양수보다 하나 더 많은 값을 표현할 수 있으므로, 음수 범위가 더 넓어진다.

### 2.2.2 Unsigned Encodings

 C 언어에서 정수를 표현하기 위해 비트를 사용하는 방식 중 하나는 **부호 없는 정수(unsigned integer)**를 표현하는 것이다. 이를 위해 비트 벡터(bit vector)라는 개념을 사용하며, 비트 벡터는 X=[x<sub>w-1</sub>, x<sub>w-2</sub>,...,x<sub>0</sub>]와 같은 형식으로 표현된다. 여기서 w는 비트의 길이를 나타낸다. 각 비트는 0과 1의 값을 가지며 x<sub>i</sub> = 1일 경우 해당 위치의 가중치가 2<sup>i</sup>가 숫자 값에 포함된다. 이렇게 비트 벡터를 부호 없는 정수로 변환하는 과정을 함수 **B2U<sub>w</sub>**로 정의할 수 있다.

 ![Alt text](/assets/images/unsignednumex.png)

 함수 **B2U<sub>w</sub>**는 비트 벡터를 부호 없는 정수로 변환하며, 다음과 같은 수식으로 표현된다.

 ![Alt text](/assets/images/eqation2_1.png)

 이 함수는 길이가 w인 비트 벡터를 0에서 2<sup>w</sup> - 1 사이의 값으로 매핑한다. 예를 들어 4비트의 경우

 ![Alt text](/assets/images/2_1ex.png)
 
 부호 없는 정수로 표현할 수 있는 값의 범위는 w-비트에 따라 달라진다. 최소값은 비트 벡터가 모두 0인 경우이며, 최대값은 모든 비트가 1인 경우로, 이를 2<sup>w</sup> - 1로 계산할 수 있다. 

 **B2U<sub>w</sub>** 함수는 **고유성(Uniqueness)**을 보장한다. 즉, 각 비트 벡터는 고유한 숫자 값에 매핑되며, 반대로 각 숫자 값도 고유한 비트 벡터에 매핑된다. 이러한 매핑은 **전단사 함수(bijection)**로 정의되며,  **B2U<sub>w</sub>**의 역함수  **U2B<sub>w</sub>**를 통해 숫자에서 비트 벡터로 변환이 가능하다. 

 결론적으로  **B2U<sub>w</sub>** 함수는 비트 벡터를 부호 없는 정수로 변환하는 데 사용되며, 최소값 0에서 최대값 2<sup>w</sup> - 1 까지의 범위를 가진 숫자와 비트 벡터 간의 고유한 매핑을 제공한다.

 ### 2.2.3 Two's-Complement Encodings

 컴퓨터에서 정수를 표현하는 가장 일반적인 방법 중 하나는 **2의 보수(Two's Complement)** 방식이다. 이 방식은 가장 높은 비트(MSB, most significant bit)를 음수 가중체를 가지는 부호 비트(sign bit)로 해석하여 음수와 양수를 모두 표현할 수 있다. 2의 보수 표현을 수학적으로 정의한 함수 **B2T<sub>w</sub>**는 다음과 같이 정의된다.

 ![Alt text](/assets/images/equation2_3.png)

 여기서 x<sub>w-1</sub>은 부호 비트를 나타내며, 1일 경우 음수를, 0일 경우 양수나 0을 나타낸다. 이 정의는 비트 벡터를 음수와 양수로 매핑한다.

 w-비트 숫자의 범위는 **TMin<sub>w</sub> = -2<sub>w-1</sub>**부터 **TMax<sub>w</sub> = -2<sub>w-1</sub> - 1**이다.

 ![Alt text](/assets/images/2_3ex.png)

 이 방식에서 비트 벡터는 유일한 숫자 값에 매핑되며, 역함수 **T2B<sub>w</sub>**를 통해 숫자를 비트 벡터로 변환할 수 있다. 이는 **전단사 함수(bijection)**로, 각 수자는 고유한 비트 벡터로 표현된다.

 ![Alt text](/assets/images/Figure2_13.png)

#### 2의 보수 범위의 특징

![Alt text](/assets/images/Figure2_14.png)

 1. **비대칭적 범위**
    - 0이 양수로 포함되기 때문에, 음수의 범위는 양수보다 하나 더 많다.(|**TMin**| = |**TMax**| + 1).
 2. **최대값과 최소값**
    - 최대값 : **TMax<sub>w</sub> = -2<sub>w-1</sub> - 1**
    - 최소값 : **TMin<sub>w</sub> = -2<sub>w-1</sub>**
 3. **비트 패턴 비교**
    - 2의 보수와 부호 없는 값의 비트 패턴은 동일하지만, 해석 방식이 다르다.
    - 예를 들어, 2의 보수에서 -1은 부호 없는 값의 최대값(**UMax**)과 동일한 비트 패턴을 가진다.

C 표준은 2의 보수 방식을 필수로 요구하지 않지만, 대부분의 시스템은 2의 보수를 사용한다. Java는 2의 보수를 필수로 요구하며, 데이터 타입의 범위와 표현 방식을 명확히 정의한다.

#### 코드 예제

 ```c
short x = 12345;
short mx = -x;

show_bytes((byte_pointer) &x, sizeof(short));
show_bytes((byte_pointer) &mx, sizeof(short));
```

![Alt text](/assets/images/Figure2_15.png)

 - x = 12345는 16진수로 **0x3039**, 이진수로 [0011000000111001]
 - mx = -12345는 16진수로 **0xCFC7**, 이진수로 [1100111111000111]

### 2.2.4 Conversions between Signed and Unsigned

 C언어에서는 부호 있는 정수(Signed)와 부호 없는 정수(Unsigned) 간의 **캐스팅(Casting)**이 가능하다. 캐스팅은 **비트 패턴은 유지하되, 숫자 해석 방식을 변경**한다. 이 변환은 **비트 레벨 관점**에서 이루어져, 숫자 값이 변할 수 있지만, **비트 패턴은 동일**하다.

#### 부호 있는 값에서 부호 없는 값으로 변환 (T2U)

 부호 있는 값에서 부호 없는 값으로 변환 시, 음수는 부호 없는 양수로 변환된다.

 ```c
 short int v = -12345;
 unsigned short uv = (unsigned short)v;
 printf("v = %d, uv = %u\n", v, uv);
 ```

 출력 결과 : **v = -12345, uv = 53191**

 **T2U<sub>16</sub>(-12345) = -12345 + 2<sup>16</sup> = 53191**

 **T2U 함수의 수학적 정의**

 ![Alt text](/assets/images/equation2_5.png)

 **T2U<sub>16</sub>(-12345) = -12345 + 2<sup>16</sup> = 53191**
 **T2U<sub>32</sub>(-1) = -1 + 2<sup>32</sup> = 4294967295**

 ![Alt text](/assets/images/Figure2_17.png)

#### 부호 없는 값에서 부호 있는 값으로 변환 (U2T)

 부호 없는 값에서 부호 있는 값으로 변환 시, **TMax**보다 큰 값은 음수로 변환된다.

 ```c
 unsigned u = 4294967295; /* UMax */
 int tu = (int)u;
 printf("u = %u, tu = %d\n", u, tu);
 ```

 출력 결과 : **u = 4294967295, tu = -1**

 **U2T<sub>32</sub>(4294967295) = 4294967295 + 2<sup>32</sup> = -1**

 ![Alt text](/assets/images/equation2_7.png)

 ![Alt text](/assets/images/Figure2_18.png)


