# L391 Number of Airplanes in the Sky

Given an interval list which are flying and landing time of the flight. How many airplanes are on the sky at most?

## Notice

If landing and flying happens at the same time, we consider landing should happen at first.

Have you met this question in a real interview?

Yes

**Example**

For interval list

```
[
  [1,10],
  [2,3],
  [5,8],
  [4,7]
]
```

Return`3`

这题因为range和n的数目不定，所以很难比较下面两个算法哪个快。

我的方法比较像range addition，要花比较多空间，不过不用排序，所以花O(n)时间，O(range)的空间。

```java
public int countOfAirplanes(List<Interval> airplanes) {
    if (airplanes == null || airplanes.size() == 0) {
        return 0;
    }
    int last = 0;
    for (Interval interval : airplanes) {
        last = Math.max(last, interval.end);
    }

    int[] res = new int[last + 2];
    for (Interval in : airplanes) {
        res[in.start]++;
        res[in.end]--;
    }

    int max = 0;
    int runningSum = 0;
    for (int i = 1; i < last + 2; i++) {
        runningSum += res[i];
        max = Math.max(runningSum, max);
    }

    return max;
}
```

下面是九章的解法：把interval拆成起点和终点，排序。按照时间先后顺序排，ascending，当时间相等时起点先于终点。遇到起点++，遇到终点--。时间O(nlogn)，空间花O(n)，因为另外存了个数组来排序。

```java
class Point{
    int time;
    int flag;

    Point(int t, int s){
      this.time = t;
      this.flag = s;
    }
    public static Comparator<Point> PointComparator  = new Comparator<Point>(){
      public int compare(Point p1, Point p2){
        if(p1.time == p2.time) return p1.flag - p2.flag;
        else return p1.time - p2.time;
      }
    };
}

class Solution {
    /**
     * @param intervals: An interval array
     * @return: Count of airplanes are in the sky.
     */
  public int countOfAirplanes(List<Interval> airplanes) { 
    List<Point> list = new ArrayList<>(airplanes.size()*2);
    for(Interval i : airplanes){
      list.add(new Point(i.start, 1));
      list.add(new Point(i.end, 0));
    }

    Collections.sort(list,Point.PointComparator );
    int count = 0, ans = 0;
    for(Point p : list){
      if(p.flag == 1) count++;
      else count--;
      ans = Math.max(ans, count);
    }

    return ans;
  }

}
```
