
process:
    independent execution unit(Address Space(for code and data), System Resources, etc). Preemptive
    parallel for multi core


thread:
    execution unit in a process. Independent PC, registers, stack, exception handler, etc

    - Hardware thread: managed by CPU. Like Hyperthreading or SMT. Parallel
    - Software thread:
      - Kernel thread. Threads Managed by kernel. Preemptive. Since it's preemptive, OS can take advantage of multiple CPUS/Cores by running more than one thread at the same time.
        Data integerity is a big issue for Kernel thread.
        a) It's preemptive, but also share address space. For example, one thread is stopped in the middle of updating a chunk of data, leaving the integrity of the data in a bad or incompleete state.
        b) Maybe parallel. Two threads sharing the same space modifying the same address.
      - Userspace thread. Non-Preemptive. Super fast thread swithcing since no need to trap into the kernel. Drawback is the inability to do blocking I/O. Also, when a thread is blocked, other threads within the task lose timeslice as well since it's non-preemptive. As it's non-preemptive, two different threads of the same process can't run parallelly.
        - Fiber: userspace thread managed by os.
        - Coroutine: userspace thread managed by applications. It's defined in the language/library level.
        - Goroutine: Special, weird thread. It's user space thread(non-preemptive. i.e, Can't be interrupted). But it's multiplexed onto kernel threads so they can run in parallel.
        Another weird stuff is that Erlang and Golang claims that their thread is preemptive. That needs further research.


References:
  http://www.tldp.org/FAQ/Threads-FAQ/Types.html
  http://stackoverflow.com/a/19518207/1523768
  http://stackoverflow.com/a/19855296/1523768
  http://jlouisramblings.blogspot.com/2013/01/how-erlang-does-scheduling.html
  http://www.weibo.com/1915508822/Af6BfFMp3?from=page_1005051915508822_profile&wvr=6&mod=weibotime&type=comment
  http://stackoverflow.com/questions/3324643/processes-threads-green-threads-protothreads-fibers-coroutines-whats-the
  http://programmers.stackexchange.com/questions/254140/is-there-a-difference-between-fibers-coroutines-and-green-threads-and-if-that-i
