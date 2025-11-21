# edit distance to palindrome

You can use dynamic programming approach to solve this problem. dp\[i, j] — minimum number of operations to convert substring s\[i..j] into palindrome. Below is the transitions:

Obviously, dp\[i, j] = 0 if i >= j

And in general dp\[i, j] is minimum of:

* dp\[i+1, j-1] if s\[i] == s\[j]
* dp\[i+1]\[j-1] + 1 # replace character s\[i] to s\[j] OR s\[j] to s\[i]
* dp\[i+1, j] + 1 # remove i-th character OR insert a new character right after j-th position
* dp\[i, j-1] + 1 # remove j-th character OR insert a new character right before i-th position

The answer will be stored in dp\[0, len(s)-1]
