# 기초 제어문(if, while, break)

## 문장의 종류

### Preprocessor 지시문

`#include`, `#define, ` `#pragma` 등 

### 선언문

`char`, `int`, `long` ,`float` ,`double` 등

- 변수 선언문

  int a;

- 함수 선언문

  void sub (int);

### 제어문

1. 택일문

   `if, else if, else`, `switch case`

2. 반복문

   `for`, `while`, `do~while`

3. 분기문

   `break`, `continue`, `goto`, `return`

### 기타

`NULL ` statement



## if 문

> 조건식에 따라 실행문을 실행 하느냐 마느냐를 결정한다.
>
> 택일문, 선택문이라 한다.

```c
if(조건식){
    //처리문장 1
}
else if (조건식){
    //처리문장 2
}
else{
    //처리문장 3
}
```



## while 문

> 조건이 참인 동안 반복시키는 **선조건비교**(조건식이 반복할 문장보다 먼저) 순환문이다.

```c
while(조건식){
    반복할 문장;
}
```



### 후조건 비교 순환문

```c
do{
    반복할 문장;
}
while(조건식);
```



### 처음부터 조건이 거짓일 때

- 후조건 비교 순환문: 한 번은 문장을 수행
- 선조건 비교 순환문: 한 번도 문장을 수행하지 않음
- 상황에 따른 반복



```c
#include <stdio.h>
int main(){
    int k, s=0;
    printf("정수를 입력. 문자를 입력하면 종료됨");
    while(scanf("%d", &k) ==1) { // 함수 call **1순위!**
        pritnf("지금까지 합은 %d\n", s+=k );
        printf("정수 입력, 문자를 입력하면 종료됨: ");
    }
    return 0;
}
```

- 메모 

  - `scanf()` 함수에는 return 값이 돌아오게 되어있다.

  - 예시

    ```c
    int a, b, res;
    res = scanf("%d %d", &a, &b);
    // return 값: 성공적으로 입력 받은 데이터의 개수 
    ```

    - 입력

      - |  a   |  b   | res(리턴 값) |
        | :--: | :--: | :----------: |
        |  10  |  20  |      2       |
        |  10  |  a   |      1       |
        |  @   |  10  |      0       |