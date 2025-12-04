#include <stdio.h>      //ㅐPRINTF, scanf를 사용하기 위해서 헤더 추가
#include <stdlib.h>    // rand, srand를 사용하기 위해서 헤더 추가
#include <time.h>   // time 함수를 사용하기 위해서 헤더 추가

int main(void)      //프로그램을 실행하는 main 함수
{                   //메인함수의 시작
    int ball[3]; // 3개의 난수 저장 배열
    srand(time(NULL)); // 난수 초기화
    // 중복 없는 3자리 난수 생성
    do {
        ball[0] = rand() % 10; // 첫번째 난수 생성(0~9)
        ball[1] = rand() % 10; // 두번째 난수 생성(0~9)
        ball[2] = rand() % 10; // 세번째 난수 생성(0~9)
    } while (ball[0] == ball[1] || ball[0] == ball[2] || ball[1] == ball[2]); // 세 숫자가 서로 중복되면 다시 난수 생성

    //printf("baseball : %d %d %d\n", ball[0], ball[1], ball[2]); // 정답 확인용(테스트시만)

    int input[3]; // 입력할 숫자 3개 저장 배열
    int strike_count = 0 ; // strike 개수 저장 변수
    int ball_count = 0 ; // ball 개수 저장 변수
    int out_count = 0; // out 개수 저장 변수
    int try_count = 0; // 시도한 횟수 기록 변수

    clock_t start, end; //게임 시작/종료 시간을 저장할 clock_t 타입 변수
    start = clock(); // 게임 시작 시간 기록

    while (1) { //// 정답을 맞출 때까지 무한 반복
        printf("\n숫자 3개를 입력하세요 (공백으로 구분): ");// 사용자에게 입력 요청
        scanf("%d %d %d", &input[0], &input[1], &input[2]);  // 사용자 입력을 배열에 저장
 
        strike_count = 0; // 이전 결과 초기화
        ball_count = 0; // 이전 결과 초기화

        // strike, ball 판정
        for (int i = 0; i < 3; i++) { // 0~2까지 반복
            if (input[i] == ball[i]) { //같은 위치,같은 숫자면 strike
                strike_count++; // strike 1 증가
            } else if (input[i] == ball[(i+1)%3] || input[i] == ball[(i+2)%3]) {
                ball_count++; //// 다른 위치의 숫자와 같으면 ball
            }
        }

        out_count = 3 - (strike_count + ball_count);  // strike와 ball이 아닌 나머지는 out
        
        try_count++; // 시도 횟수 1 증가

        if (strike_count == 3) {
            end = clock(); // 게임 종료 시간 기록            
            double elapsed_sec = (double)(end - start) / CLOCKS_PER_SEC; // 걸린 시간 계산
            printf("Home Run! %d번 만에 맞췄습니다!\n", try_count); // 시도 횟수 출력
            printf("%f 초 시간이 걸렸습니다.\n", elapsed_sec); // 걸린 시간 출력
            break; // 게임 종료 (while문 탈출)
        } else { // 정답이 아닌 경우
            printf("%d Strike, %d Ball, %d Out\n", strike_count, ball_count, out_count); // 결과 출력
        }
    }
    return 0;  // 프로그램 종료
}

