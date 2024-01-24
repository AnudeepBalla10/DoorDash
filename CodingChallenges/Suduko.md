# Suduko Validation

```java
class Solution {
    public boolean isValidSudoku(char[][] board) {
        HashSet<String> seen = new HashSet();
        for(int i =0; i<9;i++){
            for(int j=0; j< 9; j++){
                char currentVal = board[i][j];
                if(currentVal != '.'){
                if(!seen.add(currentVal+" found in row " + i) ||
                   !seen.add(currentVal+" found in coloum " + j) ||
                   !seen.add(currentVal+" found in box " + i/3 + "-" + j/3)
                )return false;
                }
            }
        }
        return true;
    }
}
```

# Suduko Solver

```java
class Solution {
    public boolean isSafe(char[][] board, int row, int col, int num) {
        // row and col
        for (int i = 0; i < board.length; i++) {
            if (board[i][col] == (char) (num + '0'))
                return false;

            if (board[row][i] == (char) (num + '0'))
                return false;
        }

        // grid
        int sr = (row / 3) * 3;
        int sc = (col / 3) * 3;

        for (int i = sr; i < sr + 3; i++) {
            for (int j = sc; j < sc + 3; j++) {
                if (board[i][j] == (char)(num + '0'))
                    return false;
            }
        }
        return true;
    }

    public boolean helper(char[][] board, int row, int col) {
        if (row == board.length)
            return true;
        int nrow = 0, ncol = 0;
        // checking if its the last col, if not move to next col else go to new row with
        // col=0;
        if (col != board.length) {
            nrow = row;
            ncol = col + 1;
        } else {
            nrow = row + 1;
            ncol = 0;
        }

        // checking if the number is already written in that spot;
        if (board[row][col] != '.') {
            return helper(board, nrow, ncol);
        } else {
            //fill the places
            for (int i = 1; i <= 9; i++) {
                if (isSafe(board, row, col, i)) {
                    board[row][col] = (char) (i + '0');
                    if (helper(board, nrow, ncol)) 
                        return true;
                    else
                        board[row][col] = '.';
                }
            }
        }
        return false;
    }

    public void solveSudoku(char[][] board) {
        helper(board, 0, 0);// row =0; col = 0;

    }
}
```
