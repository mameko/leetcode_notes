# xml parser

given a list of cvs nodes that represent an xml tree, make a parser to turn file into tree, follow up, what if there are invalid input in cvs

```
/*
 *<Story>
      <id>1234</id>
      <Snaps>
          <Snap></Snap>
          <Snap></Snap>
          <Snap></Snap>
          <Snap></Snap>
      </Snaps>
  <Story>
*/

input = [ "open,story",
          "open,id",
          "inner,1234",
          "close,id",
          "open,snaps",
          "open,snap",
          "close,snap",
          "open,snap",
          "close,snap",
          "open,snap",
          "close,snap",
          "open,snap",
          "close,snap",
          "close,snaps",
          "close,story"]
```

这题在定义dom tree时用了composite design pattern（file system也可以用这个设计模式）然后用了栈来处理输入的node。最后打印时用了[nested integer](https://github.com/mameko/pan-s-algorithm-notes/tree/26d48de4dcba6bb4e1691bde8ffdcd7272fcd0f0/L22%20Flatten%20List/README.md)那题的方法。

```java
package other;

import java.util.ArrayList;
import java.util.Arrays;
import java.util.List;
import java.util.Stack;

public class S_xmlParser {
    public DomTree parse(List<String> input) {
        Stack<TagNode> stack = new Stack<>();
        DomTree last = null;
        for (String parts : input) {
            String[] part = parts.split(",");
            if (part[0].equals("open")) {
                TagNode tag = new TagNode();
                tag.start = "<" + part[1] + ">";
                if (!stack.isEmpty()) {
                    stack.peek().children.add(tag);
                }
                stack.push(tag);
            } else if (part[0].equals("close")) {
                if (!stack.isEmpty()) {
                    TagNode tag = stack.pop();
                    tag.end = "</" + part[1] + ">";
                    last = tag;
                }
            } else {
                ContentNode content = new ContentNode();
                content.content = part[1];
                stack.peek().children.add(content);
            }
        }

        return last;
    }

    public static void main(String[] args) {
        S_xmlParser parser = new S_xmlParser();
        List<String> input = new ArrayList<>(Arrays.asList("open,story", "open,id", "inner,1234", "close,id",
                "open,snaps", "open,snap", "close,snap", "open,snap", "close,snap", "open,snap", "close,snap",
                "open,snap", "close,snap", "close,snaps", "close,story"));
        DomTree dom = parser.parse(input);
        print(dom);
    }

    private static void print(DomTree dom) {
        if (dom instanceof TagNode) {
            System.out.println(((TagNode)dom).start);
            for (DomTree tag : ((TagNode)dom).children) {
                print(tag);
            }
            System.out.println(((TagNode)dom).end);
        } else {
            System.out.println(((ContentNode)dom).content);
        }        
    }
}

// composite design pattern
class DomTree {    
}

class ContentNode extends DomTree {
    String content;

    @Override
    public String toString() {
        return content;
    }    
}

class TagNode extends DomTree {
    List<DomTree> children = new ArrayList<>();
    String start;
    String end;

    public List<DomTree> getChildren() {
        return children;
    }

    public void setChildren(List<DomTree> children) {
        this.children = children;
    }

    @Override
    public String toString() {
        return start + children.toString() + end;
    }
}

/*
 *<Story>
      <id>1234</id>
      <Snaps>
          <Snap></Snap>
          <Snap></Snap>
          <Snap></Snap>
          <Snap></Snap>
      </Snaps>
  <Story>
*/
```
