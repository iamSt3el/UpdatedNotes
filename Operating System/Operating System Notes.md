# Operating Systems: Comprehensive Notes

## 1. Foundation of Operating Systems

An operating system (OS) is a software layer that manages computer hardware and provides services for computer programs. It serves as an intermediary between users and the computer hardware.

### 1.1 Core Functions of an Operating System

- **Resource Management**: Efficiently allocates CPU time, memory, storage, and peripheral devices among competing processes.
- **Process Management**: Creates, schedules, and terminates processes and threads.
- **Memory Management**: Allocates and deallocates memory space as needed by processes.
- **File System Management**: Organizes and maintains files and directories on storage devices.
- **I/O System Management**: Controls input/output operations to and from attached devices.
- **Security and Protection**: Ensures system integrity and protects user data.

### 1.2 Operating System Components

#### 1.2.1 Kernel

The kernel is the central component of an operating system that manages operations of the computer and hardware. It:

- Provides the lowest-level abstraction layer for resources
- Manages system calls and hardware interrupts
- Controls access to CPU, memory, and I/O devices

#### 1.2.2 Shell

The shell is the user interface that allows users to interact with the kernel:

- Command-Line Interface (CLI): Text-based interface (e.g., Bash in Linux, Command Prompt in Windows)
- Graphical User Interface (GUI): Visual interface with windows, icons, menus, and pointers

#### 1.2.3 System Programs

- Utilities for file management
- Status information programs
- Programming language support
- Program loading and execution

### 1.3 System Calls

System calls provide an interface between a process and the operating system.

**Example**: File Operations System Calls in C

```c
// Opening a file
int file_descriptor = open("example.txt", O_RDONLY);

// Reading from a file
char buffer[100];
read(file_descriptor, buffer, 100);

// Writing to a file
write(file_descriptor, "Hello, World!", 13);

// Closing a file
close(file_descriptor);
```

### 1.4 OS Boot Process

1. **Power-On Self-Test (POST)**: Hardware check
2. **Bootstrap Loader**: Loads the kernel into memory
3. **Kernel Initialization**: Sets up essential services
4. **System Configuration**: Detects hardware and loads drivers
5. **User Authentication**: Login prompt

## 2. Types of Operating Systems

Operating systems can be classified based on various criteria including processing capabilities, user interface, and design philosophy.

### 2.1 Based on Processing Capabilities

#### 2.1.1 Single-User Systems

- Designed for one user at a time
- Examples: MS-DOS, early versions of Windows
- Focus on ease of use rather than resource optimization

#### 2.1.2 Multi-User Systems

- Support multiple users simultaneously
- Provide authentication and access control mechanisms
- Examples: UNIX, Linux, Windows Server

#### 2.1.3 Batch Processing Systems

- Execute jobs without user interaction
- Group similar jobs together to improve throughput
- Historical examples: IBM's OS/360

**Example**: Batch Processing Script

```bash
#!/bin/bash
# A batch file that processes multiple data files
for file in data/*.csv
do
    echo "Processing $file..."
    python process_data.py $file
done
echo "All files processed."
```

#### 2.1.4 Time-Sharing Systems

- Multiple users share computing resources simultaneously
- Uses CPU scheduling and multiprogramming
- Examples: UNIX, modern server operating systems

#### 2.1.5 Real-Time Operating Systems (RTOS)

- Guaranteed response times for critical tasks
- Used in embedded systems, industrial control, aviation
- Examples: VxWorks, FreeRTOS, QNX

**Example**: Real-Time Task in FreeRTOS

```c
void temperatureMonitorTask(void *pvParameters) {
    for (;;) {
        float temperature = readTemperatureSensor();
        if (temperature > THRESHOLD) {
            activateEmergencyCooling();
        }
        // Ensure this task runs every 10 milliseconds
        vTaskDelay(pdMS_TO_TICKS(10));
    }
}

// Task creation
xTaskCreate(
    temperatureMonitorTask,  // Function that implements the task
    "TempMonitor",           // Text name for the task
    1000,                    // Stack size in words
    NULL,                    // Task input parameter
    3,                       // Priority (higher number = higher priority)
    NULL                     // Task handle
);
```

### 2.2 Based on User Interface

#### 2.2.1 Command Line Interface (CLI)

- Text-based interface
- Commands are typed by the user
- Examples: Unix shell, Windows Command Prompt

#### 2.2.2 Graphical User Interface (GUI)

- Visual elements like windows, icons, menus
- User interacts through mouse, keyboard, or touch
- Examples: Windows, macOS, GNOME, KDE

### 2.3 Based on Design Philosophy

#### 2.3.1 Monolithic OS

- Single large program with all functionality in kernel space
- Examples: Traditional Unix, Linux kernel
- Fast but less modular

#### 2.3.2 Microkernel OS

- Minimal kernel with basic functions
- Services run as user-space processes
- Examples: MINIX, QNX
- More stable but potentially slower

#### 2.3.3 Hybrid OS

- Combines monolithic and microkernel approaches
- Examples: Windows NT, macOS (XNU kernel)

#### 2.3.4 Distributed OS

- Appears as a single system but runs across multiple physical machines
- Examples: Amoeba, Plan 9

## 3. Memory Management

Memory management is responsible for efficiently utilizing the computer's main memory (RAM) by controlling how memory is allocated and deallocated.

### 3.1 Memory Hierarchy

1. **Registers**: Extremely fast, small storage in CPU
2. **Cache**: Faster but smaller than main memory (L1, L2, L3)
3. **Main Memory (RAM)**: Primary working memory
4. **Secondary Storage**: Hard drives, SSDs, etc.

### 3.2 Memory Management Techniques

#### 3.2.1 Fixed Partitioning

- Memory is divided into fixed-size partitions
- Simple but leads to internal fragmentation
- Each partition can hold one process

#### 3.2.2 Dynamic Partitioning

- Partitions are created dynamically based on process needs
- Eliminates internal fragmentation but can lead to external fragmentation
- Requires allocation algorithms: First-fit, Best-fit, Worst-fit

#### 3.2.3 Paging

- Physical memory is divided into fixed-size frames
- Logical memory is divided into pages of the same size
- Pages are mapped to frames using a page table
- Eliminates external fragmentation

**Example**: Page Table Structure

```
Page Table for Process P1:
---------------------
| Page # | Frame # |
---------------------
|    0   |    3    |
|    1   |    7    |
|    2   |    2    |
|    3   |    5    |
---------------------

Logical Address 1KB in a system with 1KB page size:
Page Number = 1KB / 1KB = 1
Offset = 1KB % 1KB = 0

Physical Address = (Frame # × Frame Size) + Offset
                 = (7 × 1KB) + 0 = 7KB
```

#### 3.2.4 Segmentation

- Memory is divided according to the logical structure of the program
- Each segment has a name and length
- Allows for better protection and sharing

#### 3.2.5 Virtual Memory

- Allows programs to use more memory than physically available
- Uses disk as an extension of RAM
- Implements demand paging and page replacement algorithms

### 3.3 Page Replacement Algorithms

These algorithms decide which page to remove when a new page needs to be loaded into memory.

#### 3.3.1 First-In-First-Out (FIFO)

- Replaces the oldest page in memory
- Simple but may remove frequently used pages

**Example**: FIFO Page Replacement

```
Memory capacity: 3 frames
Reference string: 1, 2, 3, 4, 1, 2, 5, 1, 2, 3, 4, 5

Frame 1: | 1 | 1 | 1 | 4 | 4 | 4 | 5 | 5 | 5 | 3 | 3 | 3 |
Frame 2: | - | 2 | 2 | 2 | 2 | 2 | 2 | 1 | 1 | 1 | 4 | 4 |
Frame 3: | - | - | 3 | 3 | 3 | 3 | 3 | 3 | 2 | 2 | 2 | 5 |
         
Page faults: 8 (initial 3 loads + 5 replacements)
```

#### 3.3.2 Least Recently Used (LRU)

- Replaces the page that hasn't been used for the longest time
- Better performance but more complex to implement

#### 3.3.3 Optimal Page Replacement

- Replaces the page that won't be used for the longest time in the future
- Theoretical algorithm (requires future knowledge)
- Used as a benchmark

### 3.4 Thrashing

Thrashing occurs when a system spends more time swapping pages than executing processes.

Causes:

- Too many processes in memory
- Insufficient physical memory
- Poor page replacement algorithm

Solutions:

- Local allocation policies
- Working set model
- Page fault frequency control

## 4. Processor Scheduling Algorithms

Processor scheduling determines which process runs on the CPU at any given time, aiming to maximize CPU utilization and throughput while minimizing response time and waiting time.

### 4.1 Scheduling Criteria

- **CPU Utilization**: Keep the CPU as busy as possible
- **Throughput**: Number of processes completed per unit time
- **Turnaround Time**: Time from submission to completion
- **Waiting Time**: Total time spent waiting in the ready queue
- **Response Time**: Time from submission to first response

### 4.2 Types of Scheduling

#### 4.2.1 Preemptive vs. Non-preemptive

- **Preemptive**: Running process can be interrupted and moved to ready queue
- **Non-preemptive**: Once a process starts, it runs until completion or blocking

#### 4.2.2 Long-term vs. Short-term vs. Medium-term

- **Long-term (Job Scheduler)**: Controls degree of multiprogramming
- **Short-term (CPU Scheduler)**: Selects which process to execute next
- **Medium-term**: Handles swapping processes between main memory and disk

### 4.3 Scheduling Algorithms

#### 4.3.1 First-Come, First-Served (FCFS)

- Simplest scheduling algorithm
- Non-preemptive
- Processes are executed in the order they arrive
- Can lead to the "convoy effect"

**Example**: FCFS Scheduling

```
Process | Arrival Time | Burst Time
--------|--------------|----------
   P1   |      0       |     24
   P2   |      3       |      3
   P3   |      5       |      3

Timeline:
0                 24   27   30
|-----P1----------|P2--|P3--|

Waiting time:
P1: 0
P2: 24 - 3 = 21
P3: 27 - 5 = 22

Average waiting time: (0 + 21 + 22) / 3 = 14.33
```

#### 4.3.2 Shortest Job First (SJF)

- Assigns CPU to process with smallest execution time
- Can be preemptive (SRTF) or non-preemptive
- Optimal for minimizing average waiting time
- Difficult to implement (requires knowing execution time in advance)

**Example**: SJF (Non-preemptive)

```
Process | Arrival Time | Burst Time
--------|--------------|----------
   P1   |      0       |     24
   P2   |      3       |      3
   P3   |      5       |      3

Timeline:
0    3     6            30
|-P1-|P2--|P3--|--P1----|

Waiting time:
P1: 0 + (6 - 3) = 3 (waiting after being preempted)
P2: 3 - 3 = 0
P3: 6 - 5 = 1

Average waiting time: (3 + 0 + 1) / 3 = 1.33
```

#### 4.3.3 Priority Scheduling

- Each process is assigned a priority
- Higher priority processes are executed first
- Can lead to starvation of low-priority processes
- Aging can be implemented to prevent starvation

#### 4.3.4 Round-Robin (RR)

- Preemptive scheduling algorithm
- Each process gets a small unit of CPU time (time quantum)
- After time quantum expires, process is preempted and added to the end of the ready queue
- Performance depends heavily on the size of the time quantum

**Example**: Round-Robin (Time Quantum = 4)

```
Process | Arrival Time | Burst Time
--------|--------------|----------
   P1   |      0       |     10
   P2   |      1       |      6
   P3   |      2       |      3

Timeline:
0    4    8   11   15   19
|P1--|P2--|P3--|P1--|P2--|

Waiting time:
P1: 0 + (11 - 8) = 3
P2: (4 - 1) + (15 - 11) = 7
P3: 8 - 2 = 6

Average waiting time: (3 + 7 + 6) / 3 = 5.33
```

#### 4.3.5 Multilevel Queue Scheduling

- Ready queue is divided into separate queues for different process types
- Each queue has its own scheduling algorithm
- Scheduling must be done between queues

#### 4.3.6 Multilevel Feedback Queue Scheduling

- Similar to multilevel queue but allows processes to move between queues
- Processes are separated based on CPU burst characteristics
- If a process uses too much CPU time, it's moved to a lower-priority queue

## 5. Process Synchronization

Process synchronization deals with the coordination of multiple processes to ensure they access shared resources correctly and efficiently.

### 5.1 The Critical Section Problem

A critical section is a segment of code where shared resources are accessed. A solution must satisfy:

1. **Mutual Exclusion**: Only one process can execute in the critical section at a time
2. **Progress**: If no process is in the critical section, a process that wants to enter should be able to
3. **Bounded Waiting**: There is a limit on how many times other processes can enter before a waiting process is allowed

### 5.2 Synchronization Mechanisms

#### 5.2.1 Mutex Locks

- Simplest synchronization tool
- Provides mutual exclusion
- Has two operations: acquire() and release()

**Example**: Mutex in C (POSIX threads)

```c
#include <pthread.h>
#include <stdio.h>

pthread_mutex_t mutex = PTHREAD_MUTEX_INITIALIZER;
int counter = 0;

void* increment_counter(void* arg) {
    for (int i = 0; i < 10000; i++) {
        pthread_mutex_lock(&mutex);  // Enter critical section
        counter++;                   // Access shared resource
        pthread_mutex_unlock(&mutex); // Exit critical section
    }
    return NULL;
}

int main() {
    pthread_t t1, t2;
    pthread_create(&t1, NULL, increment_counter, NULL);
    pthread_create(&t2, NULL, increment_counter, NULL);
    
    pthread_join(t1, NULL);
    pthread_join(t2, NULL);
    
    printf("Final counter value: %d\n", counter); // Should be 20000
    return 0;
}
```

#### 6.2.4 Remote Procedure Calls (RPC)

- Allows a program to cause a procedure to execute in another address space
- Makes distributed computing more transparent
- Example implementations: gRPC, Java RMI, CORBA

#### 6.2.5 Memory-Mapped Files

- Maps a file into the address space of a process
- Allows file I/O to be treated as memory operations
- Efficient for large files

## 7. Disk Structure and Scheduling

Disk management involves organizing data on disk and scheduling disk access operations to maximize efficiency.

### 7.1 Disk Structure

#### 7.1.1 Physical Disk Structure

- **Platters**: Circular disk surfaces that store data magnetically
- **Tracks**: Concentric circles on a platter
- **Sectors**: Smallest addressable unit on a disk, typically 512 bytes
- **Cylinders**: Same track on all platters
- **Clusters/Blocks**: Multiple sectors grouped together (filesystem's allocation unit)

#### 7.1.2 Logical Disk Structure

- **Partitions**: Division of physical disk into logical sections
- **Volumes**: Logical storage unit that can span multiple partitions or disks
- **File Systems**: Method for organizing files on a disk volume

### 7.2 Disk Scheduling

Disk scheduling algorithms aim to minimize seek time, rotational latency, and transfer time.

#### 7.2.1 Performance Metrics

- **Seek Time**: Time to position the read/write head over the correct track
- **Rotational Latency**: Time for the desired sector to rotate under the head
- **Transfer Time**: Time to transfer data
- **Access Time**: Seek Time + Rotational Latency + Transfer Time
- **Disk Throughput**: Total amount of data transferred per unit time

#### 7.2.2 First-Come, First-Served (FCFS)

- Services requests in the order they arrive
- Fair but can lead to excessive head movement

**Example**: FCFS Disk Scheduling

```
Disk head starting position: 50
Request queue: 98, 183, 37, 122, 14, 124, 65, 67

Head movement sequence: 50 → 98 → 183 → 37 → 122 → 14 → 124 → 65 → 67
Total head movement: |50-98| + |98-183| + |183-37| + |37-122| + |122-14| + |14-124| + |124-65| + |65-67| = 640 cylinders
```

#### 7.2.3 Shortest Seek Time First (SSTF)

- Selects the request with minimum seek time from current position
- Can cause starvation of some requests

**Example**: SSTF Disk Scheduling

```
Disk head starting position: 50
Request queue: 98, 183, 37, 122, 14, 124, 65, 67

Head movement sequence: 50 → 65 → 67 → 37 → 14 → 98 → 122 → 124 → 183
Total head movement: |50-65| + |65-67| + |67-37| + |37-14| + |14-98| + |98-122| + |122-124| + |124-183| = 236 cylinders
```

#### 7.2.4 SCAN (Elevator) Algorithm

- Disk arm starts at one end and moves toward the other end
- Services requests along the way
- When it reaches the other end, it reverses direction
- Provides more uniform service than SSTF

**Example**: SCAN Disk Scheduling

```
Disk head starting position: 50 (moving toward higher cylinder numbers)
Request queue: 98, 183, 37, 122, 14, 124, 65, 67

Head movement sequence: 50 → 65 → 67 → 98 → 122 → 124 → 183 → (end at 199) → (change direction) → 37 → 14
Total head movement: |50-65| + |65-67| + |67-98| + |98-122| + |122-124| + |124-183| + |183-199| + |199-37| + |37-14| = 398 cylinders
```

#### 7.2.5 C-SCAN (Circular SCAN)

- Variation of SCAN
- Head moves from one end to the other, servicing requests
- When it reaches the end, it immediately returns to the beginning without servicing requests
- Provides more uniform wait time than SCAN

**Example**: C-SCAN Disk Scheduling

```
Disk head starting position: 50 (moving toward higher cylinder numbers)
Request queue: 98, 183, 37, 122, 14, 124, 65, 67
Disk range: 0-199

Head movement sequence: 50 → 65 → 67 → 98 → 122 → 124 → 183 → (end at 199, return to 0) → 14 → 37
Total head movement: |50-65| + |65-67| + |67-98| + |98-122| + |122-124| + |124-183| + |183-199| + (199-0) + |0-14| + |14-37| = 371 cylinders
```

#### 7.2.6 LOOK and C-LOOK

- Variations of SCAN and C-SCAN
- Head only goes as far as the last request in each direction

### 7.3 RAID (Redundant Array of Independent Disks)

RAID combines multiple disk drives into a logical unit for data redundancy or performance improvement.

#### 7.3.1 RAID Levels

- **RAID 0 (Striping)**: Data split across drives; improves performance but no redundancy
- **RAID 1 (Mirroring)**: Exact copy of data on two drives; provides redundancy
- **RAID 5**: Distributed parity across drives; good balance of performance and redundancy
- **RAID 6**: Dual parity; can survive two drive failures
- **RAID 10 (1+0)**: Combination of mirroring and striping

#### 7.3.2 RAID Trade-offs

- **Performance**: Different RAID levels offer different read/write performance
- **Redundancy**: Higher redundancy typically means more storage overhead
- **Cost**: Higher redundancy and performance usually mean higher cost
- **Complexity**: More complex RAID levels require more sophisticated controllers

### 7.4 File System Implementation

#### 7.4.1 Allocation Methods

- **Contiguous Allocation**: Files occupy contiguous blocks
    
    - Advantages: Simple, excellent read performance
    - Disadvantages: External fragmentation, difficult to expand files
- **Linked Allocation**: Each block contains a pointer to the next block
    
    - Advantages: No external fragmentation, files can grow easily
    - Disadvantages: Random access is slow, pointers take space, reliability issues
- **Indexed Allocation**: Index block contains pointers to all file blocks
    
    - Advantages: Supports direct access, no external fragmentation
    - Disadvantages: Overhead of index blocks, potential for wasted space

**Example**: File Allocation Table (FAT)

```
FAT Entry Structure:
-----------------
| Entry | Value |
-----------------
|   0   |  EOF  | <- File A starts here
|   1   |   2   |
|   2   |   4   |
|   3   |  EOF  | <- File B starts here
|   4   |  EOF  |
|   5   |  Free |
|   6   |  Free |
|   7   |  Bad  | <- Bad sector
-----------------

Directory Entry for File A:
Name: FileA, First Block: 0, Size: 3 blocks
Directory Entry for File B:
Name: FileB, First Block: 3, Size: 1 block
```

#### 7.4.2 Free Space Management

- **Bit Vector/Bitmap**: 1 bit for each block (0=free, 1=allocated)
- **Linked List**: Link all free blocks together
- **Grouping**: First free block contains addresses of next n free blocks
- **Counting**: Keep track of contiguous free blocks

#### 7.4.3 Directory Implementation

- **Linear List**: Simple array of file names and attributes
- **Hash Table**: Hash function determines where to place file entries
- **B-Tree**: Balanced tree structure for faster searching

### 7.5 I/O Systems

#### 7.5.1 I/O Hardware

- **I/O Devices**: Range from simple (mouse) to complex (disk)
- **Device Controllers**: Electronic components that operate port, bus, device
- **I/O Ports**: Connection points for devices
- **Buses**: Communication pathways between components

#### 7.5.2 I/O Software

- **Interrupt Handlers**: Process device interrupts
- **Device Drivers**: Interface between OS and specific hardware
- **Device-Independent I/O Software**: Provides uniform interface for device drivers
- **User-Space I/O Software**: Libraries and applications that use I/O

#### 7.5.3 I/O Methods

- **Programmed I/O**: CPU actively waits for I/O operations
- **Interrupt-Driven I/O**: CPU continues with other work until interrupted by I/O completion
- **DMA (Direct Memory Access)**: Special hardware transfers data without CPU intervention

## Conclusion

Operating systems are complex software systems that manage computer hardware resources and provide services to users and applications. Understanding the fundamental concepts of operating systems, from process management to disk scheduling, is essential for developing efficient and reliable software systems.

The field of operating systems continues to evolve with new architectures, technologies, and requirements. The principles discussed in these notes, however, remain relevant as they form the foundation upon which modern operating systems are built.

````

#### 5.2.2 Semaphores

- More sophisticated synchronization tool
- Can be used for both mutual exclusion and resource allocation
- Two operations: wait() and signal()
- Can be binary (0 or 1) or counting (can take any non-negative value)

**Example**: Producer-Consumer with Semaphores
```c
#include <pthread.h>
#include <semaphore.h>
#include <stdio.h>
#define BUFFER_SIZE 5

int buffer[BUFFER_SIZE];
int in = 0, out = 0;

sem_t empty, full, mutex;

void* producer(void* arg) {
    int item = 0;
    while (1) {
        item++;
        
        sem_wait(&empty);   // Wait for an empty slot
        sem_wait(&mutex);   // Enter critical section
        
        // Add item to buffer
        buffer[in] = item;
        in = (in + 1) % BUFFER_SIZE;
        printf("Produced: %d\n", item);
        
        sem_post(&mutex);   // Exit critical section
        sem_post(&full);    // Signal that buffer has an item
    }
}

void* consumer(void* arg) {
    while (1) {
        sem_wait(&full);    // Wait for a full slot
        sem_wait(&mutex);   // Enter critical section
        
        // Remove item from buffer
        int item = buffer[out];
        out = (out + 1) % BUFFER_SIZE;
        printf("Consumed: %d\n", item);
        
        sem_post(&mutex);   // Exit critical section
        sem_post(&empty);   // Signal that buffer has an empty slot
    }
}

int main() {
    // Initialize semaphores
    sem_init(&empty, 0, BUFFER_SIZE);  // All slots are initially empty
    sem_init(&full, 0, 0);             // No slots are initially full
    sem_init(&mutex, 0, 1);            // Binary semaphore for mutual exclusion
    
    pthread_t prod, cons;
    pthread_create(&prod, NULL, producer, NULL);
    pthread_create(&cons, NULL, consumer, NULL);
    
    pthread_join(prod, NULL);
    pthread_join(cons, NULL);
    return 0;
}
````

#### 5.2.3 Monitors

- High-level synchronization construct
- Contains both shared variables and procedures
- Only one process can be active in a monitor at a time
- Used in many programming languages (e.g., Java's synchronized methods)

**Example**: Monitor in Java

```java
public class BoundedBuffer {
    private final int[] buffer = new int[BUFFER_SIZE];
    private int count = 0, in = 0, out = 0;
    
    public synchronized void produce(int item) throws InterruptedException {
        while (count == BUFFER_SIZE) {
            // Buffer is full, wait
            wait();
        }
        
        buffer[in] = item;
        in = (in + 1) % BUFFER_SIZE;
        count++;
        
        // Notify consumer that an item is available
        notify();
    }
    
    public synchronized int consume() throws InterruptedException {
        while (count == 0) {
            // Buffer is empty, wait
            wait();
        }
        
        int item = buffer[out];
        out = (out + 1) % BUFFER_SIZE;
        count--;
        
        // Notify producer that a slot is available
        notify();
        
        return item;
    }
}
```

### 5.3 Classic Synchronization Problems

#### 5.3.1 Producer-Consumer Problem

- Producers create data items and place them in a buffer
- Consumers remove data items from the buffer
- Need to ensure that producers don't add to a full buffer and consumers don't remove from an empty buffer

#### 5.3.2 Readers-Writers Problem

- Multiple processes want to read or write to a shared resource
- Multiple readers can access simultaneously
- Only one writer can access at a time (no other readers or writers)

#### 5.3.3 Dining Philosophers Problem

- Illustrates challenges in resource allocation and deadlock prevention
- Each philosopher needs two forks to eat, but there are only as many forks as philosophers

**Example**: Solution to Dining Philosophers

```c
#include <pthread.h>
#include <stdio.h>
#include <unistd.h>

#define NUM_PHILOSOPHERS 5
#define LEFT (philosopher_id + NUM_PHILOSOPHERS - 1) % NUM_PHILOSOPHERS
#define RIGHT (philosopher_id + 1) % NUM_PHILOSOPHERS

pthread_mutex_t forks[NUM_PHILOSOPHERS];
pthread_t philosophers[NUM_PHILOSOPHERS];

void* philosopher_process(void* arg) {
    int philosopher_id = *(int*)arg;
    
    while (1) {
        // Think
        printf("Philosopher %d is thinking\n", philosopher_id);
        sleep(1);
        
        // Pick up forks
        if (philosopher_id == NUM_PHILOSOPHERS - 1) {
            // Last philosopher picks up right fork first to avoid deadlock
            pthread_mutex_lock(&forks[RIGHT]);
            pthread_mutex_lock(&forks[philosopher_id]);
        } else {
            pthread_mutex_lock(&forks[philosopher_id]);
            pthread_mutex_lock(&forks[RIGHT]);
        }
        
        // Eat
        printf("Philosopher %d is eating\n", philosopher_id);
        sleep(1);
        
        // Put down forks
        pthread_mutex_unlock(&forks[philosopher_id]);
        pthread_mutex_unlock(&forks[RIGHT]);
    }
}

int main() {
    int philosopher_ids[NUM_PHILOSOPHERS];
    
    // Initialize mutexes (forks)
    for (int i = 0; i < NUM_PHILOSOPHERS; i++) {
        pthread_mutex_init(&forks[i], NULL);
    }
    
    // Create philosopher threads
    for (int i = 0; i < NUM_PHILOSOPHERS; i++) {
        philosopher_ids[i] = i;
        pthread_create(&philosophers[i], NULL, philosopher_process, &philosopher_ids[i]);
    }
    
    // Join threads (never reached in this example)
    for (int i = 0; i < NUM_PHILOSOPHERS; i++) {
        pthread_join(philosophers[i], NULL);
    }
    
    return 0;
}
```

### 5.4 Deadlocks

A deadlock occurs when two or more processes are waiting indefinitely for an event that can only be caused by one of the waiting processes.

#### 5.4.1 Deadlock Conditions

Four conditions must be present for a deadlock to occur:

1. **Mutual Exclusion**: At least one resource must be held in a non-sharable mode
2. **Hold and Wait**: Processes holding resources can request more resources
3. **No Preemption**: Resources cannot be forcibly taken from a process
4. **Circular Wait**: A circular chain of processes exists, each waiting for a resource held by the next process

#### 5.4.2 Deadlock Prevention

Breaking any of the four conditions will prevent deadlocks:

- **Mutual Exclusion**: Not possible to break for non-sharable resources
- **Hold and Wait**: Require processes to request all resources at once
- **No Preemption**: Allow preemption if a process holding resources is blocked
- **Circular Wait**: Impose a total ordering on resource types

#### 5.4.3 Deadlock Avoidance

- Requires knowledge of future resource requests
- Uses the Banker's Algorithm to determine if granting a request would lead to an unsafe state

#### 5.4.4 Deadlock Detection and Recovery

- Allow deadlocks to occur, but detect and recover from them
- Use resource allocation graphs to detect deadlocks
- Recovery methods include process termination or resource preemption

## 6. Inter-process Communication (IPC)

Inter-process communication refers to mechanisms that allow processes to communicate and synchronize their actions.

### 6.1 IPC Categories

#### 6.1.1 Shared Memory

- Processes share a region of memory
- Fast, as no kernel involvement is needed once memory is established
- Requires synchronization mechanisms

**Example**: Shared Memory in C (POSIX)

```c
#include <stdio.h>
#include <stdlib.h>
#include <sys/mman.h>
#include <sys/stat.h>
#include <fcntl.h>
#include <unistd.h>
#include <string.h>

#define SHM_NAME "/my_shared_memory"
#define SHM_SIZE 4096

int main() {
    // Create a new process
    pid_t pid = fork();
    
    if (pid < 0) {
        perror("fork");
        exit(1);
    }
    
    // Create shared memory object
    int shm_fd = shm_open(SHM_NAME, O_CREAT | O_RDWR, 0666);
    if (shm_fd == -1) {
        perror("shm_open");
        exit(1);
    }
    
    // Set the size of the shared memory object
    ftruncate(shm_fd, SHM_SIZE);
    
    // Map the shared memory object into the process address space
    void* shm_ptr = mmap(0, SHM_SIZE, PROT_READ | PROT_WRITE, MAP_SHARED, shm_fd, 0);
    if (shm_ptr == MAP_FAILED) {
        perror("mmap");
        exit(1);
    }
    
    if (pid == 0) {  // Child process
        sleep(1);  // Ensure parent writes first
        printf("Child reads: %s\n", (char*)shm_ptr);
        
        // Write to the shared memory
        sprintf((char*)shm_ptr + strlen((char*)shm_ptr), " - Child was here");
    } else {  // Parent process
        // Write to the shared memory
        sprintf((char*)shm_ptr, "Hello from parent");
        sleep(2);  // Give child time to read and write
        printf("Parent reads: %s\n", (char*)shm_ptr);
        
        // Clean up
        wait(NULL);  // Wait for child to finish
        munmap(shm_ptr, SHM_SIZE);
        shm_unlink(SHM_NAME);
    }
    
    return 0;
}
```

#### 6.1.2 Message Passing

- Processes communicate by exchanging messages
- Can be implemented with or without a message queue
- Kernel is involved in message transfer

**Example**: Message Queues in C (POSIX)

```c
#include <stdio.h>
#include <stdlib.h>
#include <string.h>
#include <fcntl.h>
#include <sys/stat.h>
#include <mqueue.h>
#include <unistd.h>

#define QUEUE_NAME "/my_message_queue"
#define MAX_MSG_SIZE 256
#define MSG_PRIORITY 1

int main() {
    mqd_t mq;
    struct mq_attr attr;
    char buffer[MAX_MSG_SIZE];
    
    // Initialize the message queue attributes
    attr.mq_flags = 0;
    attr.mq_maxmsg = 10;
    attr.mq_msgsize = MAX_MSG_SIZE;
    attr.mq_curmsgs = 0;
    
    // Create a new process
    pid_t pid = fork();
    
    if (pid < 0) {
        perror("fork");
        exit(1);
    }
    
    if (pid == 0) {  // Child process
        // Open the message queue for receiving
        mq = mq_open(QUEUE_NAME, O_RDONLY | O_CREAT, 0644, &attr);
        if (mq == (mqd_t)-1) {
            perror("mq_open");
            exit(1);
        }
        
        // Receive the message
        unsigned int prio;
        int bytes_read = mq_receive(mq, buffer, MAX_MSG_SIZE, &prio);
        if (bytes_read >= 0) {
            buffer[bytes_read] = '\0';
            printf("Child received message: %s\n", buffer);
        } else {
            perror("mq_receive");
        }
        
        // Clean up
        mq_close(mq);
    } else {  // Parent process
        // Open the message queue for sending
        mq = mq_open(QUEUE_NAME, O_WRONLY | O_CREAT, 0644, &attr);
        if (mq == (mqd_t)-1) {
            perror("mq_open");
            exit(1);
        }
        
        // Send a message
        char* msg = "Hello from parent process";
        if (mq_send(mq, msg, strlen(msg), MSG_PRIORITY) == -1) {
            perror("mq_send");
        } else {
            printf("Parent sent message\n");
        }
        
        // Wait for child and clean up
        wait(NULL);
        mq_close(mq);
        mq_unlink(QUEUE_NAME);
    }
    
    return 0;
}
```

### 6.2 IPC Methods

#### 6.2.1 Pipes

- Oldest form of IPC
- Unidirectional (half-duplex) communication
- Used between related processes (e.g., parent-child)

**Example**: Pipe in C

```c
#include <stdio.h>
#include <stdlib.h>
#include <unistd.h>
#include <string.h>

int main() {
    int pipefd[2];
    pid_t pid;
    char buffer[100];
    
    // Create a pipe
    if (pipe(pipefd) == -1) {
        perror("pipe");
        exit(EXIT_FAILURE);
    }
    
    // Create a child process
    pid = fork();
    if (pid == -1) {
        perror("fork");
        exit(EXIT_FAILURE);
    }
    
    if (pid == 0) {  // Child process
        // Close unused write end
        close(pipefd[1]);
        
        // Read from pipe
        int bytes_read = read(pipefd[0], buffer, sizeof(buffer));
        printf("Child received: %s\n", buffer);
        
        // Close read end
        close(pipefd[0]);
        exit(EXIT_SUCCESS);
    } else {  // Parent process
        // Close unused read end
        close(pipefd[0]);
        
        // Write to pipe
        char* message = "Hello from parent!";
        write(pipefd[1], message, strlen(message) + 1);
        printf("Parent sent: %s\n", message);
        
        // Close write end
        close(pipefd[1]);
        wait(NULL);  // Wait for child to finish
        exit(EXIT_SUCCESS);
    }
    
    return 0;
}
```

#### 6.2.2 Named Pipes (FIFOs)

- Allow communication between unrelated processes
- Represented as files in the file system
- Persist until explicitly deleted

#### 6.2.3 Sockets

- Allow communication between processes on different machines
- Support both connection-oriented (TCP) and connectionless (UDP) communication
- Used extensively in networked applications

**Example**: Socket Communication (Server and Client)

```c
// Server code (server.c)
#include <stdio.h>
#include <stdlib.h>
#include <string.h>
#include <unistd.h>
#include <arpa/inet.h>

#define PORT 8080

int main() {
    int server_fd, new_socket;
    struct sockaddr_in address;
    int addrlen = sizeof(address);
    char buffer[1024] = {0};
    char* response = "Hello from server";
    
    // Create socket
    if ((server_fd = socket(AF_INET, SOCK_STREAM, 0)) == 0) {
        perror("socket failed");
        exit(EXIT_FAILURE);
    }
    
    // Set socket options
    int opt = 1;
    if (setsockopt(server_fd, SOL_SOCKET, SO_REUSEADDR, &opt, sizeof(opt))) {
        perror("setsockopt");
        exit(EXIT_FAILURE);
    }
    
    // Set up address structure
    address.sin_family = AF_INET;
    address.sin_addr.s_addr = INADDR_ANY;
    address.sin_port = htons(PORT);
    
    // Bind socket to port
    if (bind(server_fd, (struct sockaddr *)&address, sizeof(address)) < 0) {
        perror("bind failed");
        exit(EXIT_FAILURE);
    }
    
    // Listen for connections
    if (listen(server_fd, 3) < 0) {
        perror("listen");
        exit(EXIT_FAILURE);
    }
    
    printf("Server listening on port %d...\n", PORT);
    
    // Accept connection
    if ((new_socket = accept(server_fd, (struct sockaddr *)&address, (socklen_t*)&addrlen)) < 0) {
        perror("accept");
        exit(EXIT_FAILURE);
    }
    
    // Receive message from client
    int valread = read(new_socket, buffer, 1024);
    printf("Received from client: %s\n", buffer);
    
    // Send response to client
    send(new_socket, response, strlen(response), 0);
    printf("Response sent\n");
    
    close(new_socket);
    close(server_fd);
    return 0;
}

// Client code (client.c)
#include <stdio.h>
#include <stdlib.h>
#include <string.h>
#include <unistd.h>
#include <arpa/inet.h>

#define PORT 8080

int main() {
    int sock = 0;
    struct sockaddr_in serv_addr;
    char buffer[1024] = {0};
    char* message = "Hello from client";
    
    // Create socket
    if ((sock = socket(AF_INET, SOCK_STREAM, 0)) < 0) {
        perror("socket creation error");
        return -1;
    }
    
    // Set up address structure
    serv_addr.sin_family = AF_INET;
    serv_addr.sin_port = htons(PORT);
    
    // Convert IPv4 address from text to binary form
    if (inet_pton(AF_INET, "127.0.0.1", &serv_addr.sin_addr) <= 0) {
        perror("invalid address");
        return -1;
    }
    
    // Connect to server
    if (connect(sock, (struct sockaddr *)&serv_addr, sizeof(serv_addr)) < 0) {
        perror("connection failed");
        return -1;
    }
    
    // Send message to server
    send(sock, message, strlen(message), 0);
    printf("Message sent\n");
    
    // Receive response from server
    int valread = read(sock, buffer, 1024);
    printf("Server response: %s\n", buffer);
    
    close(sock);
    return 0;
}
```