# Memory Paging

## page allocation

* When the process needs memory, it requests the memory from the OS. The OS allocates memory to the process in units called pages.
* And then the OS updates the page table to map the virtual memory addresses to the physical memory addresses.
* And also the OS update the counter to keep track of the number of times a page has been accessed.

1. Check memory limit
    * The OS checks the cgroup if the process has exceeded its memory limit (soft and hard limit).
    * If the process has not exceeded, then the OS allocates memory to the process.
    * If the process has exceeded, then the OS will depend on the memory limit policy.
        * The OS can kill the process.
        * The OS can swap out the process.
        * The OS can throttle the process.
        * The OS can do nothing.
2. Allocate page
    * The OS allocates a page or multiple pages to the process.
    * The OS updates the page table to map the virtual memory addresses to the physical memory addresses.
3. Update counter
    * The OS updates the counter to keep track of the number of times a page has been accessed.


## page deallocation

* When the process no longer needs memory, it releases the memory back to the OS. The OS deallocates memory from the process in units called pages.
* And then the OS updates the page table to unmap the virtual memory addresses from the physical memory addresses.
* And also the OS update the counter to keep track of the number of times a page has been accessed.

