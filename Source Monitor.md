```c
#include <stdlib.h>
#include <stdio.h>
#include <string.h>
#include <unistd.h>
#include <sys/types.h>
#include <sys/stat.h>

char path[500];
int now = 0;
int main(int argc, char *argv[]){
    if(argc == 1){
        return 0;
    }
    strcpy(path, argv[1]);
    struct stat fileInfo;
    while(1){
        stat(path, &fileInfo);
        if(!(&fileInfo)){
            printf("ERROR\n");
            return 0;
        }
        //최초실행
        else if(now == 0){
            now = (int)fileInfo.st_mtime;
            char cmd[100];
            strcpy(cmd, "gcc ");
            strcat(cmd, path);
            system(cmd);
            usleep(500*1000);
            system("./a.out");
            printf("\n");
        }

        else{
            if (now != fileInfo.st_mtime){
                now = (int)fileInfo.st_mtime;
                system("clear");
                printf("========file changed========\n");
                char cmd[100];
                strcpy(cmd, "gcc ");
                strcat(cmd, path);
                system(cmd);
                usleep(500 * 1000);

                system("./a.out");
                printf("\n");
            }
        }
        
    }
}

```
![image](https://user-images.githubusercontent.com/60029949/116840587-5aa33900-ac11-11eb-9903-37feb78496db.png)
