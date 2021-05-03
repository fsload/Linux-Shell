```c
#include <stdio.h>
#include <stdlib.h>
#include <string.h>

#include <stdio.h>
#include <string.h>
#include <stdlib.h>
int main(){
    char buf[100][100];
    char input[100];
    int num = 0;
    while(1){
        printf("SSAFY> ");
        scanf("%s", input);
        strcpy(buf[num++], input);
        if(strcmp(input,"exit") == 0 ){
            exit(0);
        }
        else if(strcmp(input,"uptime") == 0){
            system("uptime");
        }
        else if(strcmp(input,"date") == 0){
            system("date");
        }
        else if(strcmp(input,"ls") == 0){
            system("ls -al");
        }
        else if(strcmp(input,"log") == 0){
            system("dmesg");
        }
        else if(strcmp(input,"history") == 0){
            for(int i = 0 ; i < num; i++){
                printf("%d %s \n", i+1, buf[i]);
            }
        }
        else if(strcmp(input,"hclear") == 0){
            for(int i = 0 ; i < 100; i++){
                for (int j = 0 ; j <100; j++){
                    buf[i][j] = '\n';
                }
            }
            num = 0;
        }
        else if(input[0] == '!'){
            int index;
            int i;
            for(i = 0 ; i < strlen(input) - 1 ; i++){
                input[i] = input[i+1];
            }
            input[i] = '\0';
            system(buf[atoi(input)-1]);
        }
        else {
            system("echo ERROR");
        }

    }
}

```

![image](https://user-images.githubusercontent.com/60029949/116840526-229bf600-ac11-11eb-9212-24af16375919.png)
![image](https://user-images.githubusercontent.com/60029949/116840537-2af43100-ac11-11eb-956c-aac800357e7b.png)

