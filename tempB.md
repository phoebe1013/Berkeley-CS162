## Lecture 8, Readers-Writers Language Support for Synchronization

##### Review: Implementation of Locks by Disabling Interrupts

	+ Key idea: Maintain a lock variable and impose mutual exclusion only during operations on that variable

![Locks](images/07-003.png "Locks")