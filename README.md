# OS_Project
/*project regarding reader writers problem*/


#include <unistd.h>
#include <stdio.h> 
#include <pthread.h>  
#include <semaphore.h> 

pthread_t *p,*c; 

sem_t b,Counter1=0,Counter2=0;

void  prod(pthread_t a);

int main()
{	
	int i, PrNum=0;  CnNum=0;      
	
	printf("\nEnter the no of Producers:");   
	scanf("%d",&PrNum);  
	printf("\nEnter the no of Consumers:");
	scanf("%d",&CnNum);
	printf("\nSuccess...");

	return 0; 
}


void prod(pthread_t a)
{
	int i = 0;
	printf("\n\n here number of producer is:: %d ",i);
	return p;
}

