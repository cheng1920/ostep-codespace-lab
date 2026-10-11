# OSTEP Ch.4 Homework Answers

## Q1

- Prediction / 预测:

```text
Time  PID 0    PID 1    CPU  IOs
1     RUN:cpu  READY    1
2     RUN:cpu  READY    1
3     RUN:cpu  READY    1
4     RUN:cpu  READY    1
5     RUN:cpu  READY    1
6     DONE     RUN:cpu  1
7     DONE     RUN:cpu  1
8     DONE     RUN:cpu  1
9     DONE     RUN:cpu  1
10    DONE     RUN:cpu  1
```

总时间：10 ticks。
CPU 忙碌时间：10 ticks。
CPU 利用率：10 / 10 × 100% = 100%。

- Reasoning / 理由: 两个进程各有 5 条 CPU 指令，没有 I/O。PID 0 先执行完，然后 PID 1 执行，因此 CPU 连续工作 10 ticks。

- Verified result / 验证结果:总时间 10 ticks；CPU 忙碌 10 ticks；CPU 利用率 100.00%。见 q1.txt。

- Analysis / 分析:统计与预测一致。两个进程都没有 I/O，CPU 全程忙碌。

## Q2

- Prediction / 预测:

```text
Time  PID 0    PID 1        CPU  IOs
1     RUN:cpu  READY        1
2     RUN:cpu  READY        1
3     RUN:cpu  READY        1
4     RUN:cpu  READY        1
5     DONE     RUN:io       1
6     DONE     BLOCKED           1
7     DONE     BLOCKED           1
8     DONE     BLOCKED           1
9     DONE     BLOCKED           1
10    DONE     BLOCKED           1
11*   DONE     RUN:io_done  1
```

总时间：11 ticks。
CPU 忙碌时间：6 ticks。
CPU 利用率：6 / 11 × 100% ≈ 54.55%。

- Reasoning / 理由: PID 0 先执行 4 条 CPU 指令，PID 1 随后发起 I/O。I/O 等待的 5 ticks 中，PID 0 已结束，所以 CPU 空闲。发起 I/O 和处理完成各占用 1 个 CPU tick，因此 CPU 总共忙碌 6 ticks。

- Verified result / 验证结果:总时间 11 ticks；CPU 忙碌 6 ticks；CPU 利用率 54.55%。见 q2.txt。

- Analysis / 分析:统计与预测一致。PID 0 已结束，PID 1 等待 I/O 时没有其他进程使用 CPU，因此空闲了 5 ticks。

## Q3

- Prediction / 预测:

```text
Time  PID 0        PID 1    CPU  IOs
1     RUN:io       READY    1
2     BLOCKED      RUN:cpu  1    1
3     BLOCKED      RUN:cpu  1    1
4     BLOCKED      RUN:cpu  1    1
5     BLOCKED      RUN:cpu  1    1
6     BLOCKED      DONE          1
7*    RUN:io_done  DONE     1
```

总时间：7 ticks。
CPU 忙碌时间：6 ticks。
CPU 利用率：6 / 7 × 100% ≈ 85.71%。

- Reasoning / 理由: 默认设置会在进程发起 I/O 后切换进程。因此，PID 0 等待 I/O 时，PID 1 可以执行它的 4 条 CPU 指令。CPU 只有第 6 tick 空闲。与 Q2 相比，CPU 工作和 I/O 等待发生重叠，总时间从 11 ticks 缩短到 7 ticks。

- Verified result / 验证结果:总时间 7 ticks；CPU 忙碌 6 ticks；CPU 利用率 85.71%。见 q3.txt

- Analysis / 分析:统计与预测一致。PID 1 利用了 PID 0 的 I/O 等待时间执行四条 CPU 指令，CPU 只有 1 tick 空闲。

## Q4

- Prediction / 预测:

```text
Time  PID 0        PID 1    CPU  IOs
1     RUN:io       READY    1
2     BLOCKED      READY         1
3     BLOCKED      READY         1
4     BLOCKED      READY         1
5     BLOCKED      READY         1
6     BLOCKED      READY         1
7*    RUN:io_done  READY    1
8     DONE         RUN:cpu  1
9     DONE         RUN:cpu  1
10    DONE         RUN:cpu  1
11    DONE         RUN:cpu  1
```

总时间：11 ticks。
CPU 忙碌时间：6 ticks。
CPU 利用率：6 / 11 × 100% ≈ 54.55%。

- Reasoning / 理由: SWITCH_ON_END 只在当前进程结束后切换。PID 0 等待 I/O 的第 2—6 tick，PID 1 虽然可以运行，但一直处于 READY，CPU 空闲。第 7 tick PID 0 处理 I/O 完成并结束，第 8—11 tick 才运行 PID 1。相比 Q3，总时间增加了 4 ticks。

- Verified result / 验证结果:总时间 11 ticks；CPU 忙碌 6 ticks；CPU 利用率 54.55%。见 q4.txt。

- Analysis / 分析:统计与预测一致。系统只在进程结束后切换，因此 PID 0 等待 I/O 时，PID 1 无法运行，CPU 空闲了 5 ticks。

## Q5

- Prediction / 预测:

```text
Time  PID 0        PID 1    CPU  IOs
1     RUN:io       READY    1
2     BLOCKED      RUN:cpu  1    1
3     BLOCKED      RUN:cpu  1    1
4     BLOCKED      RUN:cpu  1    1
5     BLOCKED      RUN:cpu  1    1
6     BLOCKED      DONE          1
7*    RUN:io_done  DONE     1
```

总时间：7 ticks。
CPU 忙碌时间：6 ticks。
CPU 利用率：6 / 7 × 100% ≈ 85.71%。

- Reasoning / 理由: PID 0 发起 I/O 后，系统切换到 PID 1，让它利用 I/O 等待时间执行 4 条 CPU 指令。只有第 6 tick CPU 空闲。相比 Q4，总时间从 11 ticks 减少到 7 ticks，CPU 利用率从约 54.55% 提高到约 85.71%。

- Verified result / 验证结果:总时间 7 ticks；CPU 忙碌 6 ticks；CPU 利用率 85.71%。见 q5.txt。

- Analysis / 分析:统计与预测一致。在 I/O 时切换，使 CPU 工作与 I/O 等待重叠，比 Q4 少用 4 ticks。

## Q6

- Prediction / 预测:

```text
Time  PID 0        PID 1    PID 2    PID 3    CPU  IOs
1     RUN:io       READY    READY    READY    1
2     BLOCKED      RUN:cpu  READY    READY    1    1
3     BLOCKED      RUN:cpu  READY    READY    1    1
4     BLOCKED      RUN:cpu  READY    READY    1    1
5     BLOCKED      RUN:cpu  READY    READY    1    1
6     BLOCKED      RUN:cpu  READY    READY    1    1
7*    READY        DONE     RUN:cpu  READY    1
8     READY        DONE     RUN:cpu  READY    1
9     READY        DONE     RUN:cpu  READY    1
10    READY        DONE     RUN:cpu  READY    1
11    READY        DONE     RUN:cpu  READY    1
12    READY        DONE     DONE     RUN:cpu  1
13    READY        DONE     DONE     RUN:cpu  1
14    READY        DONE     DONE     RUN:cpu  1
15    READY        DONE     DONE     RUN:cpu  1
16    READY        DONE     DONE     RUN:cpu  1
17    RUN:io_done  DONE     DONE     DONE     1
18    RUN:io       DONE     DONE     DONE     1
19    BLOCKED      DONE     DONE     DONE          1
20    BLOCKED      DONE     DONE     DONE          1
21    BLOCKED      DONE     DONE     DONE          1
22    BLOCKED      DONE     DONE     DONE          1
23    BLOCKED      DONE     DONE     DONE          1
24*   RUN:io_done  DONE     DONE     DONE     1
25    RUN:io       DONE     DONE     DONE     1
26    BLOCKED      DONE     DONE     DONE          1
27    BLOCKED      DONE     DONE     DONE          1
28    BLOCKED      DONE     DONE     DONE          1
29    BLOCKED      DONE     DONE     DONE          1
30    BLOCKED      DONE     DONE     DONE          1
31*   RUN:io_done  DONE     DONE     DONE     1
```

总时间：31 ticks。
CPU 忙碌时间：15 + 3 × 2 = 21 ticks。
CPU 利用率：21 / 31 × 100% ≈ 67.74%。

- Reasoning / 理由: IO_RUN_LATER 不会让 I/O 完成的进程立即抢占 CPU。PID 0 的第一次 I/O 在第 7 tick 完成，但直到第 17 tick 才再次运行，因此第 7—17 tick 没有 I/O 在进行。后两次 I/O 开始时，其他进程都已结束，无法利用它的等待时间，导致 CPU 共空闲 10 ticks。

- Verified result / 验证结果:总时间 31 ticks；CPU 忙碌 21 ticks；CPU 利用率 67.74%。见 q6.txt。

- Analysis / 分析:统计与预测一致。PID 0 第一次 I/O 完成后延迟运行，后两次 I/O 等待时其他进程已经结束，CPU 共空闲 10 ticks。

## Q7

- Prediction / 预测:

```text
Time  PID 0        PID 1    PID 2    PID 3    CPU  IOs
1     RUN:io       READY    READY    READY    1
2     BLOCKED      RUN:cpu  READY    READY    1    1
3     BLOCKED      RUN:cpu  READY    READY    1    1
4     BLOCKED      RUN:cpu  READY    READY    1    1
5     BLOCKED      RUN:cpu  READY    READY    1    1
6     BLOCKED      RUN:cpu  READY    READY    1    1
7*    RUN:io_done  DONE     READY    READY    1
8     RUN:io       DONE     READY    READY    1
9     BLOCKED      DONE     RUN:cpu  READY    1    1
10    BLOCKED      DONE     RUN:cpu  READY    1    1
11    BLOCKED      DONE     RUN:cpu  READY    1    1
12    BLOCKED      DONE     RUN:cpu  READY    1    1
13    BLOCKED      DONE     RUN:cpu  READY    1    1
14*   RUN:io_done  DONE     DONE     READY    1
15    RUN:io       DONE     DONE     READY    1
16    BLOCKED      DONE     DONE     RUN:cpu  1    1
17    BLOCKED      DONE     DONE     RUN:cpu  1    1
18    BLOCKED      DONE     DONE     RUN:cpu  1    1
19    BLOCKED      DONE     DONE     RUN:cpu  1    1
20    BLOCKED      DONE     DONE     RUN:cpu  1    1
21*   RUN:io_done  DONE     DONE     DONE     1
```

总时间：21 ticks。
CPU 忙碌时间：21 ticks。
CPU 利用率：21 / 21 × 100% = 100%。

- Reasoning / 理由: IO_RUN_IMMEDIATE 让 PID 0 在 I/O 完成后立即运行，从而及时发起下一次 I/O。每次等待的 5 ticks 都可以让一个 CPU 进程执行完，因此 CPU 全程没有空闲。相比 Q6，总时间从 31 ticks 减少到 21 ticks，CPU 利用率从约 67.74% 提高到 100%。

- Verified result / 验证结果:总时间 21 ticks；CPU 忙碌 21 ticks；CPU 利用率 100.00%。见 q7.txt。

- Analysis / 分析:统计与预测一致。立即运行 PID 0，使它及时发起下一次 I/O，其他进程利用等待时间执行，CPU 全程忙碌
## Q8

### Seed 1

指令列表：
- PID 0: cpu → io → io_done → io → io_done
- PID 1: cpu → cpu → cpu

#### Default

- Prediction / 预测:

```text
Time  PID 0        PID 1    CPU  IOs
1     RUN:cpu      READY    1
2     RUN:io       READY    1
3     BLOCKED      RUN:cpu  1    1
4     BLOCKED      RUN:cpu  1    1
5     BLOCKED      RUN:cpu  1    1
6     BLOCKED      DONE          1
7     BLOCKED      DONE          1
8*    RUN:io_done  DONE     1
9     RUN:io       DONE     1
10    BLOCKED      DONE          1
11    BLOCKED      DONE          1
12    BLOCKED      DONE          1
13    BLOCKED      DONE          1
14    BLOCKED      DONE          1
15*   RUN:io_done  DONE     1
```

总时间：15 ticks。
CPU 忙碌时间：8 ticks。
CPU 利用率：8 / 15 × 100% ≈ 53.33%。

- Reasoning / 理由: PID 1 的三条 CPU 指令可以在 PID 0 第一次等待 I/O 时执行。第一次 I/O 在第 8 tick 完成，此时 PID 1 已结束，PID 0 可以继续运行。第二次 I/O 等待期间没有其他可运行进程，CPU 空闲。

- Verified result / 验证结果:总时间 15 ticks；CPU 忙碌 8 ticks；CPU 利用率 53.33%。见 q8-s1.txt 的 default 部分。

- Analysis / 分析:统计与预测一致。PID 1 的三条 CPU 指令在 PID 0 第一次等待 I/O 时执行，第二次等待时没有其他进程可运行。

#### IO_RUN_IMMEDIATE

- Prediction / 预测: 逐 tick 状态表与本种子的 Default 表相同。总时间为 15 ticks，CPU 忙碌 8 ticks，CPU 利用率约 53.33%。

- Reasoning / 理由: 第一次 I/O 完成时，PID 1 已经结束，因此立即运行 PID 0 不会改变执行顺序。第二次 I/O 完成时也只有 PID 0，所以预测与默认设置相同。

- Verified result / 验证结果:总时间 15 ticks；CPU 忙碌 8 ticks；CPU 利用率 53.33%。见 q8-s1.txt 的 IO_RUN_IMMEDIATE 部分。

- Analysis / 分析:统计与预测一致。I/O 完成时 PID 1 已结束，因此立即运行 PID 0 不会改变执行顺序，结果与默认设置相同。

#### SWITCH_ON_END

- Prediction / 预测:

```text
Time  PID 0        PID 1    CPU  IOs
1     RUN:cpu      READY    1
2     RUN:io       READY    1
3     BLOCKED      READY         1
4     BLOCKED      READY         1
5     BLOCKED      READY         1
6     BLOCKED      READY         1
7     BLOCKED      READY         1
8*    RUN:io_done  READY    1
9     RUN:io       READY    1
10    BLOCKED      READY         1
11    BLOCKED      READY         1
12    BLOCKED      READY         1
13    BLOCKED      READY         1
14    BLOCKED      READY         1
15*   RUN:io_done  READY    1
16    DONE         RUN:cpu  1
17    DONE         RUN:cpu  1
18    DONE         RUN:cpu  1
```

总时间：18 ticks。
CPU 忙碌时间：8 ticks。
CPU 利用率：8 / 18 × 100% ≈ 44.44%。

- Reasoning / 理由: 系统必须等 PID 0 完全结束后才切换到 PID 1。因此两次 I/O 等待期间 CPU 都空闲，PID 1 的三条 CPU 指令只能在第 16—18 tick 执行。相比默认设置，总时间增加了 3 ticks。

- Verified result / 验证结果:总时间 18 ticks；CPU 忙碌 8 ticks；CPU 利用率 44.44%。见 q8-s1.txt 的 SWITCH_ON_END 部分。

- Analysis / 分析:统计与预测一致。PID 1 必须等待 PID 0 结束，无法利用 I/O 等待时间，因此总时间比默认设置增加 3 ticks。
### Seed 2

指令列表：
- PID 0: io → io_done → io → io_done → cpu
- PID 1: cpu → io → io_done → io → io_done

#### Default

- Prediction / 预测:

```text
Time  PID 0        PID 1        CPU  IOs
1     RUN:io       READY        1
2     BLOCKED      RUN:cpu      1    1
3     BLOCKED      RUN:io       1    1
4     BLOCKED      BLOCKED           2
5     BLOCKED      BLOCKED           2
6     BLOCKED      BLOCKED           2
7*    RUN:io_done  BLOCKED      1    1
8     RUN:io       BLOCKED      1    1
9*    BLOCKED      RUN:io_done  1    1
10    BLOCKED      RUN:io       1    1
11    BLOCKED      BLOCKED           2
12    BLOCKED      BLOCKED           2
13    BLOCKED      BLOCKED           2
14*   RUN:io_done  BLOCKED      1    1
15    RUN:cpu      BLOCKED      1    1
16*   DONE         RUN:io_done  1
```

总时间：16 ticks。
CPU 忙碌时间：10 ticks。
CPU 利用率：10 / 16 × 100% = 62.50%。

- Reasoning / 理由: PID 0 发起 I/O 后，PID 1 可以执行 CPU 指令并发起自己的 I/O。两个进程的 I/O 等待时间发生重叠。但第 4—6 tick 和第 11—13 tick，两者都在等待，因此 CPU 共空闲 6 ticks。每次 I/O 完成时，另一个进程都无法运行，所以完成 I/O 的进程可以继续执行。

- Verified result / 验证结果:总时间 16 ticks；CPU 忙碌 10 ticks；CPU 利用率 62.50%。见 q8-s2.txt 的 default 部分。

- Analysis / 分析:统计与预测一致。两个进程的 I/O 等待时间可以重叠，但两者同时阻塞时 CPU 仍然空闲，共空闲 6 ticks。

#### IO_RUN_IMMEDIATE

- Prediction / 预测: 逐 tick 状态表与本种子的 Default 表相同。总时间为 16 ticks，CPU 忙碌 10 ticks，CPU 利用率为 62.50%。

- Reasoning / 理由: 第 7、9、14 tick 有 I/O 完成时，另一个进程仍处于 BLOCKED；第 16 tick PID 0 已结束。因此没有正在运行的其他进程需要被抢占，立即运行设置不会改变本次执行顺序。

- Verified result / 验证结果:总时间 16 ticks；CPU 忙碌 10 ticks；CPU 利用率 62.50%。见 q8-s2.txt 的 IO_RUN_IMMEDIATE 部分。

- Analysis / 分析:统计与预测一致。每次 I/O 完成时，另一个进程仍在阻塞或已经结束，因此立即运行不会改变执行顺序。

#### SWITCH_ON_END

- Prediction / 预测:

```text
Time  PID 0        PID 1        CPU  IOs
1     RUN:io       READY        1
2     BLOCKED      READY             1
3     BLOCKED      READY             1
4     BLOCKED      READY             1
5     BLOCKED      READY             1
6     BLOCKED      READY             1
7*    RUN:io_done  READY        1
8     RUN:io       READY        1
9     BLOCKED      READY             1
10    BLOCKED      READY             1
11    BLOCKED      READY             1
12    BLOCKED      READY             1
13    BLOCKED      READY             1
14*   RUN:io_done  READY        1
15    RUN:cpu      READY        1
16    DONE         RUN:cpu      1
17    DONE         RUN:io       1
18    DONE         BLOCKED           1
19    DONE         BLOCKED           1
20    DONE         BLOCKED           1
21    DONE         BLOCKED           1
22    DONE         BLOCKED           1
23*   DONE         RUN:io_done  1
24    DONE         RUN:io       1
25    DONE         BLOCKED           1
26    DONE         BLOCKED           1
27    DONE         BLOCKED           1
28    DONE         BLOCKED           1
29    DONE         BLOCKED           1
30*   DONE         RUN:io_done  1
```

总时间：30 ticks。
CPU 忙碌时间：10 ticks。
CPU 利用率：10 / 30 × 100% ≈ 33.33%。

- Reasoning / 理由: PID 0 完全结束之后，PID 1 才能开始运行。四次 I/O 的等待时间都不能被其他进程利用，因此 CPU 共空闲 20 ticks。CPU 忙碌时间仍为 10 ticks，但总时间比默认设置增加了 14 ticks。

- Verified result / 验证结果:总时间 30 ticks；CPU 忙碌 10 ticks；CPU 利用率 33.33%。见 q8-s2.txt 的 SWITCH_ON_END 部分。

- Analysis / 分析:统计与预测一致。两个进程依次完成，四次 I/O 等待无法重叠，CPU 共空闲 20 ticks，总时间比默认设置增加 14 ticks。

### Seed 3

指令列表：
- PID 0: cpu → io → io_done → cpu
- PID 1: io → io_done → io → io_done → cpu

#### Default

- Prediction / 预测:

```text
Time  PID 0        PID 1        CPU  IOs
1     RUN:cpu      READY        1
2     RUN:io       READY        1
3     BLOCKED      RUN:io       1    1
4     BLOCKED      BLOCKED           2
5     BLOCKED      BLOCKED           2
6     BLOCKED      BLOCKED           2
7     BLOCKED      BLOCKED           2
8*    RUN:io_done  BLOCKED      1    1
9*    RUN:cpu      READY        1
10    DONE         RUN:io_done  1
11    DONE         RUN:io       1
12    DONE         BLOCKED           1
13    DONE         BLOCKED           1
14    DONE         BLOCKED           1
15    DONE         BLOCKED           1
16    DONE         BLOCKED           1
17*   DONE         RUN:io_done  1
18    DONE         RUN:cpu      1
```

总时间：18 ticks。
CPU 忙碌时间：9 ticks。
CPU 利用率：9 / 18 × 100% = 50.00%。

- Reasoning / 理由: 第 9 tick PID 1 的第一次 I/O 完成，但默认设置不会抢占正在运行的 PID 0。PID 0 先执行最后一条 CPU 指令，PID 1 在第 10 tick 才处理完成，第 11 tick 发起第二次 I/O。第二次 I/O 等待期间，PID 0 已结束，CPU 空闲。

- Verified result / 验证结果: 总时间 18 ticks；CPU 忙碌 9 ticks；CPU 利用率 50.00%。见 q8-s3.txt 的 default 部分。

- Analysis / 分析: 统计与预测一致。第 9 tick PID 1 的 I/O 完成后先等待，PID 0 执行最后一条 CPU 指令，导致 PID 1 的第二次 I/O 较晚开始。

#### IO_RUN_IMMEDIATE

- Prediction / 预测:

```text
Time  PID 0        PID 1        CPU  IOs
1     RUN:cpu      READY        1
2     RUN:io       READY        1
3     BLOCKED      RUN:io       1    1
4     BLOCKED      BLOCKED           2
5     BLOCKED      BLOCKED           2
6     BLOCKED      BLOCKED           2
7     BLOCKED      BLOCKED           2
8*    RUN:io_done  BLOCKED      1    1
9*    READY        RUN:io_done  1
10    READY        RUN:io       1
11    RUN:cpu      BLOCKED      1    1
12    DONE         BLOCKED           1
13    DONE         BLOCKED           1
14    DONE         BLOCKED           1
15    DONE         BLOCKED           1
16*   DONE         RUN:io_done  1
17    DONE         RUN:cpu      1
```

总时间：17 ticks。
CPU 忙碌时间：9 ticks。
CPU 利用率：9 / 17 × 100% ≈ 52.94%。

- Reasoning / 理由: 第 9 tick PID 1 的 I/O 完成后立即运行，PID 0 暂时变为 READY。PID 1 在第 10 tick 发起第二次 I/O，PID 0 的最后一条 CPU 指令可以在第 11 tick 的 I/O 等待期间执行。因此比默认设置少用 1 tick。

- Verified result / 验证结果: 总时间 17 ticks；CPU 忙碌 9 ticks；CPU 利用率 52.94%。见 q8-s3.txt 的 IO_RUN_IMMEDIATE 部分。

- Analysis / 分析: 统计与预测一致。PID 1 在第 9 tick 立即运行，第 10 tick 发起第二次 I/O。PID 0 的最后一条 CPU 指令在等待期间执行，比默认设置少用 1 tick。

#### SWITCH_ON_END

- Prediction / 预测:

```text
Time  PID 0        PID 1        CPU  IOs
1     RUN:cpu      READY        1
2     RUN:io       READY        1
3     BLOCKED      READY             1
4     BLOCKED      READY             1
5     BLOCKED      READY             1
6     BLOCKED      READY             1
7     BLOCKED      READY             1
8*    RUN:io_done  READY        1
9     RUN:cpu      READY        1
10    DONE         RUN:io       1
11    DONE         BLOCKED           1
12    DONE         BLOCKED           1
13    DONE         BLOCKED           1
14    DONE         BLOCKED           1
15    DONE         BLOCKED           1
16*   DONE         RUN:io_done  1
17    DONE         RUN:io       1
18    DONE         BLOCKED           1
19    DONE         BLOCKED           1
20    DONE         BLOCKED           1
21    DONE         BLOCKED           1
22    DONE         BLOCKED           1
23*   DONE         RUN:io_done  1
24    DONE         RUN:cpu      1
```

总时间：24 ticks。
CPU 忙碌时间：9 ticks。
CPU 利用率：9 / 24 × 100% = 37.50%。

- Reasoning / 理由: PID 0 完全结束后才运行 PID 1，三个 I/O 的等待时间都无法与其他进程的工作重叠。CPU 共空闲 15 ticks，加上 9 个忙碌 ticks，总时间为 24 ticks。

- Verified result / 验证结果:总时间 24 ticks；CPU 忙碌 9 ticks；CPU 利用率 37.50%。见 q8-s3.txt 的 SWITCH_ON_END 部分。

- Analysis / 分析:统计与预测一致。PID 0 完全结束后才运行 PID 1，三次 I/O 等待都无法利用其他进程的工作来填补，CPU 共空闲 15 ticks。