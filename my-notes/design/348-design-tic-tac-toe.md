# 348 Design Tic-Tac-Toe

Design a Tic-tac-toe game that is played between two players on anxngrid.

You may assume the following rules:

1. A move is guaranteed to be valid and is placed on an empty block.
2. Once a winning condition is reached, no more moves is allowed.
3.  A player who succeeds in placing

    n

    of their marks in a horizontal, vertical, or diagonal row wins the game.

**Example:**

```
Given 
n
 = 3, assume that player 1 is "X" and player 2 is "O" in the board.

TicTacToe toe = new TicTacToe(3);

toe.move(0, 0, 1); -
>
 Returns 0 (no one wins)
|X| | |
| | | |    // Player 1 makes a move at (0, 0).
| | | |

toe.move(0, 2, 2); -
>
 Returns 0 (no one wins)
|X| |O|
| | | |    // Player 2 makes a move at (0, 2).
| | | |

toe.move(2, 2, 1); -
>
 Returns 0 (no one wins)
|X| |O|
| | | |    // Player 1 makes a move at (2, 2).
| | |X|

toe.move(1, 1, 2); -
>
 Returns 0 (no one wins)
|X| |O|
| |O| |    // Player 2 makes a move at (1, 1).
| | |X|

toe.move(2, 0, 1); -
>
 Returns 0 (no one wins)
|X| |O|
| |O| |    // Player 1 makes a move at (2, 0).
|X| |X|

toe.move(1, 0, 2); -
>
 Returns 0 (no one wins)
|X| |O|
|O|O| |    // Player 2 makes a move at (1, 0).
|X| |X|

toe.move(2, 1, 1); -
>
 Returns 1 (player 1 wins)
|X| |O|
|O|O| |    // Player 1 makes a move at (2, 1).
|X|X|X|
```

**Follow up:**\
Could you do better than O(n2) per`move()`operation?

Hint 1: Could you trade extra space such that`move()`operation can be done in O(1)?

Hint 2 : You need two arrays: int rows\[n], int cols\[n], plus two variables: diagonal, anti\_diagonal.

这题看了提示以后基本就会做了，就是用两个数组和两个变量来数数。因为只有两个player，所以只要一个加一一个减一，每次move的时候check一下对应位置的值是否已经达到size/-size就ok了。达到size/-size就意味着这一行/列/对角线/反对角线已经有n个同样的棋子，所以返回相应的赢家。

```java
public class TicTacToe {
    int dig = 0;
    int antiDig = 0;
    int[] rowCnt;
    int[] colCnt;
    int size;
    /** Initialize your data structure here. */
    public TicTacToe(int n) {
        rowCnt = new int[n];
        colCnt = new int[n];
        size = n;
    }

    /** Player {player} makes a move at ({row}, {col}).
        @param row The row of the board.
        @param col The column of the board.
        @param player The player, can be either 1 or 2.
        @return The current winning condition, can be either:
                0: No one wins.
                1: Player 1 wins.
                2: Player 2 wins. */
    public int move(int row, int col, int player) {
        if (row < 0 || col < 0 || (player != 1 && player != 2)) {
            return 0;
        }

        // if it's player 1's move, + 1
        // if it's player 2's move, - 1        
        colCnt[col] = player == 1 ? colCnt[col] + 1 : colCnt[col] - 1;
        rowCnt[row] = player == 1 ? rowCnt[row] + 1 : rowCnt[row] - 1;
        if (col == row) {
            dig = player == 1 ? dig + 1 : dig - 1;
        }
        if (col + row == size - 1) {
            antiDig = player == 1 ? antiDig + 1 : antiDig - 1;
        }

        if (colCnt[col] == size || rowCnt[row] == size || dig == size || antiDig == size) {
            return 1;
        } else if (colCnt[col] == -size || rowCnt[row] == -size || dig == -size || antiDig == -size) {
            return 2;
        } else {
            return 0;
        }
    }
}

/**
 * Your TicTacToe object will be instantiated and called as such:
 * TicTacToe obj = new TicTacToe(n);
 * int param_1 = obj.move(row,col,player);
 */
```
