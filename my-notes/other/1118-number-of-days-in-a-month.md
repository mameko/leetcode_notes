# 1118 Number of Days in a Month

Given a year `year` and a month `month`, return _the number of days of that month_.

**Example 1:**

<pre><code>Input: year = 1992, month = 7
<strong>Output: 31
</strong></code></pre>

**Example 2:**

<pre><code>Input: year = 2000, month = 2
<strong>Output: 29
</strong></code></pre>

**Example 3:**

<pre><code>Input: year = 1900, month = 2
<strong>Output: 28
</strong></code></pre>

&#x20;**Constraints:**

* `1583 <= year <= 2100`
* `1 <= month <= 12`

一开始`还以为很简单，后来百度一下才知道闰年的规则这么复杂。小学时候被骗了。其实是，一般的年，能被4整除，不被100整除就是闰年。如果是百年的话，要被400整除才是闰年。`



```java
public int numberOfDays(int year, int month) {
    if (year < 0 || month < 1 || month > 12) {
        return -1;
    }
    
    int[] daysInMonthExceptFeb = {31, 28, 31, 30, 31, 30, 31, 31, 30, 31, 30, 31};       
    if (month == 2 && year % 4 == 0) {            
        if (year % 100 == 0) {
            if (year % 400 == 0) {
                return 29;
            } else {}
                return 28;
            }
        } else {
            return 29;    
        }
    } else {
        return daysInMonthExceptFeb[month - 1];
    }
}
```
