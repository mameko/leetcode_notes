# L555 Counting Bloom Filter

Implement a counting bloom filter. Support the following method:\
1.`add(string)`. Add a string into bloom filter.\
2.`contains(string)`. Check a string whether exists in bloom filter.\
3.`remove(string)`. Remove a string from bloom filter.

**Example**

```
CountingBloomFilter(3) 
add("lint")
add("code")
contains("lint") // return true
remove("lint")
contains("lint") // return false
```

这里我用了一个大数做为bloom filter 的array size。然后hash function的做法是[L128 Hash funtion.](../others/l128-hash-function.md) 这里还用了找小于等于n数里的prime number。

```java
public class CountingBloomFilter {
    int SIZE = 1000000;
    int[] filterArr;
    HashFun[] hf;

    public CountingBloomFilter(int k) {
        filterArr = new int[SIZE];
        HashSet<Integer> primes = new HashSet<>();
        hf = new HashFun[k];

        Random r = new Random();
        while (primes.size() < k) {
            int p = r.nextInt(1000);
            if (isPrime(p)) {
                hf[primes.size()] = new HashFun(p, SIZE);
                primes.add(p);
            }
        }
    }

    public void add(String word) {
        for (int i = 0; i < hf.length; i++) {
            int ind = hf[i].getHash(word);
            filterArr[ind]++;
        }
    }

    public void remove(String word) {
        if (!contains(word)) {
            return;
        }

        for (int i = 0; i < hf.length; i++) {
            int ind = hf[i].getHash(word);
            filterArr[ind]--;
        }
    }

    public boolean contains(String word) {
        for (int i = 0; i < hf.length; i++) {
            int ind = hf[i].getHash(word);
            if (filterArr[ind] < 1) {
                return false;
            }
        }

        return true;
    }

    private boolean isPrime(int p) {
        if (p < 2) {
            return false;
        }

        if (p == 2) {
            return true;
        }

        if (p % 2 == 0) {
            return false;
        }

        for (int i = 3; i * i <= p; i+=2) {
            if (p % i == 0) {
                return false;
            }
        }

        return true;
    }
}

class HashFun {
    int prime;
    int SIZE;

    public HashFun(int p, int s) {
        prime = p;
        SIZE = s;
    }

    public int getHash(String str) {
        int hashcode = 0;
        for (int i = 0; i < str.length(); i++) {
            hashcode = (hashcode * prime + str.charAt(i)) % SIZE;
        }

        return hashcode;
    }

}
```
