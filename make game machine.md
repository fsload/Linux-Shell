machine.c
```c

#include <stdio.h>
#include <stdlib.h>
#include <unistd.h>
#include <ncurses.h>
#include <locale.h>


int main(){
	setlocale(LC_CTYPE, "ko_KR.utf8");
	initscr();

	int coin = 0;	
	int cmd;
	int insert;
	char YesOrNo;

	while(1){
		clear();
		printw("원하는 게임을 선택하세요!\n");
		printw("스무고개 할래 : 1\n");
		printw("야구게임 할래 : 2\n");
		printw("업다운게임 할래 : 3\n");
		printw("돈을 넣을게 : 4\n");
		printw("그만할래 : 5\n");
		printw("뭐할거야?\n");
		printw("명령어 입력 >> ");
		scanw("%d", &cmd);
		printw("\n");
		if(cmd <= 0 || cmd >= 6){
			refresh();
			printw("게임을 잘못 골랐어!\n");
			usleep(2000*1000);

		}
		else if(cmd == 1){
			if (coin <= 0){
				printw("돈이 없어... 코인을 구매해서 다시 와줘!\n");
				refresh();
				usleep(2000*1000);
				continue;
			}
			coin--;
			printw("현재 코인 : %d\n", coin);

			refresh();
			getch();
			endwin();
			system("clear");
			int ret;
			ret = system("./game1");
			if(ret == 0){
				getchar();
				clear();
				initscr();
			}

		}
		else if(cmd == 2){
			if (coin <= 0){
				printw("돈이 없어... 코인을 구매해서 다시 와줘!\n");
				refresh();
				usleep(2000*1000);
				continue;
			}
			coin--;
			printw("현재 코인 : %d\n", coin);

			refresh();
			getch();
			endwin();
			system("clear");
			int ret;
			ret = system("./game2");
			if(ret == 0){
				clear();
				initscr();
			}

		}
		else if(cmd == 3){
			if (coin <= 0){
				printw("돈이 없어... 코인을 구매해서 다시 와줘!\n");
				refresh();
				usleep(2000*1000);
				continue;
			}
			coin--;
			printw("현재 코인 : %d\n", coin);

			refresh();
			getch();
			endwin();
			system("clear");
			int ret;
			ret = system("./game3");
			if(ret == 0){
				clear();
				initscr();
			}

		}
		else if(cmd == 4){
			while(1){
				printw("현재 코인 : %d\n", coin);
				printw("몇개 넣을거야?\n");
				scanw("%d", &insert);
				coin += insert;
				printw("더 넣을거야?\n지금 코인은 %d개야! (Y/N) >> ", coin);
				//getchar();
				scanw("%c", &YesOrNo);
				if(YesOrNo == 'N' || YesOrNo == 'n'){
					printw("코인이 부족하면 또 와!\n");

					break;
				}
				else if(YesOrNo == 'Y' || YesOrNo == 'y') continue;
				else{
					printw("잘못된 입력이야! 다시 실행해줘!\n");
					break;
				}

			}
		}	
		else if(cmd == 5){
			printw("다음에 또보자!\n");
			getch();
			endwin();
			exit(0);
		}
		printw("\n\n");

			getch();
			endwin();
	}	

	return 0;
}

```
game1.c
```c
#include <stdio.h>
#include <string.h>

char riddles[20][200] = {
	"나는 안경을 썼습니까?",	//상원
	"나는 비수도권 사람인가요?",	// 상원
	"나는 자취하나요?",	// 상원
	"나는 수능을 2번 이상 봤나요?",	// 유림
	"나는 학생회 활동을 했나요?",	// 창영
	"나는 최근에 전자제품에 제법 큰돈을 썼나요?",	// 창영
	"나는 노래부르기를 좋아하나요?",	// 창영
	"나는 SK하이닉스에서 근무했나요?",	// 강사님
	"나는 C를 좋아합니까?",	// 강사님
	"나는 창업 계획이 있나요?",	// 강사님
	"나는 학부인턴을 한 경험이 있나요?",	// 창영, 유림
	"나는 맥북을 가지고 있나요?",	// 창영, 유림
	"나는 20대 후반입니까?", // 상원, 창영
	"나의 이름 모두에 받침이 들어가나요?",	// 상원, 창영
	"나는 애주가입니까?",	// 상원, 창영
	"나는 남자인가요?",	// 상원, 창영, 강사님
	"나는 아이폰을 사용하나요?",	// 유림, 강사님
	"나는 컴퓨터공학 전공자인가요?",	// 상원, 창영, 유림, 강사님
	"나는 서울 16반인가요?",	// 상원, 창영, 유림, 강사님
};



void ask(char* ans){

	
	for(int i = 0 ; i < 19 ; i++){
        char temp[100];
		printf("(%d) %s (Y/N) >> ",i+1,riddles[i]);
		ans[i] = '7';
		int chances = 5;
		do{
			scanf("%s", temp);
            if(strlen(temp) != 1){
                if(chances <= 0){
                    printf("기회를 모두 사용하셨습니다. 자동으로 Y가 부여됩니다.\n");
                    ans[i] = '1';
                }
                printf("\n  Y 혹은 N 로 대답해주세요. 남은 기회 >> %d\n",chances);
                printf("(%d) %s (Y/N) >> ",i+1,riddles[i]);
                chances--;
            }
            else{
                switch(temp[0]){
                    case 'Y':
                        ans[i] = '1';
                        break;
                    case 'N':
                        ans[i] = '0';
                        break;
                    default:
                        if(chances <= 0){
                            printf("기회를 모두 사용하셨습니다. 자동으로 Y가 부여됩니다.\n");
                            ans[i] = '1';
                        }
                        printf("\n  Y 혹은 N 로 대답해주세요. 남은 기회 >> %d\n",chances);
                        printf("(%d) %s (Y/N) >> ",i+1,riddles[i]);
                        chances--;
                        break;
                }
            }
			
		}while((ans[i] != '1')&&( ans[i] != '0'));
	}
	
	return;
}


int check(char* ans){
    char people[4][20] = {
        "0000000111000001111",
        "0000000111000001111",
        "1110000000001111011",
        "0001000000110000111"
    };
	// char* choi = "0000000111000001111";
	// char* maeng = "0000111000111110011";
	// char* mok = "1110000000001111011";
	// char* yun = "0001000000110000111";
	
	char lastQuestion[100];
	char answer = '7';

    int props[4] = {0,};

    for(int i = 0 ; i < 19 ; i++){
        for(int person = 0 ; person < 4 ;person++){
            props[person] += (people[person][i] == ans[i]);
        }
    }

    int possibleAns = 0;
    int maxProp = props[possibleAns];
    
    for(int i = 0 ; i < 4; i++){
        possibleAns = props[i] >= maxProp ? i : possibleAns;
        
    }
	
	if(strcmp(people[0], ans) == 0){
		strcpy(lastQuestion, "나는 최진혁 교수님입니까?");
	}
	else if(strcmp(people[1], ans) == 0){
		strcpy(lastQuestion, "나는 맹창영입니까?");
	}
	else if(strcmp(people[2], ans) == 0){
		strcpy(lastQuestion, "나는 목상원입니까?");
	}
	else if(strcmp(people[3], ans) == 0 ){
		strcpy(lastQuestion, "나는 윤유림입니까?");
	}
	else{

        switch(possibleAns){
            case 0:
                strcpy(lastQuestion, "나는 최진혁 교수님입니까?");
                break;
            case 1:
                strcpy(lastQuestion, "나는 맹창영입니까?");
                break;
            case 2:
                strcpy(lastQuestion, "나는 목상원입니까?");
                break;
            case 3:
                strcpy(lastQuestion, "나는 윤유림입니까?");
                break;
            default:
                strcpy(lastQuestion, "나는 최진혁 교수님, 맹창영, 목상원, 윤유림 중에 있습니까?");
                break;
        }
	}
	
	int chances = 5;

    printf("(%d) %s (Y/N) >> ",20,lastQuestion);
	do{
        char temp[100];
        scanf("%s", temp);
        if(strlen(temp) != 1){
            if(chances <= 0){
                printf("기회를 모두 사용하셨습니다. 자동으로 Y가 부여됩니다.\n");
                answer = 'Y';
            }
            printf("\n  Y 혹은 N 로 대답해주세요. 남은 기회 >> %d\n",chances);
            printf("(%d) %s (Y/N) >> ",20,lastQuestion);
            chances--;
        }
        else{
            switch(temp[0]){
                case 'Y':
                    return 1;
                case 'N':
                    return 0;
                default:
                    if(chances <= 0){
                        printf("기회를 모두 사용하셨습니다. 자동으로 Y가 부여됩니다.\n");
                        answer = 'Y';
                    }
                    printf("	Y 혹은 N 로 대답해주세요. 남은 기회 >> %d\n",chances);
                    chances--;
                    break;
            }
        }
	}while(answer != 'Y' && answer != 'N');
	
	
	return 1;	
}

int play20Riddles(){
	int ans[21];
	
	ask(ans);
	ans[19] = '\0';
	if(check(ans) == 1){
		printf("컴퓨터가 승리했습니다~! 와~!\n");
	}
	else{
		printf("플레이어가 승리했네요. 축하합니다.\n");
	}

	sleep(2);

	return 0;
	
}

int main(){
     play20Riddles();
     return 0;
}

```
game2.c
```c
#include "baseball.h"

void Init()
{
    Baseball.cur_inning = Baseball.input_number = Baseball.is_end = 0;
    // init info_inning
    for (int i = 0; i < 9; ++i) {
        for (int j = 0; j < 3; ++j) {
            Baseball.info_inning[i][j] = 0;
        }
    }
    srand((unsigned int)time(NULL));

    Baseball.answer[0] = rand() % 9 + 1;

    Baseball.answer[1] = rand() % 9 + 1;
    while (Baseball.answer[0] == Baseball.answer[1]) {
        Baseball.answer[1] = rand() % 9 + 1;
    }

    Baseball.answer[2] = rand() % 9 + 1;
    while (Baseball.answer[2] == Baseball.answer[0] || Baseball.answer[2] == Baseball.answer[1]) {  // 1, 2 번째 자리와 다른게 나올 때까지
        Baseball.answer[2] = rand() % 9 + 1;
    }
}

void PrintStart()
{
    printf("숫자 야구를 시작하겠습니다!\n");
}

void PrintInning()
{
    for (int i = 0; i < Baseball.cur_inning; ++i) {
        printf("%d회 --> %d, %d Strike, %d Ball\n",
               i + 1, Baseball.info_inning[i][2], Baseball.info_inning[i][0], Baseball.info_inning[i][1]);

        if (Baseball.info_inning[i][0] == 3) {
            printf("Homerun!!! 정답입니다\n");
            return;
        }
    }
    return;
}

void InputNumber()  // 숫자 입력
{
    printf("숫자를 입력하세요 : ");
    scanf("%d", &Baseball.input_number);

    while (IsCorrectNumber(Baseball.input_number) == 0) {
        printf("%d는 잘못된 숫자입니다.\n다시 입력하세요 : ", Baseball.input_number);
        scanf("%d", &Baseball.input_number);
    }
}

int IsCorrectNumber(int number)  // 올바른 범위의 숫자인지
{
    // int number = Baseball.input_number;
    if (number < 100 || number > 999) {
        return 0;
    }

    Baseball.my_ball[2] = number % 10;
    number /= 10;

    Baseball.my_ball[1] = number % 10;
    if (Baseball.my_ball[1] == Baseball.my_ball[2]) {
        return 0;
    }
    number /= 10;

    Baseball.my_ball[0] = number % 10;
    if (Baseball.my_ball[0] == Baseball.my_ball[1] || Baseball.my_ball[0] == Baseball.my_ball[2]) {
        return 0;
    }

    return 1;
}

int CheckNumber()
{
    for (int i = 0; i < 3; ++i) {
        for (int j = 0; j < 3; ++j) {
            if (Baseball.my_ball[i] == Baseball.answer[j]) {
                if (i == j) {
                    ++Baseball.info_inning[Baseball.cur_inning][0];
                }
                else
                    ++Baseball.info_inning[Baseball.cur_inning][1];
                break;
            }
        }
    }
    Baseball.info_inning[Baseball.cur_inning][2] = Baseball.input_number;
    ++Baseball.cur_inning;

    if (Baseball.info_inning[Baseball.cur_inning - 1][0] == 3) {
        return 1;
    }
    return 0;
}

int AskRestart()
{
    char restart[10];
    getchar();
    setbuf(stdin, NULL);
    printf("게임을 한 번 더 하시겠습니까? (Y/N)");
    scanf("%s", restart);
    if (restart[1] == 0 && (restart[0] == 'Y' || restart[0] == 'y')) {
        printf("게임을 다시 시작합니다.\n\n");
        system("clear");
        return 1;
    }
    else if (restart[1] == 0 && (restart[0] == 'N' || restart[0] == 'n')) {
        printf("게임을 종료합니다......\n\n");
        return 0;
    }
    else {
        printf("알 수 없는 입력으로 인하여 게임을 종료합니다.\n\n");
        return 0;
    }
}

int CheckInning()
{
    if (Baseball.cur_inning == 9) {
        printf("모든 이닝이 종료되었습니다.\n");
        printf("정답은 %d 입니다.\n", Baseball.answer[0] * 100 + Baseball.answer[1] * 10 + Baseball.answer[2]);

        return 0;
    }
    return 1;
}

int StartClass()
{
    int is_homerun = 0;

    Init();
    PrintStart();

    sleep(5);

    while (!is_homerun) {
        InputNumber();

        system("clear");

        is_homerun = CheckNumber();

        PrintInning();
        if (CheckInning() == 0) {
            break;
        }
    }
    return AskRestart();
}

```

game3.c
```c

#include <fcntl.h>
#include <stdio.h>
#include <stdlib.h>
#include <string.h>
#include <time.h>
#include <unistd.h>

int answer = 0;
int cnt = 0;

int FindNumber(int lo, int hi)
{
    int mid = (lo + hi) >> 1;
    char x[10000];

    while (1) {
        memset(x, 0, sizeof(x));
        printf("%d ~ %d 사이의 값을 입력하세요 : ", lo, hi);
        scanf("%s", x);

        int is_num = 1;
        for (int i = 0; x[i] != 0; ++i) {
            if (x[i] < '0' || x[i] > '9') {
                printf("숫자가 아닙니다.\n");
                is_num = 0;
                break;
            }
        }
        if (is_num == 0) {
            continue;
        }

        int num = 0;
        for (int i = 0; i < x[i] != 0; ++i) {
            num *= 10;
            num += (x[i] - '0');
        }
        if (num < 1) {
            printf("입력하신 값이 자연수가 아닙니다.\n");
            continue;
        }
        else if (num < lo || num > hi) {
            printf("입력하신 값이 범위에서 벗어납니다.\n");
            continue;
        }

        printf("\n");
        if (answer < num) {
            printf("answer는 %d보다 작습니다.\n\n", num);
            ++cnt;
        }
        else if (answer > num) {
            printf("answer는 %d보다 큽니다.\n\n", num);
            ++cnt;
        }
        else if (answer == num) {
            printf("정답은 %d(이)였습니다!!\n\n", num);
            printf("%d번 만에 성공하셨습니다!\n", cnt + 1);
            break;
        }
    }
}

int main()
{
    srand(time(NULL));

    char l[10000], h[10000];
    int low, high;

    while (1) {
        while (1) {
            low = high = 0;
            printf("자연수 숫자 범위를 지정하세요(low, high 입력, low ~ high) : ");
            scanf("%s %s", l, h);

            int is_num = 1;
            for (int i = 0; l[i] != 0; ++i) {
                if (l[i] < '0' || l[i] > '9') {
                    printf("low가 자연수가 아닙니다.\n");
                    is_num = 0;
                    break;
                }
            }
            if (is_num == 0) {
                continue;
            }
            is_num = 1;
            for (int i = 0; h[i] != 0; ++i) {
                if (h[i] < '0' || h[i] > '9') {
                    printf("high가 자연수가 아닙니다.\n");
                    is_num = 0;
                    break;
                }
            }
            if (is_num == 0) {
                continue;
            }
            else {
                break;
            }
        }
        for (int i = 0; i < l[i] != 0; ++i) {
            low *= 10;
            low += (l[i] - '0');
        }
        for (int i = 0; i < h[i] != 0; ++i) {
            high *= 10;
            high += (h[i] - '0');
        }

        if (low <= high) {
            break;
        }
        else if (low < 1 || high < 1) {
            printf("low(%d) 또는 high(%d) 값이 자연수가 아닙니다.\n", low, high);
        }
        else if (low > high) {
            printf("low(%d)의 값이 high(%d)의 값보다 큽니다.\n", low, high);
        }
    }
    answer = rand() % (high - low + 1) + low;

    for (int i = 3; i >= 0; --i) {
        printf("%d초 후에 시작합니다...\n", i);
        sleep(1);
    }
    system("clear");

    FindNumber(low, high);

    sleep(1);
    printf("게임을 종료합니다.\n");

    return 0;
}

```
