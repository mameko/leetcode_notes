# file access

扔盒高频，看到了面经的api描述，感觉就像之前limebike里考的那题，知道员工上下级关系，然后给你一个员工求所有下属，包括下属的下属。

```
/A <-- explicit access  
|___ /B  
||___ /C <-- explicit access  
||___ /D -- /k access  
| ---  
|___ /E <-- access  
 |___ /F

 比如，A为系统的root, B,E are A's subfolder, C and D are B's sub folder, if the has the access for B, then the one has B' access and all its subfolder's Access.
1) given all known access right of the file system, implement a method to check if the user has the right to certain folders
2) follow up, we want to remove redundant access information in our system, how to do that ? The redundant access information, could be, the user already has access to A, however, in some access list
it explicitly authorize the user access to folder C. This is redundant as, C is a subfoler of B, and B is sub-folder of A.
*/

Set<String> access = new HashSet<String>();
access.add("C");
access.add("E");

// Child, Parent 
List<String[]> folders = Arrays.asList(new String[][]{
  {"A", null},
  {"B", "A"},
  {"C", "B"},
  {"D", "B"},
  {"E", "A"},
  {"F", "E"},
});

================lime 题=====================
给了一堆pair作为输入表示员工上下级关系，input = [ [A, C], [A, B], [B, D], [C，E],[C, F], [F, I], [F, G]]， A是上级，C是下级
然后输入一个C， 找出TA所有的下级。例子是输出：[E, F, I, G]

看着眼熟但又没做过。我就弄了一个node的数据结构存点的名字，然后每个点有一堆孩子。
最后为了查找方便，用了一个全局的hashmap去存《名字，Node》

写了两个函数，一个是把输入变成tree。
另一个是递归找孩子。

这个codepad最后还能run code。出了一堆typo和compile error。fix掉，然后得到答案。

有个类似follow up的问题，我在clarify的时候就问了。我说，可以假设图里没有loop吗？他说，可以有，有的话你就抛个异常吧。不过我一时没想出来。我就说不如写完以后再想把。
写完通过了以后，作为follow up问我了。
我说，只能在加children的时候暴力搜我父亲有那个点的所有子子孙孙有没有我啦。
然后面试官说，能用hashmap优化一下。但我还是觉得怎么也得跑一遍，填hashmap，用了hashmap就是你第二遍的时候不用再搜一遍而已。算法上没有优化。
```

```java
public class FileAccess {

    Map<String, FileNode> map = new HashMap<>();

    public static void main(String[] args) {
        FileAccess sol = new FileAccess();
        List<String[]> folders = Arrays.asList(new String[][] { { "A", null }, { "B", "A" }, { "C", "B" }, { "D", "B" },
                { "E", "A" }, { "F", "E" }, });
        sol.createTree(folders);
        Set<String> access = new HashSet<String>();
        access.add("C");
        access.add("E");

        List<FileNode> res = new ArrayList<>();
        for (String node : access) {
            FileNode curNode = sol.map.get(node);
            res.addAll(sol.findChild(curNode));
            res.add(sol.map.get(node));
        }
        System.out.println(res);
    }

    public List<FileNode> findChild(FileNode folder) {
        List<FileNode> res = folder.children;
        if (res.size() == 0) {
            return res;
        }
        for (FileNode file : res) {
            res.addAll(findChild(file));
        }

        return res;        
    }

    public void createTree(List<String[]> folders) {
        if (folders == null || folders.size() == 0) {
            return;
        }

        for (String[] folder : folders) {
            String child = folder[0];
            String parent = folder[1];
            if (!map.containsKey(child)) {
                FileNode newNode = new FileNode(child); 
                map.put(child, newNode);
            }

            if (!map.containsKey(parent)) {
                FileNode newNode = new FileNode(parent);
                map.put(parent, newNode);
            }

            map.get(parent).children.add(map.get(child));
        }
    }

}

class FileNode {
    String val;
    List<FileNode> children = new ArrayList<>();;

    public FileNode(String name) {
        val = name;        
    }

    @Override
    public String toString() {    
        return val;
    }
}
```
