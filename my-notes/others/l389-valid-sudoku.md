# L389 Valid Sudoku

Determine if a Sudoku is valid, according to:[Sudoku Puzzles - The Rules](http://sudoku.com.au/TheRules.aspx).

The Sudoku board could be partially filled, where empty cells are filled with the character`'.'`.

![](http://upload.wikimedia.org/wikipedia/commons/thumb/f/ff/Sudoku-by-L2G-20050714.svg/250px-Sudoku-by-L2G-20050714.svg.png)

A partially filled sudoku which is valid.

**Note:**\
A valid Sudoku board (partially filled) is not necessarily solvable. Only the filled cells need to be validated.

用了sudoku solver的解法：T:O(n) S:O(n) -- 可能是constant因为就只有81格。关于cubused的映射关系：i/3会把row分成3份，分别是0， 1， 2然后乘以3会把列映射成0， 3， 6。然后j/3可以把列分成3份，分别是0， 1， 2.把这两个数加起来就可以把一个cub里的数字映射成cubUsed里的一行。例如i = 4， j = 4, 就会映射到第四行里。i = 1，j = 8的话映射到第二行（这个小格子属于大格子2，所以第二行）。这是把一个个cub映射成一行。第一个\[0, 0]的cub映射成cub\[0]\[X]的第一行。\[0, 1]-->cub\[1]\[X]

```java
public boolean isValidSudoku(char[][] board) {
    if (board == null || board.length != 9 || board[0].length != 9) {
        return false;
    }

    // use 3 matrix to mark what number is already used
    boolean[][] rowUsed = new boolean[9][9];
    boolean[][] colUsed = new boolean[9][9];
    boolean[][] cubUsed = new boolean[9][9];

    for (int i = 0; i < 9; i++) {
        for (int j = 0; j < 9; j++) {
            if (board[i][j] != '.') {
                int num = board[i][j] - 1 - '0';
                if (rowUsed[i][num] || colUsed[num][j] || cubUsed[3 * (i / 3) + (j / 3)][num]) {
                    return false;
                }
                rowUsed[i][num] = true;
                colUsed[num][j] = true;
                cubUsed[3 * (i / 3) + (j / 3)][num] = true;
            }
        }
    }
    return true;
}
```

这题主要是难控制下标。

```java
public boolean isValidSudoku(char[][] board) {
    if (board == null || board.length != 9 || board[0].length != 9) {
        return false;
    }

    boolean[] hashTable = new boolean[9];
    // check cols validity
    for (int j = 0; j < 9; j++) {
        for (int i = 0; i < 9; i++) {
            if (board[i][j] != '.') {
                if (hashTable[board[i][j] - 1 - '0']) {
                    return false;
                } else {
                    hashTable[board[i][j] - 1 - '0'] = true;
                }
            }
        }
        Arrays.fill(hashTable, false);
    }

    // check rows validity
    for (int i = 0; i < 9; i++) {
        for (int j = 0; j < 9; j++) {
            if (board[i][j] != '.' ) {
                if (hashTable[board[i][j] - 1 - '0']) {
                    return false;
                } else {
                    hashTable[board[i][j] - 1 - '0'] = true;
                }
            }
        }
        Arrays.fill(hashTable, false);
    }

    // check each little squares
    for (int i = 0; i < 9; i += 3) {
        for (int j = 0; j < 9; j += 3) {

            for (int i2 = i; i2 < 3 + i; i2++) {
                for (int j2 = j; j2 < 3 + j; j2++) {
                    if (board[i2][j2] != '.') {
                        if (hashTable[board[i2][j2] - 1 - '0']) {
                            return false;
                        } else {
                            hashTable[board[i2][j2] - 1 - '0'] = true;
                        }
                    }
                }
            }

            Arrays.fill(hashTable, false);    
        }
    }

    return true;
}
```

下面是discuss里大神的手稿：

```java
public boolean isValidSudoku(char[][] board) {
    for(int i = 0; i<9; i++){
        HashSet<Character> rows = new HashSet<Character>();
        HashSet<Character> columns = new HashSet<Character>();
        HashSet<Character> cube = new HashSet<Character>();
        for (int j = 0; j < 9;j++){
            if(board[i][j]!='.' && !rows.add(board[i][j]))
                return false;
            if(board[j][i]!='.' && !columns.add(board[j][i]))
                return false;
            int RowIndex = 3*(i/3);
            int ColIndex = 3*(i%3);
            if(board[RowIndex + j/3][ColIndex + j%3]!='.' && !cube.add(board[RowIndex + j/3][ColIndex + j%3]))
                return false;
        }
    }
    return true;
}
```
