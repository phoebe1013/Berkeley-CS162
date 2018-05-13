## Class 3 Concurrency(并发性): Process, Threads, Address Space


#### Review

*  History of OS
![History](images/03-001.png "History")

* Migration of OS Concepts & Features
![OS](images/03-002.png "OS")

-----------

* Goals For Today
	+ Finish discussion of OS structure
	+ How do we provide multiprogramming?
	+ What are Processes?
	+ How are they related to Thread and Address Spaces?

----------

* UNIX System Structure -- (Simple - Only one/two levels of code)

![UNIX](images/01-049.png "UNIX")

> Kernel Mode is protected mode.
> Two Level here: User Mode & Kernel Mode
>> If any bugs in file system, then it will crush virtual memory system because there is no protection boundery.


---------

* Microkernel Structure -- (OS built from many user-level processes)

	* Moves as much from the kernel into "user" space;
	 
	* Benefits;
	
	* Detriments(损害);

![Microkernel](images/03-003.png "Microkernel")

* Partition Based Structure for Multicore chips?
![Partition Based Structure](images/03-004.png "Partition Based Structure")























