数据结构运用
这是一道经典的数据结构运用题。

具体的，我们可以使用两个优先队列（堆）来维护整个数据流数据，令维护数据流左半边数据的优先队列（堆）为 l，维护数据流右半边数据的优先队列（堆）为 r。

显然，为了可以在 O(1)O(1) 的复杂度内取得当前中位数，我们应当令 l 为大根堆，r 为小根堆，并人为固定 l 和 r 之前存在如下的大小关系：

当数据流元素数量为偶数：l 和 r 大小相同，此时动态中位数为两者堆顶元素的平均值；
当数据流元素数量为奇数：l 比 r 多一，此时动态中位数为 l 的堆顶原数。
为了满足上述说的奇偶性堆大小关系，在进行 addNum 时，我们应当分情况处理：

插入前两者大小相同，说明插入前数据流元素个数为偶数，插入后变为奇数。我们期望操作完达到「l 的数量为 r 多一，同时双堆维持有序」，进一步分情况讨论：

如果 r 为空，说明当前插入的是首个元素，直接添加到 l 即可；
如果 r 不为空，且 num <= r.peek()，说明 num 的插入位置不会在后半部分（不会在 r 中），直接加到 l 即可；
如果 r 不为空，且 num > r.peek()，说明 num 的插入位置在后半部分，此时将 r 的堆顶元素放到 l 中，再把 num 放到 r（相当于从 r 中置换一位出来放到 l 中）。
插入前两者大小不同，说明前数据流元素个数为奇数，插入后变为偶数。我们期望操作完达到「l 和 r 数量相等，同时双堆维持有序」，进一步分情况讨论（此时 l 必然比 r 元素多一）：

如果 num >= l.peek()，说明 num 的插入位置不会在前半部分（不会在 l 中），直接添加到 r 即可。
如果 num < l.peek()，说明 num 的插入位置在前半部分，此时将 l 的堆顶元素放到 r 中，再把 num 放入 l 中（相等于从 l 中替换一位出来当到 r 中）。
代码：

Java

class MedianFinder {
    PriorityQueue<Integer> l = new PriorityQueue<>((a,b)->b-a);
    PriorityQueue<Integer> r = new PriorityQueue<>((a,b)->a-b);
    
    public void addNum(int num) {
        int s1 = l.size(), s2 = r.size();
        if (s1 == s2) {
            if (r.isEmpty() || num <= r.peek()) {
                l.add(num);
            } else {
                l.add(r.poll());
                r.add(num);
            }
        } else {
            if (l.peek() <= num) {
                r.add(num);
            } else {
                r.add(l.poll());
                l.add(num);
            }
        }
    }
    
    public double findMedian() {
        int s1 = l.size(), s2 = r.size();
        if (s1 == s2) {
            return (l.peek() + r.peek()) / 2.0;
        } else {
            return l.peek();
        }
    }
}
时间复杂度：addNum 函数的复杂度为 O(\log{n})O(logn)；findMedian 函数的复杂度为 O(1)O(1)
空间复杂度：O(n)O(n)

作者：AC_OIer
链接：https://leetcode-cn.com/problems/find-median-from-data-stream/solution/gong-shui-san-xie-jing-dian-shu-ju-jie-g-pqy8/
来源：力扣（LeetCode）
