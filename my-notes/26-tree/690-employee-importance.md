# 690 Employee Importance

You are given a data structure of employee information, which includes the employee's **unique id**, their **importance value** and their **direct** subordinates' id.

For example, employee 1 is the leader of employee 2, and employee 2 is the leader of employee 3. They have importance value 15, 10 and 5, respectively. Then employee 1 has a data structure like \[1, 15, \[2]], and employee 2 has \[2, 10, \[3]], and employee 3 has \[3, 5, \[]]. Note that although employee 3 is also a subordinate of employee 1, the relationship is **not direct**.

Now given the employee information of a company, and an employee id, you need to return the total importance value of this employee and all their subordinates.

**Example 1:**

```
Input: [[1, 5, [2, 3]], [2, 3, []], [3, 3, []]], 1
Output: 11
Explanation:
Employee 1 has importance value 5, and he has two direct subordinates: employee 2 and employee 3. They both have importance value 3. So the total importance value of employee 1 is 5 + 3 + 3 = 11.
```

**Note:**

1. One employee has at most one **direct** leader and may have several subordinates.
2. The maximum number of employees won't exceed 2000.

这题，读题时间比解题长。其实就是带权重的树遍历。可以BFS或者DFS。图的遍历，T：O（e+v），所以是O(n)，S：O(n)，队列长度。但其实每次还得O(n)地找，是不是其实是n方？啊！看了solution发现，leetcode太狡猾了，用了O(n)先把东西存到hashmap里，然后查找起来就不用每次再O(n)了

```java
/*
// Definition for Employee.
class Employee {
    public int id;
    public int importance;
    public List<Integer> subordinates;
};
*/

class Solution {
    public int getImportance(List<Employee> employees, int id) {
        if (employees == null || employees.size() == 0 || id < 0) {
            return 0;
        }
        
        Map<Integer, Employee> map = new HashMap<>();
        for (Employee e : employees) {
           map.put(e.id, e);
        }
        
        Queue<Employee> queue = new LinkedList<>();
        Employee root = map.get(id);       
        
        // employee not in the list
        if (root == null) {
            return 0;
        }
        
        queue.offer(root);
        
        int sum = 0;
        while (!queue.isEmpty()) {
            Employee cur = queue.poll();
            sum += cur.importance;
            for (Integer sub : cur.subordinates) {
                queue.offer(map.get(sub));
            }
        }
        
        return sum;
    }
}
```
