# Reader-Writer-Starve-Free-Solution
## Readers-Writers Problem of Synchronization:
Readers Writers problem is a classical problem of synchronization based on sharing of data, like files among different processes.
There are only two types of process which try to access this file:
<br>
  &nbsp; - Type 1: To read from the file
<br>
	&nbsp; - Type 2: To write into the file
<br>
  Problem arises when both of them try to access the file at the same time/ access the critical section the same time.

## Solution to the problem:
The most common solution to this problem prioritize readers over writers i.e. critical section maybe accessed by two readers at a time but only by a single writer.
In the given solution i have proposed a starve free solution for readers writers problem which do not prioritize any one of them. Solution is present in the solution.txt flie in main branch.
This solution further satisfies the 3 requirements:
 <br>
	&nbsp; 1.Mutual exclusion
 <br>
	&nbsp; 2.Progress
 <br>
	&nbsp; 3.Bounded waiting 

## Explaination of the proposed solution:
### Variables and Semaphores used:
&nbsp; Semaphore T=1;	// Used for determining whose turn is there to execute
    <br>
		&nbsp; Semaphore CS=1;	// Used for accessing the critical section
    <br>
		&nbsp; Semaphore Mutex=1;	// Used for ensuring mutual exclusion (specifically in reader's part
### Reader's Code:
```
while(true)
{
	//Entry Section
	Wait(T);
	Wait(Mutex);
	r_c++;
	if(r_c == 1)
		Wait(CS);
	Signal(T);
	Signal(Mutex);


	//Critical Section

	//Exit Section
	Wait(Mutex);
	r_c--;
	if(r_c == 0)
		Signal(CS);
	Signal(Mutex);
	
	//Remainder Section
}

```
1. Initially the reader wait for its turn.
2. Reader wait to increase the count of readers.
3. When wait is over count is inreased.
4. If it is the first reader it needs to wait to get access to critical section.
5. Release turn and mutex semaphores.
6. Reading is completed.
7. Waiting for access to reduce the count of readers.
8. When wait is over reader's count reduces.
9. If it is the last reader it releases access to critical section for next reader or writer.
10. Release mutex semaphore or access to read count.
### Writer's Code:
```
while(true)
{
	//Entry Section
	Wait(T);
	Wait(CS);
	Signal(T);


	//Critical Section
	

	//Exit Section
	Signal(CS);
	//Remainder Section
}

```
1. Initially the writer wait for its turn.
2. Writer wait to access the critical section.
3. It releases turn so that next reader or writer can access it.
4. Writing is completed or critical section is executed.
5. Release the access to critical section.

## How is it starve free?
The turn(T) semaphore makes it possible for either of them to take chance in the way they want, Also apart from one or more reader's accessing critical section in this solution the writer can take the command at anytime and write, this wil solve the problem of starvation and makes the code starve free.
