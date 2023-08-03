# bisect 模块

| 名称               | 功能                                           |
| ------------------ | ---------------------------------------------- |
| **bisect_left()**  | **查找 ** 目标元素左侧插入点                   |
| **bisect_right()** | **查找** 目标元素右侧插入点                    |
| **bisect()**       | 同 bisect_right()                              |
| **insort_left()**  | 查找目标元素左侧插入点，并保序地 **插入** 元素 |
| **insort_right()** | 查找目标元素右侧插入点，并保序地 **插入** 元素 |
| **insort()**       | 同 insort_right()                              |

注意，使用 bisect 模块的方法之前，须确保待操作对象是 **有序序列**，即元素已按 **从大到小 / 从小到大** 的顺序排列

https://blog.csdn.net/qq_39478403/article/details/105373620

https://docs.python.org/zh-cn/3.6/library/bisect.html?highlight=bisect#module-bisect

- `bisect.bisect_left(a, x, lo=0, hi=len(a))`

  在 *a* 中找到 *x* 合适的插入点以维持有序。参数 *lo* 和 *hi* 可以被用于确定需要考虑的子集；默认情况下整个列表都会被使用。如果 *x* 已经在 *a* 里存在，那么插入点会在已存在元素之前（也就是左边）。如果 *a* 是列表（list）的话，返回值是可以被放在 `list.insert()` 的第一个参数的。返回的插入点 *i* 可以将数组 *a* 分成两部分。左侧是 `all(val < x for val in a[lo:i])` ，右侧是 `all(val >= x for val in a[i:hi])` 。

- `bisect.bisect_right(a, x, lo=0, hi=len(a))`

- `bisect.bisect(a, x, lo=0, hi=len(a))`

  类似于 [`bisect_left()`](https://docs.python.org/zh-cn/3.6/library/bisect.html?highlight=bisect#bisect.bisect_left)，但是返回的插入点是 *a* 中已存在元素 *x* 的右侧。返回的插入点 *i* 可以将数组 *a* 分成两部分。左侧是 `all(val <= x for val in a[lo:i])`，右侧是 `all(val > x for val in a[i:hi])` for the right side。

- `bisect.insort_left(a, x, lo=0, hi=len(a))`

  将 *x* 插入到一个有序序列 *a* 里，并维持其有序。如果 *a* 有序的话，这相当于 `a.insert(bisect.bisect_left(a, x, lo, hi), x)`。要注意搜索是 O(log n) 的，插入却是 O(n) 的。

- `bisect.insort_right(a, x, lo=0, hi=len(a))`

- `bisect.insort(a, x, lo=0, hi=len(a))`

  类似于 [`insort_left()`](https://docs.python.org/zh-cn/3.6/library/bisect.html?highlight=bisect#bisect.insort_left)，但是把 *x* 插入到 *a* 中已存在元素 *x* 的右侧。

