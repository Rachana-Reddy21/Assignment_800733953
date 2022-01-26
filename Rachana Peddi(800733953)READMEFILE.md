# Producer-Consumer Problem
Solution for the producer/consumer problem using processes instead of threads.

## Description
In this the producer is allowed to produce an item, and this is stored into shared memory.


Consumer is allowed to pick the item produced from named chunk of shared memory

File creation for  producer.c and consumer.c

````
vi producer.c

vi consumer.c
````

The above lines would create files for producer and consumer.


## Codes for producer and Consumer.


###producer.c

````
#include<stdio.h>
#include<stdlib.h>
#include<string.h>
#include<sys/types.h>
#include<fcntl.h>
#include<sys/shm.h>
#include<sys/mman.h>
#include<unistd.h>
#include<sys/stat.h>

int main(int args, char *argv[])
{
        pid_t producer_processid;
        producer_processid=fork();
        int temp = getppid();
        printf("Producer is running the process id:%d\n", temp);
        const int size = 4096;
        const char *name="assignment";
        const char *m1 = "Hello My dear Customer(item produced by producer is):";
        //m1=m1+str(temp);


        char *m2=argv[1];


int fd;
char *ptr;
fd = shm_open(name,O_CREAT|O_RDWR,0666);
ftruncate(fd,size);
ptr = (char*)mmap(0,size,PROT_READ|PROT_WRITE,MAP_SHARED,fd,0);
sprintf(ptr,"%s",m1);
ptr+=strlen(m1);
sprintf(ptr,"%s",m2);
ptr+=strlen(m2);
return 0;

}
````
###consumer.c

````
#include<stdio.h>
#include<stdlib.h>
#include<string.h>
#include<sys/types.h>
#include<fcntl.h>
#include<sys/shm.h>
#include<sys/mman.h>
#include<unistd.h>
#include<sys/stat.h>

int main(int args, char *argv[])
{
const int size = 4096;
const char *name = "assignment";
int fd;
char *ptr;
fd = shm_open(name, O_CREAT|O_RDWR, 0666);
ptr = (char*)mmap(0, size, PROT_READ|PROT_WRITE, MAP_SHARED, fd, 0);
printf("%s",(char*)ptr);
shm_unlink(name);
return 0;
}

````


##The execution steps are mentioned in make file, shown below

````
all: producer consumer

producer: producer.c
        gcc producer.c -o prod -lrt
        ./prod item_produced_by_800733953

consumer: consumer.c
        gcc consumer.c -o cons -lrt
        ./cons


````
Instead of executing make file we can follow the below procedure
````
1) Create the files producer.c and consumer.c

Open 2 different tabs for execution

Tab1 :
stepa---> gcc producer.c -o prod -lrt
stepb--->  ./prod "value to be passed for the customer or (insertion into buffer)"


Tab2:

Now in Tab2 , execute the consumer file as follows
1)  gcc consumer.c -o cons -lrt
2) ./cons
````
