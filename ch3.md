# 데이터형

## 기본 데이터형의 종류 및 데이터형의 범위

### 데이터형의 구분

- 기본 데이터형(primitive data type): 프로그램 수행에 필요한 메모리를 할당 받기 위해 기본적으로 제공되는 데이터형
  - 산술형
    - 정수형(char c=3; 가능함!!)
      - 문자형: `char`, `unsigned char`
      - 수치형: `short`, `unsigned short`, `int`, `unsigned int`, `long`, `unsigend long`, `long long`, `unsigned long long`
    - 실수형: `float`, `double`
  - 무치형: void
- 복합 데이터형(compound data type): 기본 데이터형을 응용해서 다양한 형태의 복합 데이터형을 만들어 사용한다.
  - 배열(array), 포인터(pointer), 구조체(struct), 나열형(enum), 공용체(union)



|  구분  |              기본 데이터형              | Byte |
| :----: | :-------------------------------------: | :--: |
| 문자형 |           char, unsigned char           |  1   |
| 수치형 |            int, unsigned int            |  4   |
|        |     short(int), unsigned short(int)     |  2   |
|        |      long(int), unsigned long(int)      |  4   |
|        | long long(int), unsigned long long(int) |  8   |
| 실수형 |                  float                  |  4   |
|        |                 double                  |  8   |

`sizeof` 연산자

`<limits.h> ` 를 이용해 데이터형의 범위(최대, 최소값)을 알 수 있다.

- **signed** vs **unsigned** 

  Sign Bit의 유무

### 데이터 표현에 사용되는 진법

1. 10진법
2. 8진법
3. 16진법

- suffix: 숫자에 붙이는 접미사
  - 정수형 상수의 기본형은 signed int
  - 정수형 suffix: u, l(L), ul(UL)
  - 실수형 상수의 기본형은 double
  - 실수형 suffix: f(F), l(L)

```c
#include <stdio.h>
int main(){
    int x = 10, y= 010, z=0x10;
    printf("x의 값: %d %#o %#x\n", x, x, x); //10 010 0xa
    printf("y의 값: %d %o %x\n", y, y, y); // 8 10 8
    printf("z의 값: %d %o %x\n", z, z, z); // 16 20 10
    return 0;

}
```

- `#` : flag 진법 접두사를 화면에 출력해준다.

- `LV` =`RV`

  Rv를 Lv에 bit copy

  두 개의 차원과 데이터 타입이 같아야한다.

  ```c++
  int a;
  a =7;
  // (signed int) = (signed int)
  
  long la;
  la = 7;
  // (long) = (int) : 타입 불일치이지만 autocasting -> long = long 형태로 변환
  la = 7L; 
  // suffix를 붙여주어 미리 바꾸어주자.
  
  floag fa; 
  fa = 3.5;
  // (float) = (double) -> (float) = (float)
  
  long double x;
  x = 3.5L 
  // (long double) = (long double)
   
  ```

  ##  

## 정수형의 이해

1. 정수형의 데이터 크기는 `<limits.h>` 헤더파일에서 확인 가능ㄴ

2. 종류

   `(unsigned) char`, `(unsigned) int`, `(unsigned) short` , `(unsigned) long`, `(unsigned) long long` 

3. 저장 방식

   - 양수: 부호화 절대치 표현

   - 음수: 2의 보수 표현( 보수기 in CPU)

     - 2의 보수(2's complement) 만들기

       1. 바꾸고자 하는 대상 숫자를 2진수로 변환
       2. 위 결과를 다시 1의 보수로 변환
       3. 2의 결과에 1을 더한다.

     - 2의 보수 만드는 간단한 방법

       위에서 제시한 1번의 방법으로 바꾼 후 bit열의 우측에서 좌측으로 처음 1이 나올 때까지는 bit열을 그대로 내려쓰고 나머지는 1의 보수를 취한다.

4. 예제

   1. 최대 값을 넘어가는 숫자의 표현

      ```c++
      #include <stdio.h>
      int main(){
          short x = 32767;
          short y = x+1;
          printf("x = %hd\t y = %hd\n", x, y);
          return 0;
      }
      ```

      - %hd: short 형 출력 시 사용되는 변경자
      - 자료형의 최대값 + 1 (양수) -> 자료형의 최소값(음수)
      - 자료형의 최소값 -1(음수) -> 자료형의 최대값(양수)

   2. unsigned형 기억공간에 음수값을 저장하면 어떻게 될까? 

      ```c
      #include <stdio.h>
      int main(){
          short x = -32767;
          unsigned short y = x;
          printf("x = %hd\t y = %hu\n", x, y);
      	printf("x = %hu\t y = %hd\n", x, y);
          return 0;
      }
      ```

      - 저장되어 있는 값과 상관없이 출력되는 값은 형식변환 문자열의 해석에 따른다.
      - 그럼 왜 굳이 signed, unsigned를 구분하여 변수를 선언할까?

   3. signed, unsigned형으로 구분하는 것의 의미

      ```c++
      #include <stdio.h>
      int main(){
          short x;
          unsigned short y;
          int res1, res2;
          x = y = -1;
          res1 = x * 3;
          res2 = y * 3;
      
      
          printf("res1 = %d\n", res1); // -3
          printf("res2 = %d\n", res2); // 196605
      
          return 0;
      }
      ```

      - x: signed, y: unsigned 의 데이터로 인식되어 계산된다.
      - 이항 연산시 두 피연산자의 type이 다르면 맞춰놓고 연산하려는 현상이 나타난다.

5. 형변환의 원칙

   1. 작은 -> 큰 (값의 손실을 예방하기 위해)
      - 메모리 확장 규칙
        - signed : sign bit 값으로 부호확장 
        - unsigned: 0 확장
   2. 예외: 대입연산

## 실수형의 이해

1. 컴퓨터내의 실수저장방식에는 오차가 발생하므로 "real"이 아니라 "float"라고 한다.

2. 실수형의 종류

   `float`, `double`

3. 저장방식

   - 단정도 부동소수(32bit float)
   - 배정도 부동소수(64bit float)

4. 표기방식

   - 소수형 표기
   - 지수형 표기

5. 실수형의 오차

   - 유효정밀도에 의한 오차
   - 2진수 변환 시 순환소수로 변환되어 발생하는 오차



## 문자형의 이해

1. char형은 1Byte 크기의 정수형으로 정수형 저장 방식을 그대로 따른다.

2. 문자 상수는 작은 따옴표로 묶으며 값은 그 문자에 대한 아스키코드 값으로 저장된다.

   프로그램내에서 문자에 대한 처리는 각 상수의 아스키코드 값으로 처리되므로 문자에 대한 **연산** 이 가능하다.

   

## 문자열형의 이해

1. 문자열은 문자형 자료의 집합으로 char형 1차원 배열로 표현된다.

2. 문자열 상수는 **가변길이 상수** 이므로 반드시 데이터의 끝 위치를 나타내기 위해 **NULL('\0')** 문자로 종료되며 이중인용부호 **" "** 로 묶는다.

   문자열 상수 "hi"는 'h' ,'i', '\0'의 집합이다.

   

## 형변환의 이해

### 형변환의 종류

- 자동형변환

  0차원의 기본 데이터형에서만 발생한다.

  - 두 피연산자의 타입이 다른 경우 타입을 맞추기 위해
  - 연산 시에 존재하지 않는 타입을 연산에 적합한 타입으로 바꾸기 위해

- 강제형변환

  프로그래머가 필요에 의해 cast(형변환)연산자를 이용하여 실시하는 형변환

  

### 자동형변환의 원칙

- 작은형이 큰형으로 변환된다.(최대값의 크기로 형의 크기를 구분)

- 대입연산자 Rvalue는 Lvalue의 type으로 변환된다.

- 주소는 자동형변환이 되지 않는다.

- 연산지 존재하지 않는 type은 정수 승계 규칙에 따라 **signed int**형으로 변환된다.

  `char`, `unsigned char`, `short`, `unsigned short`

### 메모

- 1byte 크기의 char형이 오면 4byte로 메모리가 확장된다. signed int 형으로 변한다. (in Register not in RAM)
- 실수형 데이터는 레지스터에 올라올 수 없다. 따라서 그것의 주소를 레지스터에 올리고 연산은 RAM에서 일어난다.
- 정수형에 대한 연산은 레지스터에서 한다. 즉, 레지스터는 정수형 데이터만을 다룰 수 있다.