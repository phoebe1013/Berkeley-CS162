## Lecture 11, Thread Scheduling(con't), Protection: Address Spaces


##### Review: Banker's Algorithm for Preventing Deadlock

+ Banker's algorithm (less conservative):
		- Allocate resources dynamically
			* Evaluate each request and grant if some ordering of threads is still deadlock free afterward
			* __Technique: pretend each request is granted, then run deadlock detection algorithm, substituting ([Max(node)] - [Alloc(node)] <= [Avail] ) for ( [Request(node)] <= [Avail] ) Grant request if result is deadlock free (conservative!)__
			* Keeps system in a "SAFE" state, i.e. there exists a sequence {T1, T2, ... Tn} with T1 requesting all remaining resources, finishing then T2 requesting all remaining resources, etc..
		- Algorithm allows the sum of maximum resource needs of all current threads to be greater then total resources


![Banker's Algorithm for Preventing Deadlock](images/10-001.png "Banker's Algorithm for Preventing Deadlock")


----------------------

##### Review: Scheduling

* __Scheduling__: selecting a waiting process from the ready queue and allocating the CPU to it

	* __FCFS Scheduling__:
		+ Run threads to completion in order of submission
		+ Pros: Simple (+)
		+ Cons: Short jobs get stuck behind long ones (-)

	* __Round-Robin Scheduling__:
		+ Give each thread a small amount of CPU time when it executes; cycle between all ready threads
		+ Pros: Better for short jobs (+)
		+ Cons: Poor when jobs are same length (-)

	* __Shortest Job First(SJF)/Shortest Remaining Time First(SRTF)__:
		+ Run whatever job has the least amount of computation to do/least remaining amount of computation to do
		+ Pros: Optimal (average response time)
		+ Cons: Hard to predict future, Unfair


----------------------

##### Review: FCFS and RR Example with different Time Quantum

> Idea here is that short bursts are frequent and long bursts are not. The reason why short bursts are frequent is because they're coming from people doing I/O typing whatever. Those are the ones that we'd like to somehow give precedence to not annoy people. 

> That let us to reject FCFS because it didn't service well the short bursts cases.

>> Best FCFS is the shortest should be first, and then next shortest so on and so forth. That's has the _best average waiting time_ and the _best average completion time_!

![Round Robin Example](images/10-011.png "Round Robin")


----------------------

* What if we Know the Future?

	+ Could we always mirror best FCFS?
	+ _Shortest Job First(SJF)_:
		- Run what ever job has the least amount of computation to do
		- Sometimes called "Shortest Time to Completion First" (STCF)

	+ _Shortest Remaining Time First (SRTF)_:
		- Preemptive version of SJF: if job arrives and has a shorter time to completion than the remaining time on the current job, immediatel preempt CPU
		- Sometimes called "Shortest Remaining Time to Completion First" (SRTCF)

	+ These can be appliced either to a whole program or the current CPU burst of each program
		- Idea is to get short jobs out of the system
		- _Big effect on short jobs, only small effect on long ones_
		- Result is _better average response time_


----------------------

* Discussion

	+ _SJF/SRTF are the best you can do at minimizing average response time_
		- Probably optimal (SJF among non-preemptive, SRTF among preemptive)
		- Since SRTF is always at least as good as SJF, focus on SRTF 
		
	+ Comparision of SRTF with FCFS and RR
		- What if all jobs the _same length_ ?
			* SRTF becomes the same as FCFS (i.e. FCFS is best can do if all jobs the same length)
		- What if jobs have _varying length_ ?
			* SRTF (and RR): short jobs not stuck behind long ones


----------------------

* Example to illustrate benefits of SRTF

> Job C is a multimedia app. It does some compute for a millisecond accesses say a CD-ROM for nine milliseconds and then compute so on.

![Example](images/11-001.png "Example")


>> 1. It's not doing well. Fraction is too low.
>> 2. Disk Utilization is good. But there is more switch time than there was before. So we really don't know whether we get exactly 90. It's likely we're not going to get a liitle under 90. And a lot switching is wasteful.
>> 3. It's an ideal because what happen is C runs for its millisecond then A starts running, then C is ready to go again. So now it's shortest remaining time. It's gets to run and then keeps running and so on.

>>> Why do we keep running A instead of B? ---- Now A is basically shorter than B after the very first slice so now A is going to go to completion. This has a lot benefits. First, we get 90% disk utilization; Second, gets us pretty close to 100% utilization for both A and B.

> B has to wait. 
>> 1. From _average completion or response time_: It's good because the average response time between A and B is better than if we swap all the time.
>> 2. If you are a person that started job B, it's bad.


![Example](images/11-002.png "Example")


----------------------

* Prediting the Length of the Next CPU Burst

	+ __Adaptive__: Changing policy based on past behavior
		- CPU scheduling, in virtual memory, in file systems, etc
		- Works because programs have predictable behavior
			* If program was I/O bound in past, likely in future
			* If computer behavior were random, wouldn't help
			
	+ Example: SRTF with estimated burst length:

![Example](images/11-003.png "Example")


----------------------

* Multi-Level Feedback Scheduling

	+ Another method for exploiting past behavior
		- First used in CTSS
		- __Multiple queues, each with different priority__
			* Higher priority queues often considered "foreground" tasks
		_ __Each queue has its own scheduling algorithm__
			* e.g foreground - RR, background - FCFS
			* Sometimes mutiple RR priorities with quantum incresing exponentially (highest: 1ms, next: 2ms, next: 4ms, ets)

	+ Adjust each job's priorty as follows (details vary)
		- Job starts in highest priority queue
		- If timeout expires, drop one level
		- If timeout doesn't expire, push up one level (or to top)

![Example](images/11-004.png "Example")

> Why does it kind like SRTF? ----  We sort threads. The short things up and the longer things down further.


----------------------

* Scheduling Details
	
	+ Result approximates SRTF:
		- CPU bound jobs drop like a rock
		- Short-runnign I/O bound jobs stay near top
	
	+ Scheduling must be done between the queues
		- __Fixed priority scheduling__:
			* serve all from highest priority, then next priority, etc
		- __Time slice__:
			* each queue gets a certain amount of CPU time
			* e.g, 70% to highest, 20% next, 10% lowest
			
	+ __Countermeasure__(对策): user action that can foil intent of the OS designer
		- For multilever feedback, put in a bunch of meaningless I/O to keep job's priority high
		- Of course, if everyone did this, wouldn't work!

	+ Example of Other program:
		- Playing against competitor, so key was to do computing at higher priority the competitors
			* Put in printf's, ran much faster!
		

----------------------

* Scheduling Fairness
	
	+ What about fairness?
		- Strict fixed-priority scheduling between queues is unfair (run highest, then next, etc):
			* Long running jobs may never get CPU
			* In multics, shut down machine, found 10-year-old job
		- Must give long-running jobs a fraction of the CPU even when there are shorter jobs to run
		- __Tradeoff: fairness gained by hurting avg response time!__

	+ How to implement fairness?
		- Could give each queue some fraction of the CPU
			* What if one long-running job and 100 short-running ones?
			* Like express lanes in a supermarket -- sometimes express lanes get so long, get better service by going into one of the other lines
		- Could increase priority of jobs that don't get service
			* What is done in UNIX
			* This is ad hoc-- waht rate should you increase priorities?
			* And, as system gets overloaded, no job gets CPU time, so everyone increases in priority --> interactive jobs suffer


![Example](images/11-005.png "Example")


----------------------

* Lottery Scheduling
		
	+ Yet another alternative: Lottery Scheduling 
		- Given each job some number of lottery tickets
		- On each time slice, randomly pick a winning ticket
		- On average, CPU time is proportional(按比例的) to number of tickets given to each job
		
	+ How to assign tickets?
		- To approximate SRTF, short running jobs get more, long running jobs get fewer
		- To avoid starvation, every job gets at least one ticket （everyone makes prograss)

	+ Advantage over strict priority shceduling: behaves gracefully as load changes
		- Adding or deleting a job affects all jobs proportionally, independent of how many tickets each job processes


![Example](images/11-006.png "Example")



----------------------

* How to Evaluate a Scheduling algorithm?

	+ Deterministic modeling 
		- takes a predetermined workload(工作量) and compute the performance of each algorithm for that workload

	+ Queueing models
		- Mathmatical approach for handling stochastic(随机的) workloads

	+ implementation/Simulation:
		- Build system which allows actual algorithms to be run against actual data. Most flexible/general.

![How to Evaluate a Scheduling algorithm](images/11-007.png "How to Evaluate a Scheduling algorithm")



----------------------

* A Final World On Scheduling

	+ When do the details of the scheduling policy and fairness really matter?
		- When there aren't enough resources to go around
	
	+ When should you simply buy a faster computer?
		- (Or network link, or expanded highway, or...)
		- One approach: Buy it when it will pay for itself in improved response time
			* Assuming you're paying for worse response time inreduced productivity, customer angst, etc
			* Might think that you should buy a faster X when X is utilized 100%, but usually, response time goes to infinity as utilization --> 100%

	+ An interesting implication of this curve:
		- Most scheduling algorithms work fine in the "linear" portion of the load curve, fail otherwise
		- Argues for buying a faster X when hit "knee" of curve

![A Final World On Scheduling](images/11-008.png "A Final World On Scheduling")




