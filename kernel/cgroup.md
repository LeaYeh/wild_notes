# Control Groups (cgroups)

* Cgroups are used to,
    * **limit the resources**
    * **prioritize the resources**
    * **monitor the resources**
* The resources that can be limited, prioritized, and monitored using cgroups are,
    * CPU
    * Memory
    * Storage
    * Network
    * Devices

## Memeory Cgroup concepts

### Definitions

* cgroup
    * control group, use to limit the resources for a group of processes.
* page
    * The **smallest unit of memory** that can be allocated by the OS.
    * The **size of a page** is determined by the OS and is usually **4KB**.
* page table
    * A **data structure** that is used by the OS to **map virtual memory addresses to physical memory addresses**.
    * The **size of a page table** is determined by the OS and is usually **4KB**.
* page fault
    * An **exception** that is raised by the CPU when a process tries to access a page that is not in memory.
    * The **OS handles page faults** by loading the page from disk into memory.
* page cache
    * A **cache** that is used by the OS to **store pages that have been read from disk**.
    * The **size of the page cache** is determined by the OS and is usually **4KB**.
* page table cache
    * A **cache** that is used by the OS to **store page tables**.
    * The **size of the page table cache** is determined by the OS and is usually **4KB**.
* counter
    * A **data structure** that is used by the OS to **keep track of the number of times a page has been accessed**.
    * The **size of a counter** is determined by the OS and is usually **4KB**.