# disk-scheduling-system
```c
#include <stdio.h> 
#include <stdlib.h> 
#include <stdbool.h>
#include <limits.h>
// Function prototypes
void FCFS(int requests[], int n, int head); void SSTF(int requests[], int n, int head);
void SCAN(int requests[], int n, int head, int disk_size); void C_SCAN(int requests[], int n, int head, int disk_size); void LOOK(int requests[], int n, int head);
int findClosestRequest(int requests[], bool serviced[], int n, int head); int main() {
int n, head, disk_size;
int algorithm_choice;
printf("Enter the number of disk requests: "); scanf("%d", &n);
int requests[n];
printf("Enter the size of the disk: "); scanf("%d", &disk_size);
printf("Enter the disk requests:\n"); for (int i = 0; i < n; i++) {
printf("Request %d: ", i + 1); scanf("%d", &requests[i]);
if (requests[i] >= disk_size || requests[i] < 0) {
printf("Invalid request. Request must be less than disk size and non- negative.\n");
i--; // Reprompt for the same index
}
}
printf("Enter the initial head position: "); scanf("%d", &head);
printf("Select Disk Scheduling Algorithm:\n"); printf("1. FCFS\n");
 
printf("2. SSTF\n");
printf("3. SCAN\n");
printf("4. C-SCAN\n");
printf("5. LOOK\n"); printf("Enter your choice: "); scanf("%d", &algorithm_choice);
switch (algorithm_choice) { case 1:
FCFS(requests, n, head); break;
case 2:
SSTF(requests, n, head); break;
case 3:
SCAN(requests, n, head, disk_size); break;
case 4:
C_SCAN(requests, n, head, disk_size); break;
case 5:
LOOK(requests, n, head); break;
default:
printf("Invalid choice!\n");
}
return 0;
}
// FCFS Disk Scheduling Algorithm
void FCFS(int requests[], int n, int head) { int total_seek_time = 0;
printf("FCFS Disk Scheduling:\n"); for (int i = 0; i < n; i++) {
printf("Servicing request at: %d\n", requests[i]); total_seek_time += abs(requests[i] - head); head = requests[i];
}
printf("Total seek time: %d\n\n", total_seek_time);
}
// SSTF Disk Scheduling Algorithm
int findClosestRequest(int requests[], bool serviced[], int n, int head) {
 
int min_distance = INT_MAX; int closest_request_index = -1; for (int i = 0; i < n; i++) {
if (!serviced[i] && abs(requests[i] - head) < min_distance) { min_distance = abs(requests[i] - head); closest_request_index = i;
}
}
return closest_request_index;
}
void SSTF(int requests[], int n, int head) { int total_seek_time = 0;
bool serviced[n];
for (int i = 0; i < n; i++) serviced[i] = false;
printf("SSTF Disk Scheduling:\n"); for (int i = 0; i < n; i++) {
int closest_request_index = findClosestRequest(requests, serviced, n, head);
serviced[closest_request_index] = true;
printf("Servicing request at: %d\n", requests[closest_request_index]); total_seek_time += abs(requests[closest_request_index] - head); head = requests[closest_request_index];
}
printf("Total seek time: %d\n\n", total_seek_time);
}
// SCAN Disk Scheduling Algorithm
void SCAN(int requests[], int n, int head, int disk_size) { int total_seek_time = 0;
int i, j;
int sorted_requests[n + 1];
for (i = 0; i < n; i++) sorted_requests[i] = requests[i]; sorted_requests[n] = head;
n++;
for (i = 0; i < n - 1; i++) {
for (j = 0; j < n - i - 1; j++) {
if (sorted_requests[j] > sorted_requests[j + 1]) { int temp = sorted_requests[j]; sorted_requests[j] = sorted_requests[j + 1]; sorted_requests[j + 1] = temp;
 
}
}
}
int head_index = 0; for (i = 0; i < n; i++) {
if (sorted_requests[i] == head) { head_index = i;
break;
}
}
printf("SCAN Disk Scheduling:\n"); for (i = head_index; i < n; i++) {
printf("Servicing request at: %d\n", sorted_requests[i]); total_seek_time += abs(sorted_requests[i] - head); head = sorted_requests[i];
}
for (i = head_index - 1; i >= 0; i--) {
printf("Servicing request at: %d\n", sorted_requests[i]); total_seek_time += abs(sorted_requests[i] - head); head = sorted_requests[i];
}
printf("Total seek time: %d\n\n", total_seek_time);
}
// C-SCAN Disk Scheduling Algorithm
void C_SCAN(int requests[], int n, int head, int disk_size) { int total_seek_time = 0;
int i, j;
int sorted_requests[n + 2];
for (i = 0; i < n; i++) sorted_requests[i] = requests[i]; sorted_requests[n] = head;
sorted_requests[n + 1] = disk_size - 1; n += 2;
for (i = 0; i < n - 1; i++) {
for (j = 0; j < n - i - 1; j++) {
if (sorted_requests[j] > sorted_requests[j + 1]) { int temp = sorted_requests[j];
 
sorted_requests[j] = sorted_requests[j + 1]; sorted_requests[j + 1] = temp;
}
}
}
int head_index = 0; for (i = 0; i < n; i++) {
if (sorted_requests[i] == head) { head_index = i;
break;
}
}
printf("C-SCAN Disk Scheduling:\n"); for (i = head_index; i < n; i++) {
printf("Servicing request at: %d\n", sorted_requests[i]); total_seek_time += abs(sorted_requests[i] - head); head = sorted_requests[i];
}
for (i = 0; i < head_index; i++) {
printf("Servicing request at: %d\n", sorted_requests[i]); total_seek_time += abs(sorted_requests[i] - head); head = sorted_requests[i];
}
printf("Total seek time: %d\n\n", total_seek_time);
}
// LOOK Disk Scheduling Algorithm
void LOOK(int requests[], int n, int head) { int total_seek_time = 0;
int i, j;
int sorted_requests[n + 1];
for (i = 0; i < n; i++) sorted_requests[i] = requests[i]; sorted_requests[n] = head;
n++;
for (i = 0; i < n - 1; i++) {
for (j = 0; j < n - i - 1; j++) {
if (sorted_requests[j] > sorted_requests[j + 1]) {
 
int temp = sorted_requests[j]; sorted_requests[j] = sorted_requests[j + 1]; sorted_requests[j + 1] = temp;
}
}
}
int head_index = 0; for (i = 0; i < n; i++) {
if (sorted_requests[i] == head) { head_index = i;
break;
}
}
printf("LOOK Disk Scheduling:\n"); for (i = head_index; i < n; i++) {
printf("Servicing request at: %d\n", sorted_requests[i]); total_seek_time += abs(sorted_requests[i] - head); head = sorted_requests[i];
}
for (i = head_index - 1; i >= 0; i--) {
printf("Servicing request at: %d\n", sorted_requests[i]); total_seek_time += abs(sorted_requests[i] - head); head = sorted_requests[i];
}
printf("Total seek time: %d\n\n", total_seek_time
);
} 
```c
