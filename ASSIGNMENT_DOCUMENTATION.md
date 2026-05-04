# Assignment 3 - Complete Documentation

**Student Name**: [Rana Eid Alotaibi]  
**Student ID**: [445052109]  
**Date Submitted**: [4 may,2026]

---

## 🎥 VIDEO DEMONSTRATION LINK (REQUIRED)

> **⚠️ IMPORTANT: This section is REQUIRED for grading!**
> 
> Upload your 3-5 minute video to your **PERSONAL Gmail Google Drive** (NOT university email).
> Set sharing to "Anyone with the link can view".
> Test the link in incognito/private mode before submitting.

**Video Link**: [Paste your personal Gmail Google Drive link here]

**Video filename**: `[YourStudentID]_Assignment3_Synchronization.mp4`

**Verification**:
- [ ] Link is accessible (tested in incognito mode)
- [ ] Video is 3-5 minutes long
- [ ] Video shows code walkthrough and commits
- [ ] Video has clear audio
- [ ] Uploaded to PERSONAL Gmail (not @std.psau.edu.sa)

---

## Part 1: Development Log (1 mark)

Document your development process with **minimum 3 entries** showing progression:

### Entry 1 - [Apr 29, 2026,3:00PM]
**What I implemented**:
Set my student ID in the code as the first required step.
**Challenges encountered**:
No major issues, but I made sure the ID is correct because it affects random values in the simulation.
**How I solved it**:
Carefully updated the studentID variable and verified it in the output.
**Testing approach**:
Ran the program once to confirm that the student ID appears correctly.
**Time spent**: 30 minutes

---

### Entry 2 - [Apr 30, 2026, 4:00PM]
**What I implemented**: 
Implemented Task 1 by adding ReentrantLock to protect shared counters.
**Challenges encountered**: 
Understanding where exactly to place lock() and unlock() and avoiding missing unlock.
**How I solved it**: 
Used try-finally blocks to guarantee unlocking even if an error occurs.
**Testing approach**: 
Ran the program multiple times and checked if counter values remain stable.
**Time spent**: 
2.5 hours
---

### Entry 3 - [Apr 30, 2026, 6:30PM]
**What I implemented**: 
mplemented Task 2 by adding a separate lock for executionLog.
**Challenges encountered**: 
Realizing that ArrayList is not thread-safe and may cause issues with concurrent access.
**How I solved it**: 
Added logLock and wrapped all log operations inside lock/unlock.
**Testing approach**: 
Ran the program multiple times to ensure no exceptions occur.
**Time spent**: 
2 hours
---

### Entry 4 - [May 2, 2026, 5:00PM]
**What I implemented**: 
Implemented Task 3 using Semaphore to control CPU access.
**Challenges encountered**: 
Determining where to place acquire() and release() properly.
**How I solved it**: 
Placed acquire() at the start of run() and release() in finally block.
**Testing approach**: 
Observed that processes execute one at a time.
**Time spent**: 
 1.5 hours
---

### Entry 5 - [May 3, 2026, 3:00PM]
**What I implemented**: 
Completed Development documentation and answered all assignment questions.
**Challenges encountered**: 
Making sure answers are clear, complete, and reflect my implementation.
**How I solved it**: 
Reviewed my code and linked each answer to actual implementation.
**Testing approach**: 
Final run of the program multiple times to ensure correctness before submission.
**Time spent**: 
3 hours
---

## Part 2: Technical Questions (1 mark)

### Question 1: Race Conditions
**Q**: Identify and explain TWO race conditions in the original code. For each:
- What shared resource is affected?
- Why is concurrent access a problem?
- What incorrect behavior could occur?

**Your Answer**:

When several threads access shared data simultaneously without synchronization, a race condition arises.When multiple threads execute contextSwitchCount++; simultaneously, the first race situation emerges in the shared counter contextSwitchCount, potentially leading to lost updates and inaccurate values.When threads perform executionLog.add(message); simultaneously, the second race condition arises in executionLog.This can result in incorrect data or runtime problems like ConcurrentModificationException since ArrayList is not thread-safe.Because threads may overwrite one another and operations are not atomic, concurrent access is problematic.  
I utilized ReentrantLock to make sure that only one thread could access these resources at a time in order to fix this problem.
---

### Question 2: Locks vs Semaphores
**Q**: Explain the difference between ReentrantLock and Semaphore. Where did you use each in your code and why?

**Your Answer**:

ReentrantLock is used for mutual exclusion, ensuring only one thread accesses shared data at a time.
Semaphore controls how many threads can access a resource simultaneously.
In my code, I used ReentrantLock to protect shared counters and logs, and Semaphore to allow only one process to use the CPU.
Locks protect data, while semaphore controls execution flow.
---

### Question 3: Deadlock Prevention
**Q**: What is deadlock? Explain TWO prevention techniques and what you did to prevent deadlocks in your code.

**Your Answer**:
Deadlock happens when threads wait indefinitely for locks held by each other.
One preventative strategy is utilizing try-finally to ensure locks are always released.
Another option is avoiding nested locks or preserving constant lock order.
In my code, I utilized try-finally blocks for every lock to ensure proper release and prevent deadlocks


---

### Question 4: Lock Granularity Design Decision 
**Q**: For Task 1 (protecting the three counters), explain your lock design choice:
- Did you use ONE lock for all three counters (coarse-grained) OR separate locks for each counter (fine-grained)?
- Explain WHY you made this choice
- What are the trade-offs between the two approaches?
- Given that the three counters are independent, which approach provides better concurrency and why?

**Your Answer**:
I used one lock (coarse-grained) for all three counters.
This simplifies implementation and reduces complexity.
However, it reduces concurrency because threads must wait even if accessing different counters.
Fine-grained locking allows better concurrency but increases complexity.
Since counters are independent, fine-grained locking could improve performance.
However, for simplicity and correctness, I chose a single lock.


---

## Part 3: Synchronization Analysis (1 mark)

### Critical Section #1: Counter Variables

**Which variables**: 
contextSwitchCount, completedProcessCount, totalWaitingTime
**Why they need protection**: 
They are shared across threads and updated concurrently.
**Synchronization mechanism used**: 
ReentrantLock
**Code snippet**:
```java
counterLock.lock();
try {
    contextSwitchCount++;
} finally {
    counterLock.unlock();
}
```

**Justification**: 
Ensures atomic updates and prevents incorrect values
---

### Critical Section #2: Execution Log

**What resource**: 
executionLog (ArrayList)
**Why it needs protection**: 
ArrayList is not thread-safe.
**Synchronization mechanism used**: 
ReentrantLock
**Code snippet**:
```java
logLock.lock();
try {
    executionLog.add(message);
} finally {
    logLock.unlock();
}
```

**Justification**: 
Prevents concurrent modification errors.
---

### Critical Section #3: CPU Semaphore

**Purpose of semaphore**: 
Control CPU access
**Number of permits and why**: 
only one process at a time
**Where implemented**: 
1 permit is used to allow only one process to access the CPU at a time.
This reflects real CPU behavior and prevents multiple threads from running simultaneously.
It ensures correct scheduling and avoids conflicts between processes.
**Code snippet**:
```java
SharedResources.cpuSemaphore.acquire();
try {
    // execution
} finally {
    SharedResources.cpuSemaphore.release();
}
```

**Effect on program behavior**: 
Ensures sequential execution and avoids conflicts.
---

## Part 4: Testing and Verification (2 marks)

### Test 1: Consistency Check
**What I tested**: Running program multiple times to verify consistent results

**Testing procedure**: 
```bash
javac SchedulerSimulationSync.java
java SchedulerSimulationSync
java SchedulerSimulationSync
java SchedulerSimulationSync
java SchedulerSimulationSync
java SchedulerSimulationSync
```

**Results**:
 Total Context Switches: 30
Total Completed Processes: 16
Total Waiting Time: 938136ms
Average Waiting Time: 58633ms

═══ Process Summary Table ═══
Process    Priority     Burst Time   Waiting Time
────────────────────────────────────────────────
P1         3            2445         36          
P2         4            9244         77561       
P3         4            6403         59630       
P4         2            6230         62045       
P5         5            2785         14613       
P6         4            6850         64300       
P7         3            6738         67159       
P8         5            2177         25498       
P9         3            4517         69902       
P10        2            5426         70425       
P11        5            6485         71857       
P12        3            6644         74344       
P13        5            3476         43894       
P14        1            4020         76992       
P15        4            9812         78810       
P16        1            4485         81070       

═══ Execution Log Summary ═══
Total log entries: 60


**Why synchronization is necessary**: 
In the absence of synchronization, shared variables like contextSwitchCount, completedProcessCount, and totalWaitingTime may experience race situations. For instance, if many threads increase contextSwitchCount at the same time, the updates may be missed, leading to inaccurate values. Concurrent access to executionLog (ArrayList) may also result in runtime exceptions or corrupted data.
Data integrity is maintained by synchronization, which guarantees mutual exclusion, allowing only one thread to change shared resources at a time.

**Conclusion**: 
The consistent outcomes from several runs attest to the proper implementation of the synchronization mechanisms ReentrantLock and Semaphore, which successfully removed race situations.
---

### Test 2: Exception Testing
**What I tested**: Checking for ConcurrentModificationException

**Testing procedure**: 
Ran program repeatedly with multiple threads.
**Results**: 
No ConcurrentModificationException occurred.
**What this proves**: 
executionLog is properly synchronized.
---

### Test 3: Correctness Verification
**What I tested**: Verifying correct final values (total burst time, context switches, etc.)

**Expected values**: 
All processes complete, counters accurate
**Actual values**: 
Matched expected
**Analysis**: 
Synchronization works correctly
---

### Test 4: Different Scenarios
**Scenario tested**: [e.g., different time quantum, more processes, etc.]
Different random process counts
**Purpose**: 
Check robustness
**Results**: 
Program worked correctly
**What I learned**: 
Synchronization ensures stability under different conditions
---

## Part 5: Reflection and Learning

### What I learned about synchronization:

I learned how race conditions affect multithreaded programs.
I understood the importance of synchronization mechanisms like locks and semaphores.
I learned how to protect shared resources and avoid concurrency issues.
I saw how synchronization keeps the output organized and prevents data from getting mixed up.
I also learned how improper synchronization can lead to unpredictable behavior.
I learned how to check if my code is running correctly by comparing the actual results with what I expected.
---

### Real-world applications:

Give TWO examples where synchronization is critical:

**Example 1**: Banking systems (updating account balance)

**Example 2**:  Database systems (handling concurrent transactions)

---

### How I would explain synchronization to others:

Synchronization is like allowing only one person to use a shared resource at a time to avoid conflicts.

---

## Part 6: GitHub Repository Information

**Repository URL**: https://github.com/Ranaeidalo/OS-Assignment3-Rana-Alotaibi.git

**Number of commits**: 4

**Commit messages**: 
1. Set my student ID: 445052109
2. task1:protect shared counters
3. task2:protect executingLog(ArrayList)
4. Task 3: Control CPU using Semaphore

---

## Summary

**Total time spent on assignment**: 
9.5 hours
**Key takeaways**: 
1. Importance of synchronization
2. Preventing race conditions
3. Using locks and semaphores correctly

**Most challenging aspect**: 
Understanding race conditions
**What I'm most proud of**: 
Successfully implementing synchronization without errors
---

**End of Documentation**
