# L526 Load Balancer

## L526 Load Balancer (simpler version of 380)

## Description

Implement a load balancer for web servers. It provide the following functionality:

1. Add a new server to the cluster =>`add(server_id)`.
2. Remove a bad server from the cluster =>`remove(server_id)`.
3. Pick a server in the cluster randomly with equal probability =>`pick()`.

## Example

At beginning, the cluster is empty =>`{}`.

```
add(1)
add(2)
add(3)
pick()
>> 1         // the return value is random, it can be either 1, 2, or 3.
pick()
>> 2
pick()
>> 1
pick()
>> 3
remove(1)
pick()
>> 2
pick()
>> 3
pick()
>> 3
```

这题是低配版的380，做法一样，而且不用判断重复，主要记住怎么set arraylist和random的用法。

```java
public class LoadBalancer {
    HashMap<Integer, Integer> sevToLoc;
    List<Integer> servers;
    public LoadBalancer() {
        sevToLoc = new HashMap<>();
        servers = new ArrayList<>();
    }

    /*
     * @param server_id: add a new server to the cluster
     * @return: nothing
     */
    public void add(int server_id) {
        sevToLoc.put(server_id, servers.size());
        servers.add(server_id);
    }

    /*
     * @param server_id: server_id remove a bad server from the cluster
     * @return: nothing
     */
    public void remove(int server_id) {
        // write your code here
        if (sevToLoc.containsKey(server_id)) {
            int loc = sevToLoc.get(server_id);
            if (loc != servers.size() - 1) { // 与最后一个交换，删除得比较快
                int lastServer = servers.get(servers.size() - 1);
                sevToLoc.put(lastServer, loc);
                servers.set(loc, lastServer);
            }
            servers.remove(servers.size() - 1);
        }
    }

    /*
     * @return: pick a server in the cluster randomly with equal probability
     */
    public int pick() {
        // write your code here
        Random r = new Random();
        int loc = r.nextInt(servers.size());
        return servers.get(loc);
    }
}
```
