# 1.
## c언어
```

#include <stdio.h>
#include <stdlib.h>

#define MAX_STACK 100

// 각 재귀 호출의 스택 프레임
typedef struct {
    int n;          // 함수 매개변수
    int left;       // domino(n-1)의 결과
    int right;      // domino(n-2)의 결과
    int result;     // 최종 반환값
} StackFrame;

// 전역 정수형 포인터 배열
int *stack[MAX_STACK];

// 스택의 현재 최상단 인덱스
int top = -1;

// 현재 스택 전체 출력
void printStack(void) {
    printf("\n========== STACK ==========\n");

    if (top == -1) {
        printf("Stack is empty.\n");
    } else {
        for (int i = top; i >= 0; i--) {
            StackFrame *frame = (StackFrame *)stack[i];

            printf("Stack[%d] : n=%d, left=%d, right=%d, result=%d",
                   i, frame->n, frame->left,
                   frame->right, frame->result);

            if (i == top) {
                printf("  <-- TOP");
            }
            printf("\n");
        }
    }

    printf("===========================\n");
}

// 새로운 스택 프레임 생성 및 PUSH
StackFrame *push(int n) {
    if (top >= MAX_STACK - 1) {
        printf("Stack Overflow!\n");
        exit(EXIT_FAILURE);
    }

    StackFrame *frame = malloc(sizeof(StackFrame));

    if (frame == NULL) {
        printf("Memory allocation failed!\n");
        exit(EXIT_FAILURE);
    }

    frame->n = n;
    frame->left = -1;
    frame->right = -1;
    frame->result = -1;

    stack[++top] = (int *)frame;

    printf("\n[PUSH] domino(%d) called\n", n);
    printStack();

    return frame;
}

// 스택 프레임 제거 및 메모리 해제
void pop(void) {
    if (top < 0) {
        printf("Stack Underflow!\n");
        exit(EXIT_FAILURE);
    }

    StackFrame *frame = (StackFrame *)stack[top];

    printf("\n[POP] domino(%d) finished, result=%d\n",
           frame->n, frame->result);

    free(frame);
    stack[top] = NULL;
    top--;

    printStack();
}

// 도미노 경우의 수를 계산하는 재귀함수
int domino(int n) {
    StackFrame *frame = push(n);

    int result;

    // 재귀 종료 조건
    if (n == 1 || n == 2) {
        result = n;
    } else {
        // 첫 번째 재귀 호출
        int left = domino(n - 1);
        frame->left = left;

        // 두 번째 재귀 호출
        int right = domino(n - 2);
        frame->right = right;

        // 두 결과를 더해 최종값 계산
        result = left + right;
    }

    frame->result = result;

    printf("\n[RETURN] domino(%d) = %d\n", n, result);

    // 반환 직전의 전체 스택 출력
    printStack();

    // 현재 프레임 제거
    pop();

    return result;
}

int main(void) {
    int n = 6;

    printf("Domino Recursion Simulation\n");
    printf("n = %d\n", n);

    int answer = domino(n);

    printf("\n===========================\n");
    printf("Final Result : %d\n", answer);
    printf("Final Stack Top : %d\n", top);
    printf("===========================\n");

    return 0;
}
```
## 실행결과 및 스택 변화
```
Domino Recursion Simulation
n = 6

[PUSH] domino(6) called

========== STACK ==========
Stack[0] : n=6, left=-1, right=-1, result=-1  <-- TOP
===========================

[PUSH] domino(5) called

========== STACK ==========
Stack[1] : n=5, left=-1, right=-1, result=-1  <-- TOP
Stack[0] : n=6, left=-1, right=-1, result=-1
===========================

[PUSH] domino(4) called

========== STACK ==========
Stack[2] : n=4, left=-1, right=-1, result=-1  <-- TOP
Stack[1] : n=5, left=-1, right=-1, result=-1
Stack[0] : n=6, left=-1, right=-1, result=-1
===========================

[PUSH] domino(3) called

========== STACK ==========
Stack[3] : n=3, left=-1, right=-1, result=-1  <-- TOP
Stack[2] : n=4, left=-1, right=-1, result=-1
Stack[1] : n=5, left=-1, right=-1, result=-1
Stack[0] : n=6, left=-1, right=-1, result=-1
===========================

[PUSH] domino(2) called

========== STACK ==========
Stack[4] : n=2, left=-1, right=-1, result=-1  <-- TOP
Stack[3] : n=3, left=-1, right=-1, result=-1
Stack[2] : n=4, left=-1, right=-1, result=-1
Stack[1] : n=5, left=-1, right=-1, result=-1
Stack[0] : n=6, left=-1, right=-1, result=-1
===========================

[RETURN] domino(2) = 2

========== STACK ==========
Stack[4] : n=2, left=-1, right=-1, result=2  <-- TOP
Stack[3] : n=3, left=-1, right=-1, result=-1
Stack[2] : n=4, left=-1, right=-1, result=-1
Stack[1] : n=5, left=-1, right=-1, result=-1
Stack[0] : n=6, left=-1, right=-1, result=-1
===========================

[POP] domino(2) finished, result=2

========== STACK ==========
Stack[3] : n=3, left=-1, right=-1, result=-1  <-- TOP
Stack[2] : n=4, left=-1, right=-1, result=-1
Stack[1] : n=5, left=-1, right=-1, result=-1
Stack[0] : n=6, left=-1, right=-1, result=-1
===========================

[PUSH] domino(1) called

========== STACK ==========
Stack[4] : n=1, left=-1, right=-1, result=-1  <-- TOP
Stack[3] : n=3, left=2, right=-1, result=-1
Stack[2] : n=4, left=-1, right=-1, result=-1
Stack[1] : n=5, left=-1, right=-1, result=-1
Stack[0] : n=6, left=-1, right=-1, result=-1
===========================

[RETURN] domino(1) = 1

========== STACK ==========
Stack[4] : n=1, left=-1, right=-1, result=1  <-- TOP
Stack[3] : n=3, left=2, right=-1, result=-1
Stack[2] : n=4, left=-1, right=-1, result=-1
Stack[1] : n=5, left=-1, right=-1, result=-1
Stack[0] : n=6, left=-1, right=-1, result=-1
===========================

[POP] domino(1) finished, result=1

========== STACK ==========
Stack[3] : n=3, left=2, right=-1, result=-1  <-- TOP
Stack[2] : n=4, left=-1, right=-1, result=-1
Stack[1] : n=5, left=-1, right=-1, result=-1
Stack[0] : n=6, left=-1, right=-1, result=-1
===========================

[RETURN] domino(3) = 3

========== STACK ==========
Stack[3] : n=3, left=2, right=1, result=3  <-- TOP
Stack[2] : n=4, left=-1, right=-1, result=-1
Stack[1] : n=5, left=-1, right=-1, result=-1
Stack[0] : n=6, left=-1, right=-1, result=-1
===========================

[POP] domino(3) finished, result=3

========== STACK ==========
Stack[2] : n=4, left=-1, right=-1, result=-1  <-- TOP
Stack[1] : n=5, left=-1, right=-1, result=-1
Stack[0] : n=6, left=-1, right=-1, result=-1
===========================

[PUSH] domino(2) called

========== STACK ==========
Stack[3] : n=2, left=-1, right=-1, result=-1  <-- TOP
Stack[2] : n=4, left=3, right=-1, result=-1
Stack[1] : n=5, left=-1, right=-1, result=-1
Stack[0] : n=6, left=-1, right=-1, result=-1
===========================

[RETURN] domino(2) = 2

========== STACK ==========
Stack[3] : n=2, left=-1, right=-1, result=2  <-- TOP
Stack[2] : n=4, left=3, right=-1, result=-1
Stack[1] : n=5, left=-1, right=-1, result=-1
Stack[0] : n=6, left=-1, right=-1, result=-1
===========================

[POP] domino(2) finished, result=2

========== STACK ==========
Stack[2] : n=4, left=3, right=-1, result=-1  <-- TOP
Stack[1] : n=5, left=-1, right=-1, result=-1
Stack[0] : n=6, left=-1, right=-1, result=-1
===========================

[RETURN] domino(4) = 5

========== STACK ==========
Stack[2] : n=4, left=3, right=2, result=5  <-- TOP
Stack[1] : n=5, left=-1, right=-1, result=-1
Stack[0] : n=6, left=-1, right=-1, result=-1
===========================

[POP] domino(4) finished, result=5

========== STACK ==========
Stack[1] : n=5, left=-1, right=-1, result=-1  <-- TOP
Stack[0] : n=6, left=-1, right=-1, result=-1
===========================

[PUSH] domino(3) called

========== STACK ==========
Stack[2] : n=3, left=-1, right=-1, result=-1  <-- TOP
Stack[1] : n=5, left=5, right=-1, result=-1
Stack[0] : n=6, left=-1, right=-1, result=-1
===========================

[PUSH] domino(2) called

========== STACK ==========
Stack[3] : n=2, left=-1, right=-1, result=-1  <-- TOP
Stack[2] : n=3, left=-1, right=-1, result=-1
Stack[1] : n=5, left=5, right=-1, result=-1
Stack[0] : n=6, left=-1, right=-1, result=-1
===========================

[RETURN] domino(2) = 2

========== STACK ==========
Stack[3] : n=2, left=-1, right=-1, result=2  <-- TOP
Stack[2] : n=3, left=-1, right=-1, result=-1
Stack[1] : n=5, left=5, right=-1, result=-1
Stack[0] : n=6, left=-1, right=-1, result=-1
===========================

[POP] domino(2) finished, result=2

========== STACK ==========
Stack[2] : n=3, left=-1, right=-1, result=-1  <-- TOP
Stack[1] : n=5, left=5, right=-1, result=-1
Stack[0] : n=6, left=-1, right=-1, result=-1
===========================

[PUSH] domino(1) called

========== STACK ==========
Stack[3] : n=1, left=-1, right=-1, result=-1  <-- TOP
Stack[2] : n=3, left=2, right=-1, result=-1
Stack[1] : n=5, left=5, right=-1, result=-1
Stack[0] : n=6, left=-1, right=-1, result=-1
===========================

[RETURN] domino(1) = 1

========== STACK ==========
Stack[3] : n=1, left=-1, right=-1, result=1  <-- TOP
Stack[2] : n=3, left=2, right=-1, result=-1
Stack[1] : n=5, left=5, right=-1, result=-1
Stack[0] : n=6, left=-1, right=-1, result=-1
===========================

[POP] domino(1) finished, result=1

========== STACK ==========
Stack[2] : n=3, left=2, right=-1, result=-1  <-- TOP
Stack[1] : n=5, left=5, right=-1, result=-1
Stack[0] : n=6, left=-1, right=-1, result=-1
===========================

[RETURN] domino(3) = 3

========== STACK ==========
Stack[2] : n=3, left=2, right=1, result=3  <-- TOP
Stack[1] : n=5, left=5, right=-1, result=-1
Stack[0] : n=6, left=-1, right=-1, result=-1
===========================

[POP] domino(3) finished, result=3

========== STACK ==========
Stack[1] : n=5, left=5, right=-1, result=-1  <-- TOP
Stack[0] : n=6, left=-1, right=-1, result=-1
===========================

[RETURN] domino(5) = 8

========== STACK ==========
Stack[1] : n=5, left=5, right=3, result=8  <-- TOP
Stack[0] : n=6, left=-1, right=-1, result=-1
===========================

[POP] domino(5) finished, result=8

========== STACK ==========
Stack[0] : n=6, left=-1, right=-1, result=-1  <-- TOP
===========================

[PUSH] domino(4) called

========== STACK ==========
Stack[1] : n=4, left=-1, right=-1, result=-1  <-- TOP
Stack[0] : n=6, left=8, right=-1, result=-1
===========================

[PUSH] domino(3) called

========== STACK ==========
Stack[2] : n=3, left=-1, right=-1, result=-1  <-- TOP
Stack[1] : n=4, left=-1, right=-1, result=-1
Stack[0] : n=6, left=8, right=-1, result=-1
===========================

[PUSH] domino(2) called

========== STACK ==========
Stack[3] : n=2, left=-1, right=-1, result=-1  <-- TOP
Stack[2] : n=3, left=-1, right=-1, result=-1
Stack[1] : n=4, left=-1, right=-1, result=-1
Stack[0] : n=6, left=8, right=-1, result=-1
===========================

[RETURN] domino(2) = 2

========== STACK ==========
Stack[3] : n=2, left=-1, right=-1, result=2  <-- TOP
Stack[2] : n=3, left=-1, right=-1, result=-1
Stack[1] : n=4, left=-1, right=-1, result=-1
Stack[0] : n=6, left=8, right=-1, result=-1
===========================

[POP] domino(2) finished, result=2

========== STACK ==========
Stack[2] : n=3, left=-1, right=-1, result=-1  <-- TOP
Stack[1] : n=4, left=-1, right=-1, result=-1
Stack[0] : n=6, left=8, right=-1, result=-1
===========================

[PUSH] domino(1) called

========== STACK ==========
Stack[3] : n=1, left=-1, right=-1, result=-1  <-- TOP
Stack[2] : n=3, left=2, right=-1, result=-1
Stack[1] : n=4, left=-1, right=-1, result=-1
Stack[0] : n=6, left=8, right=-1, result=-1
===========================

[RETURN] domino(1) = 1

========== STACK ==========
Stack[3] : n=1, left=-1, right=-1, result=1  <-- TOP
Stack[2] : n=3, left=2, right=-1, result=-1
Stack[1] : n=4, left=-1, right=-1, result=-1
Stack[0] : n=6, left=8, right=-1, result=-1
===========================

[POP] domino(1) finished, result=1

========== STACK ==========
Stack[2] : n=3, left=2, right=-1, result=-1  <-- TOP
Stack[1] : n=4, left=-1, right=-1, result=-1
Stack[0] : n=6, left=8, right=-1, result=-1
===========================

[RETURN] domino(3) = 3

========== STACK ==========
Stack[2] : n=3, left=2, right=1, result=3  <-- TOP
Stack[1] : n=4, left=-1, right=-1, result=-1
Stack[0] : n=6, left=8, right=-1, result=-1
===========================

[POP] domino(3) finished, result=3

========== STACK ==========
Stack[1] : n=4, left=-1, right=-1, result=-1  <-- TOP
Stack[0] : n=6, left=8, right=-1, result=-1
===========================

[PUSH] domino(2) called

========== STACK ==========
Stack[2] : n=2, left=-1, right=-1, result=-1  <-- TOP
Stack[1] : n=4, left=3, right=-1, result=-1
Stack[0] : n=6, left=8, right=-1, result=-1
===========================

[RETURN] domino(2) = 2

========== STACK ==========
Stack[2] : n=2, left=-1, right=-1, result=2  <-- TOP
Stack[1] : n=4, left=3, right=-1, result=-1
Stack[0] : n=6, left=8, right=-1, result=-1
===========================

[POP] domino(2) finished, result=2

========== STACK ==========
Stack[1] : n=4, left=3, right=-1, result=-1  <-- TOP
Stack[0] : n=6, left=8, right=-1, result=-1
===========================

[RETURN] domino(4) = 5

========== STACK ==========
Stack[1] : n=4, left=3, right=2, result=5  <-- TOP
Stack[0] : n=6, left=8, right=-1, result=-1
===========================

[POP] domino(4) finished, result=5

========== STACK ==========
Stack[0] : n=6, left=8, right=-1, result=-1  <-- TOP
===========================

[RETURN] domino(6) = 13

========== STACK ==========
Stack[0] : n=6, left=8, right=5, result=13  <-- TOP
===========================

[POP] domino(6) finished, result=13

========== STACK ==========
Stack is empty.
===========================

===========================
Final Result : 13
Final Stack Top : -1
===========================
```
# 2.
## RISC-V
```
main:
    addi sp, zero, 1024    # 스택 시작 주소를 1024로 설정
    addi a0, zero, 6       # 매개변수 n = 6 설정
    jal ra, domino        # domino(6) 호출
    addi s0, a0, 0        # 최종 반환값을 s0에 저장
    jal zero, finish      # 결과 확인을 위한 대기 지점으로 이동

domino:
    addi sp, sp, -32      # 32바이트 스택 프레임 생성
    sw ra, 16(sp)         # 호출한 곳으로 돌아갈 복귀 주소 저장
    sw a0, 0(sp)          # 매개변수 n 저장

    addi t0, zero, -1     # 아직 계산되지 않은 값을 -1로 설정
    sw t0, 4(sp)          # left 초기화
    sw t0, 8(sp)          # right 초기화
    sw t0, 12(sp)         # result 초기화

    addi t0, zero, 2      # 종료 조건 비교를 위해 2 설정
    bge t0, a0, base_case # 2 >= n이면 기본 경우로 이동

    lw a0, 0(sp)          # 현재 프레임에서 원래 n 읽기
    addi a0, a0, -1       # 첫 번째 호출의 매개변수 n - 1 계산
    jal ra, domino        # domino(n - 1) 호출
    sw a0, 4(sp)          # 첫 번째 반환값을 left에 저장

    lw a0, 0(sp)          # 현재 프레임에서 원래 n 다시 읽기
    addi a0, a0, -2       # 두 번째 호출의 매개변수 n - 2 계산
    jal ra, domino        # domino(n - 2) 호출
    sw a0, 8(sp)          # 두 번째 반환값을 right에 저장

    lw t0, 4(sp)          # left 값을 t0에 읽기
    lw t1, 8(sp)          # right 값을 t1에 읽기
    add a0, t0, t1        # left + right 계산
    sw a0, 12(sp)         # 계산값을 result에 저장
    jal zero, domino_end  # 반환 처리로 이동

base_case:
    sw a0, 12(sp)         # n=1 또는 n=2이면 n을 result에 저장

domino_end:
    lw a0, 12(sp)         # result를 반환값 레지스터 a0에 읽기
    lw ra, 16(sp)         # 현재 호출의 복귀 주소 복원
    addi sp, sp, 32       # 현재 스택 프레임 제거
    jalr zero, ra, 0      # 복귀 주소로 이동 (인터프리터 문법)

finish:
    jal zero, finish      # 결과 확인을 위해 현재 위치에서 반복

```
## 실행결과
![RISC-V 실행 결과](./img/img.png)


## 레지스터 값
```
Register Expected Results
## expect[0] = 0x00000000
## expect[1] = 0x0000000c
## expect[2] = 0x00000400
## expect[3] = 0x00000000
## expect[4] = 0x00000000
## expect[5] = 0x00000008
## expect[6] = 0x00000005
## expect[7] = 0x00000000
## expect[8] = 0x0000000d
## expect[9] = 0x00000000
## expect[10] = 0x0000000d
## expect[11] = 0x00000000
## expect[12] = 0x00000000
## expect[13] = 0x00000000
## expect[14] = 0x00000000
## expect[15] = 0x00000000
## expect[16] = 0x00000000
## expect[17] = 0x00000000
## expect[18] = 0x00000000
## expect[19] = 0x00000000
## expect[20] = 0x00000000
## expect[21] = 0x00000000
## expect[22] = 0x00000000
## expect[23] = 0x00000000
## expect[24] = 0x00000000
## expect[25] = 0x00000000
## expect[26] = 0x00000000
## expect[27] = 0x00000000
## expect[28] = 0x00000000
## expect[29] = 0x00000000
## expect[30] = 0x00000000
## expect[31] = 0x00000000
```