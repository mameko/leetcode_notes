# L556 Standard Bloom filter

Implement a standard bloom filter. Support the following method:\
1.`StandardBloomFilter(k)`,The constructor and you need to create k hash functions.\
2.`add(string)`. add a string into bloom filter.\
3.`contains(string)`. Check a string whether exists in bloom filter.

**Example**

```
StandardBloomFilter(3)
add("lint")
add("code")
contains("lint") // return true
contains("world") // return false
```

```java
public class StandardBloomFilter {
    int SIZE = 1000000;
    boolean[] filterArr;
    HashFun[] hf;

    public StandardBloomFilter(int k) {
        filterArr = new boolean[SIZE];
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
            filterArr[ind] = true;
        }
    }

    public boolean contains(String word) {
         for (int i = 0; i < hf.length; i++) {
            int ind = hf[i].getHash(word);
            if (!filterArr[ind]) {
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
