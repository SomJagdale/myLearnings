Operational Interview Quetions

1. Can you explain the difference between `ps`, `top`, and `htop` commands in Linux? How do you use them to monitor and analyze process performance in a production environment?
   - **`ps`**: A snapshot tool, shows the current processes running at a specific time. It provides detailed information about processes but doesn’t update in real time.
   - **`top`**: A real-time process viewer. It refreshes every few seconds and displays CPU, memory usage, and system load in real-time.
   - **`htop`**: Similar to `top` but more interactive and user-friendly. It has a colored interface and supports scrolling through process lists for detailed monitoring.
   - **Usage**: In a production environment, `ps` is useful for capturing the current state of the processes, while `top` or `htop` is better suited for real-time monitoring. I often use `ps aux` to list all processes and their statuses and `htop` to drill down into resource-heavy processes.

2. How do you use `netstat`, `ss`, or `lsof` to troubleshoot network connectivity issues? Can you explain how you would use these tools to identify open ports and active connections?
   - **`netstat`**: Shows network connections, routing tables, and interface statistics. Common command: `netstat -tuln` (lists listening TCP/UDP ports).
   - **`ss`**: A faster alternative to `netstat`, provides similar data with less overhead. Command `ss -tuln` also lists listening ports.
   - **`lsof`**: Lists open files, including network sockets. `lsof -i :80` can tell you which process is using port 80.
   - **Usage**: I use `netstat` or `ss` to identify listening ports and active connections in case of issues like port conflicts or network delays. `lsof` helps in pinpointing which process is holding a network socket.

3. How would you use `ulimit` to manage system resources in Linux? Can you explain how to configure and adjust limits for file descriptors or process memory in a production scenario?
   - **`ulimit`** manages user-level resource limits, such as open files (`nofile`), processes (`nproc`), or memory (`memlock`).
   - For example, to increase the number of open file descriptors for a process, use `ulimit -n 65536`. In `/etc/security/limits.conf`, you can set persistent limits for specific users.
   - **Usage**: In production, I use `ulimit` to prevent resource exhaustion (like file descriptor limits in servers handling many concurrent connections). Adjusting `nofile` is common for database servers.

 4. How do you set conditional breakpoints in `gdb` to stop execution only when specific conditions are met? Can you provide a practical example of using breakpoints and watchpoints during a debugging session?
   - Use `break [location] if [condition]` to set a conditional breakpoint. Example: `break myfile.cpp:50 if x > 10` stops the execution at line 50 only when `x` exceeds 10.
   - **Watchpoints** track changes in variable values. `watch x` will pause when `x` changes.
   - **Usage**: In one instance, I set a conditional breakpoint to pause only when a variable had an unexpected value, which allowed me to avoid stepping through hundreds of iterations unnecessarily.

5. When dealing with a core dump, how do you use `gdb` to analyze the state of a crashed application? Can you walk me through the steps to load and examine a core dump file to determine the root cause of a segmentation fault?
   - **Steps**:
     1. Ensure core dumps are enabled: `ulimit -c unlimited`.
     2. Run the crashing program and locate the core dump (usually in the working directory).
     3. Open `gdb` with the executable and core dump: `gdb ./app core`.
     4. Use `bt` (backtrace) in `gdb` to view the call stack at the time of the crash.
   - **Usage**: I’ve used this approach to track down segmentation faults where memory was incorrectly accessed after being freed, and a `bt` backtrace helped pinpoint the offending function.

 6. Have you ever used `gdb` for remote debugging? Can you explain how to set up and perform remote debugging for a process running on a different machine?
   - Remote debugging is done by running `gdbserver` on the remote machine and attaching to a process or executable, e.g., `gdbserver :2345 ./app`.
   - Connect from the local machine: `gdb ./app`, then `target remote [remote-ip]:2345`.
   - **Usage**: I’ve used remote debugging in cloud environments where direct access to servers was restricted. By setting up `gdbserver`, I could debug production-like issues without full access.

7. What is ThreadSanitizer (TSan), and how do you use it to detect data races in multi-threaded applications? Can you explain a scenario where TSan helped you catch a concurrency issue that was difficult to reproduce?
   - **TSan** detects data races by instrumenting memory accesses and synchronization events. Compile with `-fsanitize=thread` and run your program.
   - **Usage**: TSan once caught a non-reproducible bug where two threads were racing to update shared data without proper locking. This tool helped me identify exactly where the race was occurring, which was nearly impossible to catch otherwise.

8. How do you integrate TSan into a CI/CD pipeline? Can you share any best practices for incorporating thread-safety checks into your automated testing workflows?
   - Add TSan to the test build (using `-fsanitize=thread`) and configure CI to execute the tests in a multi-threaded environment.
   - **Best Practice**: Ensure that sanitized builds are separate from release builds to avoid performance penalties. Regularly run tests under TSan as part of the pipeline to catch regressions.

9. Can you explain how AddressSanitizer (ASan) helps detect memory-related errors like buffer overflows and memory leaks? Can you give an example where ASan caught a critical bug in your application?
   - **ASan** detects memory issues like out-of-bounds access and use-after-free by instrumenting memory allocations. Compile with `-fsanitize=address`.
   - **Usage**: I once had a bug where a buffer overflow wasn’t immediately obvious but caused crashes later in execution. ASan immediately caught the overflow and helped me resolve it.

10. AddressSanitizer can introduce performance overhead during testing. How do you balance thorough memory error detection with maintaining reasonable testing performance?
   - Run ASan selectively—either on a subset of tests or periodically as part of nightly builds. Avoid running it on time-sensitive or performance-critical builds.
   - **Best Practice**: Run sanitized tests in a parallel job with regular tests to avoid slowing down the main CI/CD pipeline.

11. Can you explain the difference between `git rebase` and `git merge`? In which scenarios would you prefer one over the other, and how do you handle conflicts during a rebase?
   - **Rebase** rewrites history by applying changes on top of another branch, making for a linear history. **Merge** retains the full history with a new merge commit.
   - **Usage**: I prefer `rebase` for feature branches to keep the history clean. I use `merge` in collaborative branches (e.g., `develop`) where preserving history is essential.

12. Have you used `git bisect` to track down the introduction of a bug? Can you describe the process and how you can leverage this tool to narrow down the problematic commit?
   - Start with `git bisect start`, mark good and bad commits (`git bisect good`/`bad`), and `git bisect` will divide the history to identify the commit that introduced the bug.
   - **Usage**: I once used `git bisect` to trace a subtle regression over hundreds of commits. It narrowed the issue down to a specific merge, making it much easier to identify the problem.

13. What branching strategies do you follow for large projects? Can you explain how you manage long-running feature branches, hotfixes, and release branches in a team environment using Git?

### 14. **Git – Submodules**
   - **Question:** How do you manage projects with multiple Git repositories using `git submodule`? What are the challenges of using submodules, and how do you handle updates across multiple repositories?

### 15. **Linux – Kernel Logs and System Monitoring**
   - **Question:** How do you use `dmesg`, `journalctl`, and `/proc` to troubleshoot issues at the kernel level? Can you share an example where these tools helped you resolve a low-level system issue?

### 16. **Linux – Filesystem and Disk Usage**
   - **Question:** How do you handle filesystem-related issues such as out-of-disk-space errors? Can you explain how to use tools like `du`, `df`, and `iotop` to analyze and resolve disk space problems in a production environment?

### 17. **Linux – Performance Tuning**
   - **Question:** How do you use tools like `perf`, `strace`, and `ltrace` to profile and analyze the performance of a Linux-based application? Can you provide an example where you identified and optimized a performance bottleneck?

### 18. **Linux – Security and Permissions**
   - **Question:** How do you troubleshoot file permission issues in Linux? Can you explain the differences between `chown`, `chmod`, and `setfacl`, and how you use them to manage user access to files and directories?

### 19. **Debugging – Multithreading and Concurrency**
   - **Question:** What strategies do you use to debug multithreaded applications in Linux? Can you discuss any challenges you’ve faced in synchronizing threads or debugging deadlocks, and how you resolved them?
13. Git – Branching Strategies
Answer:
Common strategies include Git Flow, Feature Branches, and Trunk-Based Development. For long-running features, use feature branches, and integrate often to reduce merge conflicts.
Use case: Use Git Flow for projects with clearly defined release cycles, employing develop for staging new features and master for production-ready code.
14. Git – Submodules
Answer:
git submodule lets you include one Git repository inside another. While it’s useful for modular projects, it can be tricky to manage versions across submodules. Use git submodule update --init to sync submodule dependencies.
Use case: In large projects with dependencies managed separately (e.g., third-party libraries), submodules allow you to version-control external repositories.
15. Linux – Kernel Logs and System Monitoring
Answer:
dmesg displays kernel messages, useful for diagnosing hardware or driver issues. journalctl queries system logs, including kernel, boot, and application logs. /proc is a virtual filesystem that provides process and system information.
Use case: Use dmesg | grep error to troubleshoot driver or hardware issues and journalctl -xe for detailed log analysis.
16. Linux – Filesystem and Disk Usage
Answer:
Use du to calculate directory sizes and df to report filesystem space usage. iotop shows I/O usage by process.
Use case: When a server runs out of disk space, du -sh helps identify large directories. df -h provides overall disk usage, and iotop identifies processes with high disk I/O.
17. Linux – Performance Tuning
Answer:
perf analyzes CPU performance by profiling applications and system calls. strace and ltrace trace system calls and library calls made by a process, respectively.
Use case: To identify a performance bottleneck, use perf to profile CPU usage and strace to analyze slow system calls.
18. Linux – Security and Permissions
Answer:
chown changes file ownership, chmod modifies permissions, and setfacl manages access control lists (ACLs). setfacl offers more fine-grained control than traditional UNIX permissions.
Use case: When multiple users need different levels of access to the same file, use setfacl to grant specific permissions without altering the core permission structure.
19. Debugging – Multithreading and Concurrency
Answer:
Debugging multi-threaded applications can be challenging due to race conditions and deadlocks. Use tools like gdb (with thread awareness), TSan, and pstack to diagnose thread-related issues.

19. Debugging – Multithreading and Concurrency
Answer:
Debugging multi-threaded applications can be challenging due to race conditions and deadlocks. Use tools like gdb (with thread awareness), TSan, and pstack to diagnose thread-related issues.
Use case: For a deadlock, use gdb to examine thread states and pstack to print backtraces of all threads.
20. Debugging – Core Dump Configuration
Answer:
To enable core dumps, configure /proc/sys/kernel/core_pattern to specify where core files should be stored (e.g., /var/crash/core-%e-%p-%t). Set ulimit -c unlimited to allow core dump generation.
Use case: When debugging crashes in production, configure core dumps for post-mortem analysis. Use gdb to load the dump and analyze the crash.
