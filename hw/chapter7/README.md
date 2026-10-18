# Chapter 7：CPU 调度实验

本实验使用 OSTEP 的 scheduler.py，比较 FIFO、SJF 和 RR
在不同任务长度、输入顺序和时间片下的响应时间与周转时间。

## 文件说明
- [analysis.md](analysis.md)：Q1–Q7 的理论计算、实验验证和分析。
- [data/](data/)：17 个模拟器原始输出文件。

## 实验环境与方法
- 环境：GitHub Codespaces、Python 3。
- 模拟器：ext/ostep-homework/cpu-sched/scheduler.py。
- 所有任务在时间 0 到达，不计上下文切换开销。
- 先进行理论计算，再使用 -c 参数验证。
- 使用 -l 指定任务长度，使用 -p 选择算法，使用 -q 设置 RR 时间片。
- 各实验参数和结果见 analysis.md。

## 主要结论
1. FIFO 队列已按任务长度升序排列时，SJF 与 FIFO 的周转时间相同。
2. 对本实验的两组任务，RR 使用小时间片改善响应时间，
   但增加了平均周转时间。
3. RR 队列按长度排序且时间片足够大时，可以获得与 SJF 相同的响应时间。
4. 所有任务长度按相同比例增加时，SJF 平均响应时间也按比例增加。
5. 对同时到达的 N 个任务，不计切换开销时，
   RR 最坏首次响应时间的上界为 (N-1)q。

## 参考资料
- [OSTEP 第七章](https://pages.cs.wisc.edu/~remzi/OSTEP/cpu-sched.pdf)
- [官方模拟器](https://github.com/remzi-arpacidusseau/ostep-homework/tree/master/cpu-sched)