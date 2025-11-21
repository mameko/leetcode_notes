# 272 Closest Binary Search Tree Value II

Given a non-empty binary search tree and a target value, findkvalues in the BST that are closest to the target.

**Note:**

* Given target value is a floating point.
* You may assume k is always valid, that is: k ≤ total nodes.
* You are guaranteed to have only one unique set of k values in the BST that are closest to the target.

**Example:**

```
Input: root = [4,2,5,1,3], target = 3.714286, and k = 2

    4
   / \
  2   5
 / \
1   3

Output: [4,3]
```

**Follow up:**\
Assume that the BST is balanced, could you solve it in less thanO(n) runtime (wheren= total nodes)?

这题其实延续了[270 Closest Binary Search Tree Value](270-closest-binary-search-tree-value.md)的做法，只是把min从一个数换成一个heap。但这解法是T:O(N)。一开还以为可以logN地2分做，然后发现如果k大于半数的话，连另外一边的也得加进去，所以不行。然后下面还贴了O(logN)的解法作为参考，自己是想不出来的了。

```java
/**
 * Definition for a binary tree node.
 * public class TreeNode {
 *     int val;
 *     TreeNode left;
 *     TreeNode right;
 *     TreeNode(int x) { val = x; }
 * }
 */
class Solution {
    public List<Integer> closestKValues(TreeNode root, double target, int k) {
        List<Integer> res = new ArrayList<>();
        if (root == null) {
            return res;
        }

        // find min use maxHeap, keep k cloest in the heap
        PriorityQueue<HeapNode> diffMaxHeap = new PriorityQueue<>((node1, node2) -> node2.diff.compareTo(node1.diff));
        Stack<TreeNode> stack = new Stack<>();        
        TreeNode cur = root;

        while (cur != null || !stack.isEmpty()) {
            while (cur != null) {
                stack.push(cur);
                cur = cur.left;
            }

            cur = stack.pop();
            double diff = Math.abs(cur.val - target);
            diffMaxHeap.offer(new HeapNode(diff, cur.val));
            if (diffMaxHeap.size() > k) {              
                diffMaxHeap.poll();              
            }             

            cur = cur.right;
        }

        while (!diffMaxHeap.isEmpty()){
            res.add(diffMaxHeap.poll().nodeVal);
        }
        return res;
    }
}

class HeapNode {
    Double diff;
    int nodeVal;

    public HeapNode(Double d, int v) {
        diff = d;
        nodeVal = v;
    }
}
```

O(logN)的解法：

```java
public class Solution {
    public List<Integer> closestKValues(TreeNode root, double target, int k) {
        List<Integer> ret = new LinkedList<>();
        Stack<TreeNode> succ = new Stack<>();
        Stack<TreeNode> pred = new Stack<>();
        initializePredecessorStack(root, target, pred);
        initializeSuccessorStack(root, target, succ);
        if(!succ.isEmpty() && !pred.isEmpty() && succ.peek().val == pred.peek().val) {
            getNextPredecessor(pred);
        }
        while(k-- > 0) {
            if(succ.isEmpty()) {
                ret.add(getNextPredecessor(pred));
            } else if(pred.isEmpty()) {
                ret.add(getNextSuccessor(succ));
            } else {
                double succ_diff = Math.abs((double)succ.peek().val - target);
                double pred_diff = Math.abs((double)pred.peek().val - target);
                if(succ_diff < pred_diff) {
                    ret.add(getNextSuccessor(succ));
                } else {
                    ret.add(getNextPredecessor(pred));
                }
            }
        }
        return ret;
    }

    private void initializeSuccessorStack(TreeNode root, double target, Stack<TreeNode> succ) {
        while(root != null) {
            if(root.val == target) {
                succ.push(root);
                break;
            } else if(root.val > target) {
                succ.push(root);
                root = root.left;
            } else {
                root = root.right;
            }
        }
    }

    private void initializePredecessorStack(TreeNode root, double target, Stack<TreeNode> pred) {
        while(root != null){
            if(root.val == target){
                pred.push(root);
                break;
            } else if(root.val < target){
                pred.push(root);
                root = root.right;
            } else{
                root = root.left;
            }
        }
    }

    private int getNextSuccessor(Stack<TreeNode> succ) {
        TreeNode curr = succ.pop();
        int ret = curr.val;
        curr = curr.right;
        while(curr != null) {
            succ.push(curr);
            curr = curr.left;
        }
        return ret;
    }

    private int getNextPredecessor(Stack<TreeNode> pred) {
        TreeNode curr = pred.pop();
        int ret = curr.val;
        curr = curr.left;
        while(curr != null) {
            pred.push(curr);
            curr = curr.right;
        }
        return ret;
    }
}

// cleaner version
class Solution {
public List closestKValues(TreeNode root, double target, int k) {

    Stack<TreeNode> succ = new Stack<>();
    Stack<TreeNode> pred = new Stack<>();

    inOrderTraversal(root,false,target,succ);
    inOrderTraversal(root,true,target,pred);

    List<Integer> result = new ArrayList<>();

    for(int i = 0; i<k; i++){
        if(succ.size()==0){
            result.add(getNext(pred,true));
        }else if(pred.size()==0){
            result.add(getNext(succ,false));
        }else{
            if(Math.abs(target-succ.peek().val)<Math.abs(target-pred.peek().val)){
                result.add(getNext(succ,false));
            }else{
                result.add(getNext(pred,true));
            }
        }
    }

    return result;


}

private void inOrderTraversal(TreeNode root, boolean reverse, double target, Stack stack){
    while(root != null){
        if((!reverse && root.val>=target) || (reverse && root.val<target)){
            stack.push(root);
            root = reverse?root.right:root.left;
        }else{
            root = reverse?root.left:root.right;
        }
    }
}

private Integer getNext(Stack<TreeNode> stack, boolean reverse){
    TreeNode curr = stack.pop();
    Integer val = curr.val;
    curr = reverse?curr.left:curr.right;
    while(curr!=null){
        stack.push(curr);
        curr = reverse? curr.right: curr.left;
    }
    return val;
}
}
```
