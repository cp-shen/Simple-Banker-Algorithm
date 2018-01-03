# Simple-Banker-Algorithm
Simple-Banker-Algorithm in C

```C
#include <stdio.h>

int is_sufficient(const int* cur_avail, const int* cur_req){
    for(int i = 0; i < 3; i++){
        if(cur_avail[i] < cur_req[i]){
            return 0;
        }
    }
    return 1;
}

void execute_process(int* cur_avail, const int* allocated){
    for(int i = 0; i < 3; i++){
        cur_avail[i] += allocated[i];
    }
}

int main() {
    printf("Assume that we have five processes and three kinds of resources.\n");

    //maximum requirements
    int max_req[5][3] = {{7, 5, 3}, {3, 2, 2}, {9, 0, 2}, {2, 2, 2}, {4, 3, 3}};
    //already allocated
    int allocated[5][3] = {{0, 1, 0}, {2, 0, 0}, {3, 0, 2}, {2, 1, 1}, {0, 0, 2}};
    //current requirements
    int cur_req[5][3] = {{0, 0, 0}, {0, 0, 0}, {0, 0, 0}, {0, 0, 0}, {0, 0, 0}};
    //current available
    int cur_avail[3] = {3, 3, 2};
    //safety sequence
    int safe_seq[5] = {0, 0, 0, 0, 0};
    //record the status of processes
    int is_executed[5] = {0, 0, 0, 0, 0};

    int i, j = 0;

    printf("init values of each kind of resources are\na__b__c\n10__5__7\n");
    printf("current available part of each kind of resources are\na__b__c\n3__3__2\n");

    printf("max requirement for each resources are\n____----____a__b__c\n");
    for(i = 0; i < 5; i++){
        printf("process#%d: ", i);
        for(j = 0; j < 3; j ++){
            printf("%2d ", max_req[i][j]);
            cur_req[i][j] = max_req[i][j] - allocated[i][j];
        }
        printf("\n");
    }

    printf("allocated resources are\n____----____a__b__c\n");
    for(i = 0; i < 5; i++){
        printf("process#%d: ", i);
        for(j = 0; j < 3; j++){
            printf("%2d ", allocated[i][j]);
        }
        printf("\n");
    }

    printf("current requirements are\n____----____a__b__c\n");
    for(i = 0; i < 5; i++){
        printf("process#%d: ", i);
        for(j = 0; j < 3; j++){
            printf("%2d ", cur_req[i][j]);
        }
        printf("\n");
    }

    j = 0;
    for(i = 0; i < 5; i++){
        if(is_executed[i] == 0 && is_sufficient(cur_avail, cur_req[i])){
            execute_process(cur_avail, allocated[i]);
            is_executed[i] = 1;
            safe_seq[j] = i;
            j++;
            i = -1;
        }
    }


    int is_safe = 1;
    for(i = 0; i < 5; i++){
        if(is_executed[i] == 0){
            is_safe = 0;
            break;
        }
    }

    if(is_safe){
        printf("Safe!\n");
        for(i = 0; i < 5; i++){
            printf(" ->%d", safe_seq[i]);
        }
    }else{
        printf("Unsafe!\n");
    }

    return 0;
}
```
