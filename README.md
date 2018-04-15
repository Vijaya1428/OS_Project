

#include <unistd.h>
#include <stdio.h>
#include <conio.h>
#include <stdlib.h>
#include <pthread.h>  
#include <semaphore.h> 
#include <string.h>


pthread_t *p,*c; 

sem_t b, Counter1 , Counter2 ;
int *buffer;
int buffer_pos = -1;
int P_count , C_count , buffer_length;


int prod(pthread_t a);
void cons(int j,pthread_t a);
void* Prod();
void* Cons();

int main()
{	
	int z;
	int count;
	sem_init(&b ,0,1);
	sem_init(&Counter2,0,0);

	printf("\nenter the no. of producers:");
	scanf("%d",&P_count);
	p = (pthread_t*) malloc(P_count*sizeof(pthread_t));

	printf("\nenter the no. of consumers:");
	scanf("%d",&C_count);
	c = (pthread_t*) malloc(C_count*sizeof(pthread_t));

	printf("\nenter the buffer size :");
	scanf("%d",&buffer_length);
	buffer = (int*) malloc(buffer_length*sizeof(int));

	sem_init(&Counter1 , 0 , buffer_length);

	for(z = 0 ; z < P_count ; z++)
	{
	count = pthread_create(prod+z, NULL , &Prod, NULL);
	if(count != 0)
	{
	printf("\n the producers which is creating error is::  %d: %s", z+1 , strerror(count));
	}
	else
	{
	printf("\n the producers which is successfully created is::%d", z+1);
	}
	}

	for(z=0 ; z < C_count ; z++)
	{
	count = pthread_create(cons+z, NULL, &Cons, NULL);
	if(count != 0)
	{
	printf("\nerror has occured....");
	printf(" consumer %d can't be created : %s" , z+1, strerror(count));
	}
	else
	{
	printf("\n the successfully created consumers is:: %d", z+1);
	}
	}

	for(z=0 ; z<P_count ; z++)
	{
	pthread_join(*(p+z),NULL);
	}
	for(z=0 ; z < C_count ; z++)
	{
	pthread_join(*(c+z),NULL);
	}


	return 0; 
}


int prod(pthread_t a)
{
	int i = 0;
	printf("\n\n here number of producer is:: %d ",i);
	return p;


	int j = 1 + rand()%40;
	while(!pthread_equal(*(p+i),a) && i < P_count)
	{
	i++;
	}
	printf("\n here no of Producer is %d  and it produces %d", i+1, j);
	return j;
}

void cons(int j, pthread_t a)
{
	int k = 0;
	while(!pthread_equal(*(c+k),a) && k < C_count)
	{
	k++;
	}

	printf("\n no of Buffer:");
	for(k=0 ; k <= buffer_pos ; ++k)
	printf("%d ",*(buffer + k));
	printf("\nConsumer %d has consumed %d\n", k+1 , j );
	printf("\nCurrent buffer len is: %d\n", buffer_pos);
}


void* Prod(){

	while(1)
	{
	int x = prod(pthread_self());
	sem_wait(&Counter1);
	sem_wait(&b);

	++buffer_pos;
	*(buffer + buffer_pos) = x; 
	sem_post(&b);
	sem_post(&Counter2);
	sleep(1 + rand()%3);
	}
	
	return NULL;
}

void* Cons(){
	int w;
	while(1){
	sem_wait(&Counter2);
	sem_wait(&b);
	w = *(buffer + buffer_pos);
	cons(c , pthread_self());
	--buffer_pos;
	sem_post(&b);
	sem_post(&Counter1);
	sleep(1+rand()%3);
	}
      return NULL;
}

