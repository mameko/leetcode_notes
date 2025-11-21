# 6 其他常用snipets

## Comparator :

参照 [56 Merge Intervals](../my-notes/intervals/56-merge-intervals.md)， 其实还能利用java 8的lambda写，更简洁，参照 [767 Reorganize String](../my-notes/others/767-reorganize-string.md)

```java
Collections.sort(intervals, new Comparator<Interval>(){
    public int compare(Interval i1, Interval i2) {
        return i1.start - i2.start;
    }
});

// lambda的写法，但这syntectic sugar好像有点慢。
PriorityQueue<Map.Entry<Character, Integer>> pq = new PriorityQueue<>((e1, e2) -> e2.getValue() - e1.getValue());
```

## Implement Comparable:

```java
class Pair implements Comparable<Pair> {
    int time;
    boolean isStart;

    public Pair(int t, boolean start) {
        time = t;
        isStart = start;
    }

    public int compareTo(Pair p2) {
        return this.time - p2.time;
    }
}
```

## Loop HashMap :

```java
HashMap<A, B> map = ...
for(Map.Entry<A, B> entry : map.entrySet()) {
    entry.getKey()
    entry.getValue()
}
```

## Reverse：

```java
private void reverse(int start, int end, ArrayList<Integer> nums) {
    for (int i = start, j = end; i < j; i++, j--) {
        int tmp = nums.get(i);
        nums.set(i, nums.get(j));
        nums.set(j, tmp);
    }
}
```

## Override equals & hashcode

```java
class Node {
    Integer curi;
    Integer curj;
    Node prevNode;

    public Node(Integer ci, Integer cj, Node prev) {
        curi = ci;
        curj = cj;
        prevNode = prev;
    }

    public int hashCode() {
        return curi;
    }

    public boolean equals(Object obj) {
        boolean flag = false;
        Node n = (Node) obj;
        if (n.curi == curi && n.curj == curj) {
            flag = true;
        }
        return flag;
    }
}
```

## n^2 check Palindrome:

```java
private void isPar( dp[len][len] ) {
    for (i : 0 ~ <len) {
        [i][j] = T;
    }

    for (i : 0 ~ <len) {
        [i][i + 1] = (char(i) == chat(i + 1))
    }

    for (l = 2; l < len; l++) {
        for (start = 0; start + l < len; start++) {
            [s][s + l] = [s + 1][s + l - 1] //左下，中间是否palindrome
                         && (char(s) == char(s + l))// 这两个是否相同
        }
    }
}

// -------------------- OR -----------------------------

for (int i = 0; i < n; i++) {
    for (int j = 0; j <= i; j++) {
        isPal[i][j] = s.charAt(i) == s.charAt(j) && (i <= j + 1 || isPal[i - 1][j + 1]);
    }
}
```

## BFS:

```java
void Search(Node root) {
    Queue<...> q = new LinkedList<>();
    root.visited = true;
    q.enqueue(root);
    while (!q.isEmpty()) {
        for ( size ) {
             Node r = q.poll();
             // do something with r
             visit(r);
             for each (n in r's neighour) {
             if (n.visited == false) {
                  n.visited = true;
                  q.offer(n);
             }
         }
     }
}

// leetcode模板
/**
 * Return the length of the shortest path between root and target node.
 */
int BFS(Node root, Node target) {
    Queue<Node> queue;  // store all nodes which are waiting to be processed
    Set<Node> used;     // store all the used nodes
    int step = 0;       // number of steps neeeded from root to current node
    // initialize
    add root to queue;
    add root to used;
    // BFS
    while (queue is not empty) {
        step = step + 1;
        // iterate the nodes which are already in the queue
        int size = queue.size();
        for (int i = 0; i < size; ++i) {
            Node cur = the first node in queue;
            return step if cur is target;
            for (Node next : the neighbors of cur) {
                if (next is not in used) {
                    add next to queue;
                    add next to used;
                }
            }
            remove the first node from queue;
        }
    }
    return -1;          // there is no path from root to target
}
```

There are two cases you don't need the hash set `used:`

1. You are absolutely sure there is no cycle, for example, in tree traversal;
2. You do want to add the node to the queue multiple times.

## DFS:

```java
void Search(Node root) {
    if (root == null) {
        return;
    }

    // do something with root
    visit(root);
    visited = true;

    for each (n in root's neighour) {
        if (n.visited == false) {
            search(n);
        }
    }
}

// leetcode模板

/*
 * Return true if there is a path from cur to target.
 */
boolean DFS(int root, int target) {
    Set<Node> visited;
    Stack<Node> s;
    add root to s;
    while (s is not empty) {
        Node cur = the top element in s;
        return true if cur is target;
        for (Node next : the neighbors of cur) {
            if (next is not in visited) {
                add next to s;
                add next to visited;
            }
        }
        remove cur from s;
    }
    return false;
}
```

## Sliding Window:

```java
cnt = t.len, left = 0, targetMap, min/max
for (right 0 ~ s.len) {
    //expand right
    if (tm(rc) >= 1) {
        cnt--/++
    }
    tm(rc)--;

    if (cnt==0) {// satisfy condition, do something here
    }

    // shrink left
    if contains {
        if (tm.(lc) >= 0) {
            cnt++/--;
        }
        tm(lc)++;
    }
}

// leetcode 模板

/*
 * Return true if there is a path from cur to target.
 */
//系统栈
boolean DFS(Node cur, Node target, Set<Node> visited) {
    return true if cur is target;
    for (next : each neighbor of cur) {
        if (next is not in visited) {
            add next to visted;
            return true if DFS(next, target, visited) == true;
        }
    }
    return false;
}

//自己维护栈
boolean DFS(int root, int target) {
    Set<Node> visited;
    Stack<Node> s;
    add root to s;
    while (s is not empty) {
        Node cur = the top element in s;
        return true if cur is target;
        for (Node next : the neighbors of cur) {
            if (next is not in visited) {
                add next to s;
                add next to visited;
            }
        }
        remove cur from s;
    }
    return false;
}
```

## 2 sum 类模板（对撞型two pointer）

```java
// 给一个数组A
int left = 0;
int right = nums.length - 1;
while (left < right) {
    if (A[left] 和 A[right]满足一个条件) {
        // 做一些事情
        right--; // 不考虑[left + 1, right - 1]和right组成的pair
    } else {
        left++; // 不考虑[left + 1, right - 1]和left组成的pair
    }
}
```

## 窗口类指针移动模板

```java
// 通过两重for循环改进算法
for (i = 0; i < n; i++) {
    while (j < n) {
        if (满足条件) {
            j++;
            //更新j状态
        } else (不满足条件) {
            break;
        }
    }
    //更新i状态
}
```

## 同向双指针模板

```
j = 0 or j = 1
for i from 0 to (n - 1)
    while j < n and (i, j的搭配不满足条件）
        j += 1
    if （i, j的搭配满足条件）
        处理i，j的这次搭配
    
```
