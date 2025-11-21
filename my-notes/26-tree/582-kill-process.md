# 582 Kill Process

Given **n** processes, each process has a unique **PID (process id)**&#x61;nd its **PPID (parent process id)**.

Each process only has one parent process, but may have one or more children processes. This is just like a tree structure. Only one process has PPID that is 0, which means this process has no parent process. All the PIDs will be distinct positive integers.

We use two list of integers to represent a list of processes, where the first list contains PID for each process and the second list contains the corresponding PPID.

Now given the two lists, and a PID representing a process you want to kill, return a list of PIDs of processes that will be killed in the end. You should assume that when a process is killed, all its children processes will be killed. No order is required for the final answer.

**Example 1:**

```
Input: 
pid =  [1, 3, 10, 5]
ppid = [3, 0, 5, 3]
kill = 5

Output: [5,10]

Explanation: 
           3
         /   \
        1     5
             /
            10
Kill 5 will also kill 10.
```

**Note:**

1. The given kill id is guaranteed to be one of the given PIDs.
2. n >= 1.

感觉这题很像那条[limebike](../mian-jing/file-access.md)的题，本质是树的遍历，但套了一个马甲。之前套了文件呀，员工什么的，这里是套process。嘛，还是那样，其实不用建树，只要用map就可以了，而且感觉还快点呢。（今天才想到可以用bfs，之前[file access](../mian-jing/file-access.md)用的是dfs）T:O(n)最坏情况下kill了0的那个，要整棵树都返回， 虽然做了点优化但你懂的。 S:O(n)，hashmap和queue都花了这么多空间。

```java
public List<Integer> killProcess(List<Integer> pid, List<Integer> ppid, int kill) {
    List<Integer> res = new ArrayList<>();
    // 其实这里还可以判断如果ppid和pid的size不一样，因为下面的加map里的loop假设了它两的长度一样
    if (pid == null || pid.size() == 0 || ppid == null || ppid.size() == 0 || kill < 0) {
        return res;
    }

    if (kill == 0) {
        return pid;
    }

    // <ppid, List<pid>>，存起对应关系
    Map<Integer, List<Integer>> pMap = new HashMap<>();

    for (int i = 0; i < pid.size(); i++) {
        pMap.putIfAbsent(ppid.get(i), new ArrayList<>());
        pMap.get(ppid.get(i)).add(pid.get(i));
    }

    // 遍历找subtree
    Queue<Integer> queue = new LinkedList<>();        
    queue.offer(kill);
    while (!queue.isEmpty()) {
        int cur = queue.poll();            
        res.add(cur);
        if (pMap.containsKey(cur)) {
            queue.addAll(pMap.get(cur));
        }
    }

    return res;
}
```
