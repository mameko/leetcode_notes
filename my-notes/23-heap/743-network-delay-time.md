# 743 Network Delay Time

There are`N`network nodes, labelled`1`to`N`.

Given`times`, a list of travel times as **directed** edges`times[i] = (u, v, w)`, where`u`is the source node,`v`is the target node, and`w`is the time it takes for a signal to travel from source to target.

Now, we send a signal from a certain node`K`. How long will it take for all nodes to receive the signal? If it is impossible, return`-1`.

**Note:**

1. `N`will be in the range`[1, 100]`.
2. `K`will be in the range`[1, N]`.
3. The length of`times`will be in the range`[1, 6000]`.
4. All edges`times[i] = (u, v, w)`will have`1 <= u, v <= N`and`1 <= w <= 100`.

一开始看到这题写着easy所以就立刻做了，然而，做了半天没做对。大概方法是相对了，但实现有问题。估计一开始存的方法不方便，所以heap没写好。（存成邻接表，然后neighbor用了hashmap\<nei，距离>)然后看了答案才知道，这题用的是Dijkastras...卧槽...最后写了半天还是没写对，就看了答案了。具体看注释。

```java
public int networkDelayTime(int[][] times, int N, int K) {
    if (times == null || times.length == 0 || K < 0 || N < 0) {
        return -1;
    }

    // <n1, list<nei, dist>>
    Map<Integer, List<int[]>> graph = new HashMap<>();
    for (int[] edge : times) {
        int n1 = edge[0];
        int n2 = edge[1];
        int dist = edge[2];
        if (!graph.containsKey(n1)) {
            graph.put(n1, new ArrayList<>());
        }
        graph.get(n1).add(new int[]{n2, dist});
    }

    // make a min heap to quickly find the min cost edge
    PriorityQueue<int[]> pq = new PriorityQueue<>(
        (e1, e2) -> (e1[0] - e2[0])
    );

    // starting node's dist to itself is 0
    pq.offer(new int[]{0, K});

    // <node, dist>
    Map<Integer, Integer> visitedAndDist = new HashMap<>();

    while (!pq.isEmpty()) {
        int[] edge = pq.poll();
        int dist = edge[0];
        int node = edge[1];            
        // visited
        if (visitedAndDist.containsKey(node)) {
            continue;
        }
        visitedAndDist.put(node, dist);

        // this node doesn't have neighbour
        if (!graph.containsKey(node)) {
            continue;
        }

        // process this node's neighbour
        for (int[] e : graph.get(node)) {
            int nei = e[0];
            int distToNei = e[1];
            // not visited yet
            if (!visitedAndDist.containsKey(nei)) {
                pq.offer(new int[]{dist + distToNei, nei});
            }
        }            
    }

    // cannot reach some of the nodes
    if (visitedAndDist.size() != N) {
        return -1;
    }

    // find max
    int max = 0;
    for (int d : visitedAndDist.values()) {
        max = Math.max(max, d);
    }

    return max;
}
```

另外看到人家用邻接表写，好像挺快的，抄下来作为参考.

```java
public int networkDelayTime(int[][] times, int N, int K) {
         int[][] graph = new int[N+1][N+1];
    for (int i = 0; i < graph.length; i++) {
            for (int j = 0; j < graph[i].length; j++) {
                graph[i][j] = Integer.MAX_VALUE;
            }
        }
    for (int[] edge: times) {
        graph[edge[0]][edge[1]]=edge[2];
    }
    int[] dist=new int[N+1];
    boolean[] seen=new boolean[N+1];

    for(int n=0;n<dist.length;++n) {
        dist[n]=Integer.MAX_VALUE;
    }
    dist[K]=0;

    while(true) {
        int candi=-1;
        int candiDist=Integer.MAX_VALUE;
        for(int i=1;i<=N;i++) {
            if(!seen[i]&& dist[i]<candiDist){
                candi=i;
                candiDist=dist[i];
            }
        }
        if(candi==-1)
            break;
        seen[candi]=true;

        for(int v=1;v<=N;++v){
            if(!seen[v]&&graph[candi][v]!=Integer.MAX_VALUE&&dist[candi]+graph[candi][v]<dist[v])
                dist[v]=dist[candi]+graph[candi][v];
        }

    }

    int ans=0;
    // for(int cand:dist){
    //     if(cand==Integer.MAX_VALUE)
    //         return -1;
    //     ans=Math.max(ans,cand);
    // }
    for(int i=1;i<=N;i++){
        if(dist[i]==Integer.MAX_VALUE)
            return -1;
        ans=Math.max(ans,dist[i]);
    }
    return ans;
}
```
