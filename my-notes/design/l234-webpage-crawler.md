# L234 Webpage Crawler

Implement a webpage Crawler to crawl webpages of`http://www.wikipedia.org/`. To simplify the question, let's use url instead of the the webpage content.

Your crawler should:

1. Call`HtmlHelper.parseUrls(url)`to get all urls from a webpage of given url.
2. Only crawl the webpage of wikipedia.
3. Do not crawl the same webpage twice.
4. Start from the homepage of wikipedia: [http://www.wikipedia.org/](http://www.wikipedia.org/)

## Notice

You need do it with multithreading.

**Example**

Given

```
"http://www.wikipedia.org/": ["http://www.wikipedia.org/help/"]
"http://www.wikipedia.org/help/": []
```

Return`["http://www.wikipedia.org/", "http://www.wikipedia.org/help/"]`

Given:

```
"http://www.wikipedia.org/": ["http://www.wikipedia.org/help/"]
"http://www.wikipedia.org/help/": ["http://www.wikipedia.org/", "http://www.wikipedia.org/about/"]
"http://www.wikipedia.org/about/": ["http://www.google.com/"]
```

Return \["[http://www.wikipedia.org/](http://www.wikipedia.org/)", "[http://www.wikipedia.org/help/](http://www.wikipedia.org/help/)", "\[[http://www.wikipedia.org/about/"\](http://www.wikipedia.org/about/")\\](http://www.wikipedia.org/about/%22]\(http://www.wikipedia.org/about/%22\)/)]

不知道为毛70% TLE，但生产者消费者模式和java 线程怎么写都在这了，写下来参考。

```java
import java.util.concurrent.BlockingQueue;
import java.util.concurrent.ConcurrentHashMap;
import java.util.concurrent.LinkedBlockingQueue;
import java.net.URL;

public class Solution {
    /**
     * @param url a url of root page
     * @return all urls
     */
    int tnum = 12;
    public List<String> crawler(String url) {
        BlockingQueue<String> q = new LinkedBlockingQueue<>();
        ConcurrentHashMap<String, String> visited = new ConcurrentHashMap<>();
        List<String> result = new ArrayList<>();
        try {
            q.put(url);
            visited.put(url, "");

            Thread[] threads = new Thread[tnum];
            for (int i = 0; i < tnum; i++) {
                threads[i] = new Thread(new Crawler(q, visited));
                threads[i].start();
            }

            for (int i = 0; i < tnum; i++) {
                threads[i].join();
            }
        } catch (Exception e) {
            System.out.println("thread interupt exception thrown");
        }
        for (String s : visited.keySet()) {
            result.add(s);
        }

        return result;
    }
}

class Crawler implements Runnable {
    private final BlockingQueue<String> queue;
    private final ConcurrentHashMap<String, String> visited;

    public Crawler(BlockingQueue<String> q, ConcurrentHashMap<String, String> v) {
        queue = q;
        visited = v;
    }

    public void run() {
        while (!queue.isEmpty()) {
            try {
                String cur = queue.take();

                for (String nei : HtmlHelper.parseUrls(cur)) {
                    if (visited.containsKey(nei)) {
                        continue;
                    }

                    String domain = "";
                    URL netUrl = new URL(nei);
                    domain = netUrl.getHost();

                    if (!domain.endsWith("wikipedia.org")) {
                        continue;
                    }

                    queue.put(nei);
                    visited.put(nei, "");
                }
            } catch (Exception e) {
                    System.out.println(" exception thrown"); 
            }
        }
    }
}
```
