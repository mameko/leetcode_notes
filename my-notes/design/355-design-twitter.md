# 355 Design Twitter

Implement a simple twitter. Support the following method:

1. `postTweet(user_id, tweet_text)`. Post a tweet.
2. `getTimeline(user_id)`. Get the given user's most recently 10 tweets posted by himself, order by timestamp from most recent to least recent.
3. `getNewsFeed(user_id)`. Get the given user's most recently 10 tweets in his news feed (posted by his friends and himself). Order by timestamp from most recent to least recent.
4. `follow(from_user_id, to_user_id)`. from\_user\_id followed to\_user\_id.
5. `unfollow(from_user_id, to_user_id)`. from\_user\_id unfollowed to to\_user\_id.

**Example**

```
postTweet(1, "LintCode is Good!!!")
>> 1
getNewsFeed(1)
>> [1]
getTimeline(1)
>> [1]
follow(2, 1)
getNewsFeed(2)
>> [1]
unfollow(2, 1)
getNewsFeed(2)
>> []
```

```java
/**
 * Definition of Tweet:
 * public class Tweet {
 *     public int id;
 *     public int user_id;
 *     public String text;
 *     public static Tweet create(int user_id, String tweet_text) {
 *         // This will create a new tweet object,
 *         // and auto fill id
 *     }
 * }
 */
public class MiniTwitter {
    // storge <uid, tweet> -- db table
    HashMap<Integer, ArrayList<TweetWithTS>> userTweetTable;
    // foller table <from, to>
    HashMap<Integer, HashSet<Integer>> followers;
    Long globalOrder;// maybe able to use id as timestamp instead of using global variable

    public MiniTwitter() {
        userTweetTable = new HashMap<>();
        followers = new HashMap<>();
        globalOrder = 0L;
    }

    // @param user_id an integer
    // @param tweet a string
    // return a tweet
    public Tweet postTweet(int user_id, String tweet_text) {
        // add tweet to usser - tweet table
        ArrayList<TweetWithTS> tmp;
        Tweet newTwt = Tweet.create(user_id, tweet_text);
        TweetWithTS tweetWithTime = new TweetWithTS(newTwt, globalOrder++);
        // check null every where
        if (userTweetTable.containsKey(user_id)) {
            tmp = userTweetTable.get(user_id);
        } else {
            tmp = new ArrayList<>();
        }
        // tmp is reference, will add to actual list in hashmap
        tmp.add(tweetWithTime);
        userTweetTable.put(user_id, tmp);
        return newTwt;
    }

    // @param user_id an integer
    // return a list of 10 new feeds recently
    // and sort by timeline
    // -------use pull----------
    public List<Tweet> getNewsFeed(int user_id) {
        // get all user this user followed
        HashSet<Integer> followTarget;
        // check null every whare
        if (followers.containsKey(user_id)) {
            followTarget = followers.get(user_id);
        } else {
            followTarget = new HashSet<>();
        }
        followTarget.add(user_id);// this will add user to his/her follow target list, which won't affect anything here, we may want to avoid this ?

        // extract last 10 tweets from each of follow targets
        // min heap when we need last 10 (recent tweets has TimeStamp later ->
        // bigger)
        TreeSet<TweetWithTS> treeSet = new TreeSet<>(new Comparator<TweetWithTS>() {
            public int compare(TweetWithTS t1, TweetWithTS t2) {
                return t1.ts.compareTo(t2.ts);
            }
        });

        for (Integer fid : followTarget) {
            List<TweetWithTS> cur = userTweetTable.get(fid);
            if (cur == null) {
                continue;
            }
            int size = cur.size();
            int from = size - 10 > 0 ? size - 10 : 0;
            cur = cur.subList(from, size);
            treeSet.addAll(cur);
        }

        // get last 10 out of the set
        ArrayList<Tweet> res = new ArrayList<>();
        for (int i = 0; i < 10 && !treeSet.isEmpty(); i++) {
            res.add(treeSet.pollLast().tw);
        }

        return res;
    }

    // @param user_id an integer
    // return a list of 10 new posts recently
    // and sort by timeline
    public List<Tweet> getTimeline(int user_id) {
        List<TweetWithTS> tmp = new ArrayList<>();// we new a list here so we won't change the actual list in hashmap
        List<Tweet> res = new ArrayList<>();
        if (userTweetTable.containsKey(user_id)) {
            tmp.addAll(userTweetTable.get(user_id));
            // here we need to use reverse order, if you use ascending order and get last 10, you will still need to reverse them when you return
            Collections.sort(tmp, new Comparator<TweetWithTS>() {
                @Override
                public int compare(TweetWithTS t1, TweetWithTS t2) {
                    return t2.ts.compareTo(t1.ts);
                }
            });
            // remember only return 10
            int size = tmp.size();
            int to = size - 10 > 0 ? 10 : size;
            tmp = tmp.subList(0, to);
            res = extractTweets(tmp);
        }
        return res;
    }

    // @param from_user_id an integer
    // @param to_user_id an integer
    // from user_id follows to_user_id
    public void follow(int from_user_id, int to_user_id) {
        HashSet<Integer> tmp;
        // check null every whare
        if (followers.containsKey(from_user_id)) {
            tmp = followers.get(from_user_id);
        } else {
            tmp = new HashSet<>();
        }
        tmp.add(to_user_id);
        followers.put(from_user_id, tmp);
    }

    // @param from_user_id an integer
    // @param to_user_id an integer
    // from user_id unfollows to_user_id
    public void unfollow(int from_user_id, int to_user_id) {
        HashSet<Integer> tmp = followers.get(from_user_id);
        if (tmp != null) {
            tmp.remove(to_user_id);
            followers.put(from_user_id, tmp);
        }
    }

    // this method is to extract tweets from tweetsWithTimeStamp
    private List<Tweet> extractTweets(List<TweetWithTS> list) {
        ArrayList<Tweet> res = new ArrayList<>();
        for (TweetWithTS t : list) {
            res.add(t.tw);
        }
        return res;
    }
}

class TweetWithTS {
    Long ts;
    Tweet tw;

    public TweetWithTS(Tweet tweet, Long ts) {
        this.ts = ts;
        this.tw = tweet;
    }

}
```
