# Bai_05_Lab05_HeDieuHanh
Tính ANS bằng việc điều khiển các biến w, x, y, v trước đó

/*######################################
#University of Information Technology#
#IT007 Operating System#
#<Pham Thanh Dat>, <20521175>#
#File: ex5.c#
######################################*/

#include <stdio.h>
#include<semaphore.h>
#include<pthread.h>
#include<stdlib.h>
#include<time.h>
int w, v, z, y;
int ans = 0, x1 = 1, x2 = 2, x3 = 3, x4 = 4, x5 = 5, x6 = 6;
sem_t A_e, A_f, B_c, B_d, C_e, D_f, E_g, F_g;
void *PROCESS_A(void *mess)
{

    w = x1 * x2;
    printf("w = x1 * x2 = %d\n", w);
    sem_post(&A_e);
    sem_post(&A_f);
}
void *PROCESS_B(void *mess)
{
    v = x3 * x4;
    printf("v = x3 * x4 = %d\n", v);
    sem_post(&B_c);
    sem_post(&B_d);
}
void *PROCESS_C(void *mess)
{
    sem_wait(&B_c);
    y = v * x5;
    printf("y = v * x5 = %d\n", y);
    sem_post(&C_e);
}
void *PROCESS_D(void *mess)
{
    sem_wait(&B_d);
    z = v * x6;
    printf("z = v * x6 = %d\n", z);
    sem_post(&D_f);
}
void *PROCESS_E(void *mess)
{
    sem_wait(&A_e);
    sem_wait(&C_e);
    y = w * y;
    printf("y = w * y = %d\n", y);
    sem_post(&E_g);
}
void *PROCESS_F(void *mess)
{
    sem_wait(&A_f);
    sem_wait(&D_f);
    z = w * z;
    printf("z = w * z = %d\n", z);
    sem_post(&F_g);
}
void *PROCESS_G(void *mess)
{
    sem_wait(&E_g);
    sem_wait(&F_g);
    ans = y + z;
    printf("ans = y + z = %d\n", ans);
}
int main()
{
    sem_init(&A_e, 0, 1);
    sem_init(&A_f, 0, 0);
    sem_init(&B_c, 0, 0);
    sem_init(&B_d, 0, 0);
    sem_init(&C_e, 0, 0);
    sem_init(&D_f, 0, 0);
    sem_init(&E_g, 0, 0);
    sem_init(&F_g, 0, 0);
    pthread_t a, b, c, d, e, f, g;
    pthread_create(&a, NULL, &PROCESS_A, NULL);
    pthread_create(&b, NULL, &PROCESS_B, NULL);
    pthread_create(&c, NULL, &PROCESS_C, NULL);
    pthread_create(&d, NULL, &PROCESS_D, NULL);
    pthread_create(&e, NULL, &PROCESS_E, NULL);
    pthread_create(&f, NULL, &PROCESS_F, NULL);
    pthread_create(&g, NULL, &PROCESS_G, NULL);
    while(1){};
    return 0;
}
