// main function with header files

#include<stdio.h>
#include<stdlib.h>
#include<unistd.h>
#include<pthread.h>
#include<errno.h>
#include<sys/ipc.h>
#include<semaphore.h>
 
#define N 5
 
time_t end_time;/*end time*/
sem_t mutex,customers,barbers;/*Three semaphors*/
int count=0;/*The number of customers waiting for haircuts*/
 
void barber(void *arg);
void customer(void *arg);
 
int main(int argc,char *argv[])
{
pthread_t id1,id2;
int status=0;
end_time=time(NULL)+20;/*Barber Shop Hours is 20s*/
 
/*Semaphore initialization*/
sem_init(&mutex,0,1);
sem_init(&customers,0,0);
sem_init(&barbers,0,1);
 
/*Barber_thread initialization*/
status=pthread_create(&id1,NULL,(void *)barber,NULL);
if(status!=0)
perror("create barbers is failure!\n");
/*Customer_thread initialization*/
status=pthread_create(&id2,NULL,(void *)customer,NULL);
if(status!=0)
perror("create customers is failure!\n");
 
/*Customer_thread first blocked*/
pthread_join(id2,NULL);
pthread_join(id1,NULL);
 
exit(0);
}
