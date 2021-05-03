```c
#include <ncurses.h>
#include <stdio.h>
#include <stdlib.h>
#include <locale.h>
#include <unistd.h>

#define size 7

char map[size][size+1] = {
	"#######",
	"#  M  #",
	"# ##  #",
	"#    I#",
	"#  ^###",
	"#    Y#",
	"#######"
};

int ny = 1;
int nx = 1;

int hp = 100;

void print(){
	clear();

	for(int y = 0 ; y < size ; y++){
		for(int x = 0 ; x < size ; x++){
			if(y == ny && x == nx){
				printw("ቼቼ");
			}
			else {
				if(map[y][x] == '#')printw("▒▒");
				else if(map[y][x]==' ') printw("  ");
				else if(map[y][x]=='M') printw("㉿");
				else if(map[y][x]=='I') printw("ꇳ");
				else if(map[y][x]=='Y') printw("ᕩᕩ");
				else if(map[y][x]=='^') printw("^^");
				
				
			}
		}
		printw("\n");
	}
	printw("HP : %d",hp);
	refresh();
}



int main(){
	setlocale(LC_CTYPE, "ko_KR.utf8");
	initscr();

	nodelay(stdscr, TRUE);
	keypad(stdscr, TRUE);
	while(1){
		print();

		int ch = getch();
		if(ch == ERR) ch = 0;
		if(ch == KEY_LEFT){
			if(map[ny][nx-1] != '#') nx--;
			if(map[ny][nx] == '^') hp -= 10;
		}
		if(ch == KEY_RIGHT){
			if(map[ny][nx+1] != '#') nx++;
			if(map[ny][nx] == '^') hp -= 10;
		}
		if(ch == KEY_UP){
			if(map[ny-1][nx] != '#') ny--;
			if(map[ny][nx] == '^') hp -= 10;
		}
		if(ch == KEY_DOWN){
			if(map[ny+1][nx] != '#') ny++;
			if(map[ny][nx] == '^') hp -= 10;
		}

		if(map[ny][nx] == 'I'){
			hp = 100;
			map[ny][nx] = ' ';
		}

		if(map[ny][nx] == 'M' || hp == 0){
			print();
			usleep(500 * 1000);
			clear();
			mvprintw(10,30, "GAME OVER");
			refresh();
			sleep(1);
			break;
		}
		if(map[ny][nx] == 'Y'){
			print();
			usleep(500 * 1000);
			clear();
			mvprintw(10,30, "CLEAR!\n HP : %d",hp);
			refresh();
			sleep(1);
		}

	}

	getch();
	endwin();
	return 0;
}

```
![image](https://user-images.githubusercontent.com/60029949/116840663-a48c1f00-ac11-11eb-818d-bd468041e1db.png)
