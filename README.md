# Operating Systems Labs (Java)

Four small Java simulations of core OS mechanisms: CPU scheduling, process synchronization, paged memory and a FAT file system.

Year: 2021-2022 (git: 2022-04-05) · Stack: Java (plain JDK, no build tool) · Context: university coursework (KPI), course "Introduction to Operating Systems", variant 9

## What's inside

- `lab1_os/` - process scheduling. A two-level scheduler: an "interactive" queue served by Round Robin (quantum 2) and a "background" queue served by SRTF (shortest remaining time first). The CPU time is split 80/20 between them. Processes get random arrival and burst times; every tick prints remaining execution, arrival and waiting time tables, and the run ends with average wait for both queues.
- `lab2_os/` - process synchronization. A flight ticket vending machine with two threads: `ThreadA` is the user interface (menu, money input), `ThreadB` is the cashier that computes change. They communicate through a shared `PostBox`. The coin bank (1, 2, 5, 10, 25, 50 kopecks, fixed initial counts) pays change greedily and refuses when exact change is impossible.
- `lab3_os/` - memory management. Paged virtual memory: processes are split into sections, sections into pages, and 7 physical frames back them. Menu-driven: load/unload a process, translate a virtual address to a physical one, clear a process. When frames run out, the oldest page is evicted.
- `lab4_os/` - file systems. A FAT model with a 256-entry cluster table and a 10-entry root directory. Menu: print the FAT table (hex grid), list the directory, create, grow and delete files, and walk a file's cluster chain.


## Interesting bits

- `lab1_os/src/com/Main.java`: both algorithms share one `Process` class and the same `printExec` trace, so SRTF and RR can be compared by average waiting time on the same kind of random workload.
- `lab2_os/src/com/Main.java`: synchronization is done with a shared flag object (`PostBox.sent`, `change`, `signal`) and `Thread.sleep` polling rather than locks or semaphores.
- `lab3_os/src/com/Main.java`: eviction picks the page with the earliest `LocalDateTime` load time (FIFO-style); the demo processes in `Processes` have more virtual pages in total than the 7 physical frames, as the task required.
- `lab4_os/src/com/Main.java`: each cluster stores a `next` index, so a file is a linked list through the table; `printArrayOfClusters` follows it.

## Run

Verified here with a JDK (javac/java, Java 14+ is needed for the switch expression in lab3):

```
javac -d out lab1_os/src/com/Main.java && java -cp out com.Main
```

Replace `1` with `2`, `3`, `4` for the other labs. Lab 1 was run and prints the traces and averages; labs 2-4 were compiled, and lab 4 starts its menu, but were not exercised interactively.

## Note

Written in 2021-2022, before AI coding assistants. Kept as-is for history.
