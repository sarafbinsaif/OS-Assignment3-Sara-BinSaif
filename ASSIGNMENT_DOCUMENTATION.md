# Assignment 3 - Complete Documentation

**Student Name**: [Sara Faisal BinSaif]  
**Student ID**: [445052371]  
**Date Submitted**: [4 May 2026]

---

## 🎥 VIDEO DEMONSTRATION LINK (REQUIRED)

> **⚠️ IMPORTANT: This section is REQUIRED for grading!**
> 
> Upload your 3-5 minute video to your **PERSONAL Gmail Google Drive** (NOT university email).
> Set sharing to "Anyone with the link can view".
> Test the link in incognito/private mode before submitting.

**Video Link**: [(https://drive.google.com/file/d/1yEKEV23xR3ysPhT3O1L9FCjlF4XOZeMu/view?usp=drivesdk)]

**Video filename**: `445052371_Assignment3_Synchronization.mp4`

**Verification**:
- [ ] Link is accessible (tested in incognito mode)
- [ ] Video is 3-5 minutes long
- [ ] Video shows code walkthrough and commits
- [ ] Video has clear audio
- [ ] Uploaded to PERSONAL Gmail (not @std.psau.edu.sa)

---

## Part 1: Development Log (1 mark)

Document your development process with **minimum 3 entries** showing progression:

### Entry 1 - [3 May, 10:00 PM]
**What I implemented**: 

I added a separate ReentrantLock to protect the executionLog ArrayList. This ensures that multiple threads do not modify the log simultaneously, preventing concurrent modification issues.


**Challenges encountered**: 

I encountered issues with improper synchronization where log entries were sometimes missing or out of order.


**How I solved it**: 

I introduced a dedicated lock (logLock) specifically for executionLog and ensured that every log insertion was protected inside a critical section.


**Testing approach**: 

I verified that all log messages appeared correctly and no ConcurrentModificationException occurred during execution.


**Time spent**: 

1 hour

---

### Entry 3 - [3 May, 10:30 PM]
**What I implemented**:  
I added a separate ReentrantLock to protect the executionLog ArrayList. This ensures that multiple threads do not modify the log simultaneously, preventing concurrent modification issues.

**Challenges encountered**:  
I encountered issues with improper synchronization where log entries were sometimes missing or out of order.

**How I solved it**:  
I introduced a dedicated lock (logLock) specifically for executionLog and ensured that every log insertion was protected inside a critical section.

**Testing approach**:  
I verified that all log messages appeared correctly and no ConcurrentModificationException occurred during execution.

**Time spent**:  
45 min
---

### Entry 3 - [3 May, 11:00 PM]
**What I implemented**:  
I implemented a Semaphore to control CPU access, ensuring that only one process (thread) can execute at a time. I added acquire() before execution and release() after execution.

**Challenges encountered**:  
I initially forgot to release the semaphore in all execution paths, which caused threads to block indefinitely.

**How I solved it**:  
I used try-finally blocks to guarantee that the semaphore is always released, even if an exception occurs.

**Testing approach**:  
I executed the program multiple times and confirmed that processes executed one at a time and no deadlocks occurred.

**Time spent**:  
1 hour

---

### Entry 4 - [Date, Time]
**What I implemented**: 

**Challenges encountered**: 

**How I solved it**: 

**Testing approach**: 

**Time spent**: 

---

### Entry 5 - [Date, Time]
**What I implemented**: 

**Challenges encountered**: 

**How I solved it**: 

**Testing approach**: 

**Time spent**: 

---

## Part 2: Technical Questions (1 mark)

### Question 1: Race Conditions
**Q**: Identify and explain TWO race conditions in the original code. For each:
- What shared resource is affected?
- Why is concurrent access a problem?
- What incorrect behavior could occur?

**Your Answer**:

Race conditions occur when multiple threads access shared resources simultaneously without proper synchronization. 

The first race condition exists in updating shared counters such as contextSwitchCount and completedProcessCount. These variables are accessed and modified by multiple threads, and concurrent increments like contextSwitchCount++ are not atomic operations. This can lead to lost updates where multiple threads overwrite each other's changes, resulting in incorrect counts.

The second race condition occurs in the executionLog ArrayList. Since multiple threads attempt to add log entries at the same time using executionLog.add(message), concurrent modification can lead to inconsistent logs or even runtime exceptions such as ConcurrentModificationException.

Without synchronization, these shared resources can produce unpredictable and incorrect behavior, affecting the accuracy of the simulation.

---

### Question 2: Locks vs Semaphores
**Q**: Explain the difference between ReentrantLock and Semaphore. Where did you use each in your code and why?

**Your Answer**:

ReentrantLock and Semaphore are both synchronization mechanisms but serve different purposes.

ReentrantLock is used to provide mutual exclusion, ensuring that only one thread can access a critical section at a time. In this assignment, ReentrantLock was used to protect shared resources such as counters and the execution log, preventing race conditions during updates.

On the other hand, Semaphore is used to control access to a limited number of resources. A binary semaphore (with one permit) was used to simulate CPU access, allowing only one process (thread) to execute at a time.

The key difference is that locks protect data integrity, while semaphores manage resource allocation and control concurrency levels.

---

### Question 3: Deadlock Prevention
**Q**: What is deadlock? Explain TWO prevention techniques and what you did to prevent deadlocks in your code.

**Your Answer**:

Deadlock is a situation where two or more threads are blocked indefinitely because each is waiting for a resource held by another thread.

One common prevention technique is using try-finally blocks to ensure that locks and semaphores are always released, even if an exception occurs. In this assignment, all lock.lock() and semaphore.acquire() calls were paired with unlock() and release() inside finally blocks to guarantee proper resource release.

Another technique is avoiding circular wait by maintaining a consistent order of acquiring locks. Although this assignment uses a limited number of locks, careful design ensures that locks are not acquired in conflicting orders.

These approaches help prevent deadlocks and ensure that the system remains responsive and stable during execution.

---

### Question 4: Lock Granularity Design Decision 
**Q**: For Task 1 (protecting the three counters), explain your lock design choice:
- Did you use ONE lock for all three counters (coarse-grained) OR separate locks for each counter (fine-grained)?
- Explain WHY you made this choice
- What are the trade-offs between the two approaches?
- Given that the three counters are independent, which approach provides better concurrency and why?

**Your Answer**:

In this assignment, a coarse-grained locking approach was used by applying a single ReentrantLock to protect all three counters (contextSwitchCount, completedProcessCount, and totalWaitingTime).

The main reason for this choice was simplicity and ease of implementation. Using one lock ensures that all updates to shared counters are synchronized consistently, reducing the risk of errors.

However, the trade-off is reduced concurrency, since only one thread can update any of the counters at a time, even though they are independent variables.

In contrast, a fine-grained approach would use separate locks for each counter, allowing multiple threads to update different counters simultaneously, improving concurrency and performance.

Since the three counters are independent, fine-grained locking would provide better concurrency. However, for this assignment, the coarse-grained approach is sufficient and safer for ensuring correctness.

---

## Part 3: Synchronization Analysis (1 mark)

### Critical Section #1: Counter Variables

**Which variables**:  
contextSwitchCount, completedProcessCount, totalWaitingTime

**Why they need protection**:  
These variables are shared among multiple threads and are frequently updated during execution. Since operations like incrementing (e.g., contextSwitchCount++) are not atomic, concurrent updates may lead to lost updates and incorrect values.

**Synchronization mechanism used**:  
ReentrantLock


**Code snippet**:
// protect shared counters
SharedResources.lock.lock();
try {
    contextSwitchCount++;
} finally {
    SharedResources.lock.unlock();
}

**Justification**:  
ReentrantLock ensures mutual exclusion, allowing only one thread to update the shared counters at a time. This guarantees consistency and prevents race conditions in critical updates.

---

### Critical Section #2: Execution Log

**What resource**:  
executionLog (ArrayList)

**Why it needs protection**:  
The execution log is accessed by multiple threads simultaneously to record events. Without synchronization, concurrent modifications can lead to inconsistent logs or runtime exceptions such as ConcurrentModificationException.

**Synchronization mechanism used**:  
ReentrantLock (separate logLock)


**Code snippet**:
// protect execution log
SharedResources.logLock.lock();
try {
    executionLog.add(message);
} finally {
    SharedResources.logLock.unlock();
}
**Justification**:  
Using a dedicated lock for the execution log prevents concurrent modification issues and ensures that log entries are recorded safely and in order.

---

### Critical Section #3: CPU Semaphore

**Purpose of semaphore**:  
To control access to the CPU and ensure that only one thread executes at a time.

**Number of permits and why**:  
1 permit (binary semaphore), because the CPU can only execute one process at a time.

**Where implemented**:  
Inside the run() and runToCompletion() methods, where each thread must acquire the semaphore before execution and release it after completion.


**Code snippet**:

// acquire CPU
SharedResources.cpuSemaphore.acquire();
try {
    // process execution
} finally {
    // release CPU
    SharedResources.cpuSemaphore.release();
}

**Effect on program behavior**:  
The semaphore ensures that threads execute one at a time, simulating real CPU scheduling. It prevents simultaneous execution, avoids conflicts, and ensures predictable and consistent program behavior.

---

## Part 4: Testing and Verification (2 marks)

### Test 1: Consistency Check
**What I tested**:  
Running the program multiple times to verify consistent results.

**Testing procedure**:  
# Compiling the program
javac SchedulerSimulationSync.java

# Running the program multiple times
java SchedulerSimulationSync
java SchedulerSimulationSync
java SchedulerSimulationSync
java SchedulerSimulationSync
java SchedulerSimulationSync

**Results**:  
The program produced consistent outputs across all runs. The number of context switches, completed processes, and total waiting time remained stable and correct in each execution. The execution log also showed proper sequential behavior without missing or duplicated entries.

**Why synchronization is necessary**:  
Without synchronization, race conditions could occur when multiple threads update shared variables such as contextSwitchCount, completedProcessCount, and totalWaitingTime simultaneously. This could lead to incorrect counts or inconsistent results. Additionally, concurrent access to executionLog could cause data corruption or runtime exceptions.

**Conclusion**:  
Synchronization ensures deterministic behavior and consistent results across multiple executions, confirming that shared resources are properly protected.

---

### Test 2: Exception Testing
**What I tested**:  
Checking for ConcurrentModificationException in the execution log.

**Testing procedure**:  
I ran the program multiple times while ensuring that multiple threads were writing to the executionLog concurrently. I specifically monitored the program output for any runtime exceptions.

**Results**:  
No ConcurrentModificationException occurred during any of the test runs. All log entries were recorded successfully and in the correct order.

**What this proves**:  
This confirms that the executionLog is properly synchronized using ReentrantLock, and concurrent modifications are safely handled.

---

### Test 3: Correctness Verification
**What I tested**:  
Verifying the correctness of final computed values such as total burst time, context switches, and completed processes.

**Expected values**:  
- Total burst time should equal the sum of all process burst times.  
- The number of completed processes should match the number of input processes.  
- Context switches should reflect correct scheduling behavior.

**Actual values**:  
- Total burst time matched the expected sum.  
- All processes were completed successfully.  
- Context switches were consistent with the round-robin scheduling logic.

**Analysis**:  
The results confirm that synchronization did not interfere with the correctness of the scheduling logic. Instead, it ensured accurate and reliable updates to shared variables.

---

### Test 4: Different Scenarios
**Scenario tested**:  
Changing the time quantum and observing system behavior.

**Purpose**:  
To evaluate how different scheduling parameters affect execution and verify that synchronization still works correctly under varying conditions.

**Results**:  
With a smaller time quantum, the number of context switches increased, while with a larger time quantum, context switches decreased. In both cases, the program executed correctly without errors.

**What I learned**:  
This test demonstrated that synchronization mechanisms are independent of scheduling parameters. Regardless of the time quantum, locks and semaphores ensured safe execution and consistent results.
---

## Part 5: Reflection and Learning

### What I learned about synchronization:

Through this assignment, I learned the importance of synchronization in multithreaded systems. I understood how race conditions occur when multiple threads access shared resources without proper control. I also learned how ReentrantLock ensures mutual exclusion, preventing simultaneous access to critical sections. Additionally, I gained practical experience using Semaphore to control access to limited resources such as the CPU. One key challenge was identifying all critical sections that required protection. Another important insight was the necessity of using try-finally blocks to guarantee that locks and semaphores are always released. Overall, this assignment strengthened my understanding of thread safety and concurrent programming concepts.

---

### Real-world applications:

Give TWO examples where synchronization is critical:

**Example 1**:  
Banking systems, where multiple transactions may attempt to update the same account balance simultaneously. Synchronization ensures that deposits and withdrawals are processed correctly without data corruption.

**Example 2**:  
Operating system process scheduling, where multiple processes compete for CPU access. Synchronization mechanisms like semaphores ensure that only one process uses the CPU at a time, maintaining system stability.

---


### How I would explain synchronization to others:

Synchronization can be explained as a way to organize access when multiple threads want to use the same resource. Imagine a single bathroom shared by many people; only one person can use it at a time. A lock acts like a key that allows only one person to enter, while others must wait. Similarly, a semaphore controls how many people can enter at once. Without synchronization, threads would interfere with each other, leading to incorrect results. Therefore, synchronization ensures order, safety, and correctness in concurrent programs.

---

## Part 6: GitHub Repository Information

**Repository URL**: https://github.com/sarafbinsaif/OS-Assignment3-Sara-BinSaif

**Number of commits**: 4

**Commit messages**: 
1. Add Student ID
2. Add mutex locks for shared resources  
3. Protect execution log with lock  
4. Add semaphore for CPU scheduling 

---

## Summary

**Total time spent on assignment**:  
Approximately 5.5 hours

**Key takeaways**:  
1. Synchronization is essential to prevent race conditions in multithreaded programs.  
2. Locks and semaphores serve different purposes but work together to ensure safe execution.  
3. Proper testing is necessary to verify correctness and consistency.

**Most challenging aspect**:  
Identifying all critical sections and ensuring that locks were correctly applied without causing deadlocks.

**What I'm most proud of**:  
Successfully implementing synchronization mechanisms and achieving consistent and correct program output across multiple test runs.

---

**End of Documentation**
