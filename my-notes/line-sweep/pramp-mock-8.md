# pramp mock 8

The mall management is trying to figure out what was the busiest moment in the mall in the last year. You are given data from the door detectors: each data entry includes a timestamp(secondes in Unix Epoch format), an amount of people and whether they entered or exited.

Example of a data entry: {time : 1440084737, count: 4, type : "enter"}

Find what was the busiest period in the mall on the last year. Return an array with two Epoch timestamps representing the beginning and end of that period. You man assume that the data your are given is accurate and that each second with entries or exits is recorded. Implement the most efficient solution possible and analyze its time and space complexity.

这题明显是line sweep，不过得注意相同时间有进有出时，要先减掉再加上。不然的话结果就不对了。最后这题只是返回最大时间点，我在想，如果follow up要求返回的是一段时间的开始和结束得怎么做。个人觉得可以在update max时的else里记录什么时候local max变小了。然后那个就是max period的终点，不过返回时要判断是否起点<终点，是才返回，因为有一种情况是，max一直保持到最后，那样的话最后一次变小就不存在。那时候返回\[entry i， entry i]

```java
class Pramp {
   public static void main(String[] args) {
      String pramp = "Practice Makes Perfect";
      System.out.println(pramp);
   }
}


class DataEntry {
   Long time;
   int count;
   boolean type;

   // constructor...

}

public String[] busiestTime(ArrayList<DataEntry> entries) {
   if (entries == null || entries.size() == 0) {
      return new String[2];
   }
   // 如果同一时间有人进又有人出的话，要先减再加。
   Collections.sort(entries, new Comparator<DataEntry>(){
      public int compare(DataEntry e1, DataEntry e2) {
         if (e1.time.equals(e2.time)) {
             if (e1.type) {return 1}
             else { return -1;}
         }
         return e1.time.compareTo(e2.time);
      }
   });

   int max = 0;
   int local = 0;
   Long[] res = new Long[2];

   for (DataEntry entry : entries) {

      // if we see enter entry, add the count
      if (entry.type) {
         local += entry.count;
      } else {
         // if we see exited entry, minus the count
         local -= entry.count;
      }

      if (max < local) {
         max = local;
         res[0] = entry.time;
         if (not the last one) {
             res[1] = i + 1
         } else {
             res[1] = i;
         }
      } 

   }

   return res;
}

time   1    2   4  4  6  9
entry  2        3  
 exit       -1    -5  -3  
lastU  1    1   4             4  


local  +2   1   4   1   
max    +2   2   4   4
lastU   1   1   4   4  4
```
