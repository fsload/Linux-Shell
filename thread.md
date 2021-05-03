```c
include <pthread.h>
#include <ncurses.h>
#include <unistd.h>

pthread_mutex_t mlock;

void *abc(){

	while(1) {
		clear();
		for(int i = 0;i <= 30 ; i++){
			//clear();

			pthread_mutex_lock(&mlock);
			for(int j=0 ; j<= i ; j++){

				mvprintw(10,30+j,"*");				
				refresh();

			}

			pthread_mutex_unlock(&mlock);
			usleep(50000);

		}

		for(int i = 30 ; i >= 0 ; i--){
			clear();

			pthread_mutex_lock(&mlock);

			for(int j = i ; j >= 0 ; j --){

				mvprintw(10,30+j,"*");		
				refresh();

			}

			pthread_mutex_unlock(&mlock);
			usleep(50000);

		}
	}
	return 0;
}

void *count(){
	int cnt = 0;
	while(1){
		//clear();
		pthread_mutex_lock(&mlock);
		mvprintw(10,10,"%d",cnt++);
		refresh();
		pthread_mutex_unlock(&mlock);
		usleep(50000);

	}
	return 0;
}

int main(){

	initscr();

	clear();

	pthread_t tid, tid2;
	pthread_create(&tid, NULL, count, NULL);
	pthread_create(&tid2, NULL, abc, NULL);

	pthread_join(tid, NULL);
	pthread_join(tid2, NULL);

	getch();
	endwin();

	return 0;
}

```

https://user-images.githubusercontent.com/60029949/116840719-e87f2400-ac11-11eb-9062-3cbc2073a8d8.mp4

