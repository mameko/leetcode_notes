# ctci 16.10 Living People

Given a list of people with their birth and death years, implement a method to compute the year with the most number of people alive. You may assume that all people were born between 1900 and 2000 (inclusive). If a person was alive during any portion of that year, they should be included in that year's count. For example, Person (birth= 1908, death= 1909) is included in the counts for both 1908 and 1909.

我的做法还是range addition，花比较多空间。要建一个这个range那么大的数组。O（n）时间和O（range）的空间。跟前面的那题很像。

```java
public int livingPeople(List<Pair> people) {
    if (people == null || people.size() == 0) {
        return 0;
    }

    int[] tmp = new int[101];
    for (Pair birth_death : people) {
        int birth = birth_death.getN1() - 1900;
        int death = birth_death.getN2() - 1900;

        if (birth < 0) {
            birth = 0;
        }

        tmp[birth] = 1;
        if (death < 100) {
            tmp[death + 1] = -1;
        } else {
            tmp[100] = -1;
        }
    }

    int[] result = new int[101];
    result[0] = tmp[0];
    for (int i = 1; i < result.length; i++) {
        result[i] = result[i - 1] + tmp[i];
    }

    int max = -1;
    int maxyear = -1;
    for (int i = 0; i < result.length; i++) {
        if (result[i] > max) {
            max = result[i];
            maxyear = i;
        }
    }

    return maxyear + 1900;
}
```
