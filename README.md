# OS-Practical_Day_25-16.06.2025-

Thread->
* A thread is a lightweight unit of execution within a process in an operating system. Think of a process as a program running on your computer, and threads as smaller tasks within that program.

Practical_01:

#include <stdio.h>
#include <stdlib.h>
#include <unistd.h>
#include <pthread.h>

// A normal C function that is executed as a thread 
// when its name is specified in pthread_create()

void *helloworld(void *vargp){
	sleep(1);
	printf("Hello World \n!");
	return NULL;
}

int main(){
	pthread_t thread_id;
	printf("Before Thread \n");
	pthread_create(&thread_id, NULL, helloworld, NULL);
	pthread_join(thread_id, NULL);
	printf("After Thread \n");
	exit(0);
}

Conclusion: 
* Thread creation: The program uses pthread_create() to start a new thread that runs the helloworld function.
* Sleep delay: Inside the thread function, sleep(1) pauses execution for 1 second before printing.
* Output from thread: After the delay, it prints Hello World!.
* Thread synchronization: pthread_join() is used in main() to wait until the thread finishes before continuing.
* Order of execution:
      "Before Thread" is printed first.
      Then the new thread prints "Hello World!" after 1 second.
      Finally, "After Thread" is printed.
* Program ends cleanly with exit(0);

![Thread](https://github.com/user-attachments/assets/0a3a6c8f-f8f5-410e-b090-719dce591a1c)

Practical_02:
//Multi threaded process

#include <stdio.h>
#include <pthread.h>

//function to be executed by the threaded
void* print_message(void* arg){
	char* message = (char*) arg;
	printf("%s\n", message);
	return NULL;
}

int main(){
	pthread_t thread1, thread2;
		//create first thread1
	pthread_create(&thread1, NULL, print_message, "Hello from Thread 1!");
		//create first thread2
	pthread_create(&thread2, NULL, print_message, "Hello from Thread 2!");
		//wait for both threads to finish
	pthread_join(thread1, NULL);
	pthread_join(thread2, NULL);
		printf("Both thread completed. \n")
	return 0;
	}

 Conclusion: 
        * Two threads are created using pthread_create() — thread1 and thread2.
        * Each thread runs the same function print_message, but with different messages ("Hello from Thread 1!" and "Hello from Thread 2!").
        * pthread_join() waits for both threads to finish before the program continues.
        * The output will print both thread messages, though the order may vary (because threads run independently).
        * Finally, it prints "Both thread completed."

  ![Multi_Thread](https://github.com/user-attachments/assets/bb28b7c9-c470-4afd-8565-ab01eacc7f4e)


 Practical_03:
//Basic thread

#include <stdio.h>
#include <pthread.h>

void* print_message(void* arg){
	char* message = (char*) arg;
	printf("%s\n", message);
	return NULL;
	
}
int main(){
	pthread_t threads[3];
	char* messages[] = {
		"thread 1 says hi!",
		"thread 2 says hello!",
		"thread 3 says hey!"
	};
		for(int i=0; i<3; i++){
		pthread_create(&threads[i], NULL, print_message, messages[i]);
	}
		for(int i=0; i<3; i++){
		pthread_join(threads[i], NULL);
	}
		printf("All threads Done.\n");
	return 0;
}

Conclusion: 
      * The program creates 3 threads using a loop with pthread_create().
      * Each thread runs the print_message function with a different string message.
      * Threads run concurrently, so the order of printed messages may vary each time you run the program.
      * pthread_join() is used in a loop to wait for all 3 threads to finish before printing the final message.
      * After all threads complete, the program prints "All threads Done."

 ![Basic_thread](https://github.com/user-attachments/assets/44d83fc4-b33c-446a-8026-e8f597c35fec)
     

Practical_04: 
//using thread to compute parts of a sum(pararel Sum)
#include <stdio.h>
#include <pthread.h>

#define SIZE 6

int arr[SIZE] = {1, 2, 3, 4, 5, 6};
int sum1 = 0, sum2 = 0;

void* sum_part1(void* arg){
	for(int i=0; i<SIZE/2; i++){
		sum1 += arr[i];
	}
	return NULL;
}

void* sum_part2(void* arg){
	for(int i = SIZE/2; i< SIZE; i++){
		sum2 += arr[i];
	}
	return NULL;
}


int main(){
	pthread_t t1, t2;
	pthread_create(&t1, NULL, sum_part1, NULL);
	pthread_create(&t2, NULL, sum_part2, NULL);
	pthread_join(t1, NULL);
	pthread_join(t2, NULL);
	printf("Total um = %d\n", sum1 + sum2);
	return 0;
}

Conclusion:
* Two threads are created to split the work of summing an array.
* The array {1, 2, 3, 4, 5, 6} is divided:
    sum_part1 sums the first half → 1+2+3 = 6
    sum_part2 sums the second half → 4+5+6 = 15
* Both threads run in parallel, making the summing process faster for large data.
* pthread_join() ensures main waits until both threads finish.
* Final result printed:
    Total sum = sum1 + sum2 = 6 + 15 = 21
