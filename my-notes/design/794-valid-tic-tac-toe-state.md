# 794 Valid Tic-Tac-Toe State

A Tic-Tac-Toe board is given as a string array`board`. Return True if and only if it is possible to reach this board position during the course of a valid tic-tac-toe game.

The`board`is a 3 x 3 array, and consists of characters`" "`,`"X"`, and`"O"`. The " " character represents an empty square.

Here are the rules of Tic-Tac-Toe:

* Players take turns placing characters into empty squares (" ").
* The first player always places "X" characters, while the second player always places "O" characters.
* "X" and "O" characters are always placed into empty squares, never filled ones.
* The game ends when there are 3 of the same (non-empty) character filling any row, column, or diagonal.
* The game also ends if all squares are non-empty.
* No more moves can be played if the game is over.

```
Example 1:
Input: board = ["O  ", "   ", "   "]
Output: false
Explanation: The first player always plays "X".

Example 2:
Input: board = ["XOX", " X ", "   "]
Output: false
Explanation: Players take turns making moves.

Example 3:
Input: board = ["XXX", "   ", "OOO"]
Output: false

Example 4:
Input: board = ["XOX", "O O", "XOX"]
Output: true
```

**Note:**

* `board`is a length-3 array of strings, where each string`board[i]`has length 3.
* Each`board[i][j]`is a character in the set`{" ", "X", "O"}`.

这题不看答案真不知道怎么做。起码有5中case要handle。除了上述4种情况以外，还有一种就是：“XXX”，“OO\__", "O_ "。这种情况应该返回false，因为X已经赢了，O不能再放了。这题需要用一个turn变量来check board上的X和O数目是否合法。他们之间只能差1，当我们见到X的时候turn++,O的时候turn--。如果turn最后不是0或1的话，证明某一个多放了。（1表示X多一个，0的时候数目相等）。另外我们还需要两个变量xwin和owin来记录是否有人赢了。如果有我们应该结束游戏，用来判断第5种情况。

```java
public boolean validTicTacToe(String[] board) {
    if (board == null || board.length != 3 || board[0].length() != 3) {
        return false;
    }

    int[] row = new int[3];
    int[] col = new int[3];
    int dig = 0;
    int anti = 0;
    boolean xwin = false;
    boolean owin = false;
    int turn = 0;

    for (int i = 0; i < board.length; i++) {
        for (int j = 0; j < board[0].length(); j++) {
            char cur = board[i].charAt(j);
            if (cur == 'X') {
                row[i]++;
                col[j]++;
                if (i == j) {
                    dig++;    
                }
                if (i + j == 2) {
                    anti++;
                }
                turn++;
            } else if (cur == 'O') {
                row[i]--;
                col[j]--;
                if (i == j) {
                    dig--;    
                }
                if (i + j == 2) {
                    anti--;
                }
                turn--;
            }
        }
    }

    if (row[0] == 3 || row[1] == 3 || row[2] == 3 || col[0] == 3 || col[1] == 3 || col[2] == 3 || dig == 3 || anti == 3) {
        xwin = true;
    }

    if (row[0] == -3 || row[1] == -3 || row[2] == -3 || col[0] == -3 || col[1] == -3 || col[2] == -3 || dig == -3 || anti == -3) {
        owin = true;
    }

    if ((xwin && turn == 0) || (owin && turn == 1)) {
        return false;
    }

    return (turn == 0 || turn == 1) && (!owin || !xwin);
}
```
