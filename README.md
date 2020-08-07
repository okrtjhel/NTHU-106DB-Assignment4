# Assignment 4
In this assignment, you are asked to optimize file and buffer modules of VanillaCore.

## Steps
To complete this assignment, you need to

1. Fork the Assignment 4 project
2. Trace the code in `as4-core-patch`'s `org.vanilladb.core.storage.file` and `org.vanilladb.core.storage.buffer` package yourself
3. Modify the current implementation to reach better performance
4. Test (part of) correctness using provided test cases
5. Run experiments using provided benchmark project
6. Write an experiment analysis report
7. Push your repository to Gitlab and open a merge request


## Main Objective: Optimization

The current file and buffer manager of VanillaCore are not optimized for multi-threading. We just wrap all public methods with `synchronized` to keep thread-safty. The main objective of this assignment is to optimize these two modules for better performance.


## Constraints

Here are some constraints you must follow during coding

- You **can only** modify the code in these packages:
  - `org.vanilladb.core.storage.file`
  - `org.vanilladb.core.storage.buffer`
- You **can not** change the existing public APIs (public methods, public properties), but you can add new public methods for optimization.
- Your optimization must at least pass the provided test cases.

If you follows the above rules and hand in your code along with your report in time, you will get at least 60% of scores. The rest of scores will be given by the correctness (in logic) and the performance of your optimization.

Note that even if you passed all the test cases, it would not mean that there is no bug. We will check the logic of your modification during the demo.

## Experiments

You need to run experiments to demonstrate how much performance your optimization improves. There are two types of experiments you must do:

- Show overall improvement
- Show improvement made by each optimization

This time we request you to record both throughput and average latency of each of your experiment. Also, you should show the results using plots, instead of tables.

### Overall Improvement

Firstly, you must run experiments with and without all your optimization to show the overall improvement in this assignment.

### Improvement Made by Each Optimization

Then, for each optimization you performs, you should run experiments to show the improvement made by that particular optimization.

### Parameters

This time, we provide a micro-benchmark to measure your performance. The micro-benchmark is very simliar to the benchmarker in assignment 2.

There are some parameters in the benchmark you can adjust:
- NUM_RTES (value > 1) - The number of clients
- RW_TX_RATE (1.0 >= value >= 0.0) - The probability of generating a write transaction
- TOTAL_READ_COUNT (value >= 1) - The number of records read by a transaction
- LOCAL_HOT_COUNT (TOTAL_READ_COUNT >= value >= 0) - The number of **hot** records read by a transaction
- WRITE_RATIO_IN_RW_TX (1.0 >= value >= 0.0) - The ratio of writes to the total reads of a transaction
- HOT_CONFLICT_RATE (0.1 >= value >= 0.001) - The probability of a hot record conclicting with each other

There is also a configuration in VanillaCore you can try:
- BUFFER_POOL_SIZE (value > 1) - The size of buffer pool

Note that it is hard to see the effect of the optimization for `org.vanilladb.core.storage.file` when VanillaCore has a large buffer pool because it makes VanillaCore seldom fetch data from disks.

## The report

- Briefly explain what you exactly do for optimization
- Experiments
  - Your experiement enviornment including (a list of your hardware components, the operating system)
    - e.g. Intel Core i5-3470 CPU @ 3.2GHz, 16 GB RAM, 128 GB SSD, CentOS 7
  - The experiments showing the overall improvement
  - The experiments showing the improvement made by each of your optimization
  - Analyze and explain the result of the experiments
- **Discuss and conclude why your optimization works**

	Note: There is no strict limitation to the length of your report. Generally, a 2~3 pages report with some figures and tables is fine. **Remember to include all the group members' student IDs in your report.**


## Submission

The procedure of submission is as following:

1. Fork our [Assignment 4](https://shwu10.cs.nthu.edu.tw/courses-databases-2018-spring/db18-assignment-4) on GitLab
2. Clone the repository you forked
3. Finish your work and write the report
4. Commit your work, push your work to GitLab.
  - Name your report as `[Team Member 1 ID]_[Team Member 2 ID]_assignment2_report`
    - E.g. `102062563_103062528_assignment2_reprot.pdf`
5. Open a merge request to the original repository.
  - Source branch: Your working branch.
  - Target branch: The branch with your team number. (e.g. `team-1`)
  - Title: `Team-X Submission` (e.g. `Team-1 Submission`).

**Important: We do not accept late submission.**

## No Plagiarism Will Be Tolerated

If we find you copy someone’s code, you will get 0 point for this assignment.

## Demo

Due to the complexity of this assignment, we hope you can come to explain your work face to face. We will announce a demo time registration table after submission. Don't forget to choose the demo time for your team.

## Hints

### Smaller Critical Section

Critical sections are usually used to protect some shared resource
- Reducing the size of critical sections usually makes transaction have less chance to block each other
- Some kinds of transactions will be stalled during execution due to some critical sections, even if they do not need to use those resource
- Java standard library provides lots of convenient data structures that optimized for multi-threading. You can find them in `java.util.concurrent`.

### Never Do It Again

Some works may take lots of time, such as calculating hash code for a `BlockId`. Part of them may stay unchanged in the future, but the program still repeatedly calculate them when we need them. Those values only need to be calculated once and could be cached to avoid repeated calculation.

## Deadline
Sumbit your work before **2018/05/08 (Tue.) 23:59:59**.
