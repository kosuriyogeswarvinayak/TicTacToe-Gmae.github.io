# TicTacToe-Gmae.github.io
Game between human and AI
import java.util.Random;
import java.util.Scanner;

public class TicTacToe {
    private char[][] board;
    private char currentPlayer;
    private Random random;

    public TicTacToe() {
        board = new char[3][3];
        currentPlayer = 'X';
        random = new Random();
        initializeBoard();
    }

    public void initializeBoard() {
        for (int i = 0; i < 3; i++) {
            for (int j = 0; j < 3; j++) {
                board[i][j] = '-';
            }
        }
    }

    public void printBoard() {
        System.out.println("-------------");
        for (int i = 0; i < 3; i++) {
            System.out.print("| ");
            for (int j = 0; j < 3; j++) {
                System.out.print(board[i][j] + " | ");
            }
            System.out.println();
            System.out.println("-------------");
        }
    }

    public boolean isBoardFull() {
        for (int i = 0; i < 3; i++) {
            for (int j = 0; j < 3; j++) {
                if (board[i][j] == '-') {
                    return false;
                }
            }
        }
        return true;
    }

    public boolean checkForWin() {
        return (checkRowsForWin() || checkColumnsForWin() || checkDiagonalsForWin());
    }

    private boolean checkRowsForWin() {
        for (int i = 0; i < 3; i++) {
            if (checkRowCol(board[i][0], board[i][1], board[i][2])) {
                return true;
            }
        }
        return false;
    }

    private boolean checkColumnsForWin() {
        for (int i = 0; i < 3; i++) {
            if (checkRowCol(board[0][i], board[1][i], board[2][i])) {
                return true;
            }
        }
        return false;
    }

    private boolean checkDiagonalsForWin() {
        return (checkRowCol(board[0][0], board[1][1], board[2][2]) || checkRowCol(board[0][2], board[1][1], board[2][0]));
    }

    private boolean checkRowCol(char c1, char c2, char c3) {
        return ((c1 != '-') && (c1 == c2) && (c2 == c3));
    }

    public void changePlayer() {
        currentPlayer = (currentPlayer == 'X') ? 'O' : 'X';
    }

    public boolean placeMark(int row, int col) {
        if (row >= 0 && row < 3 && col >= 0 && col < 3 && board[row][col] == '-') {
            board[row][col] = currentPlayer;
            return true;
        }
        return false;
    }

    public void aiMakeMove() {
        int row, col;
        do {
            row = random.nextInt(3);
            col = random.nextInt(3);
        } while (!placeMark(row, col));
    }

    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);
        TicTacToe game = new TicTacToe();
        int row, col;

        while (!game.isBoardFull() && !game.checkForWin()) {
            game.printBoard();
            System.out.println("Player " + game.currentPlayer + ", enter your move (row and column): ");

            if (game.currentPlayer == 'X') {
                row = scanner.nextInt();
                col = scanner.nextInt();
                if (!game.placeMark(row, col)) {
                    System.out.println("Invalid move. Try again.");
                    continue;
                }
            } else {
                game.aiMakeMove();
            }

            if (game.checkForWin()) {
                game.printBoard();
                if (game.currentPlayer == 'X') {
                    System.out.println("Player X wins!");
                } else {
                    System.out.println("AI wins!");
                }
                break;
            } else if (game.isBoardFull()) {
                game.printBoard();
                System.out.println("It's a tie!");
                break;
            } else {
                game.changePlayer();
            }
        }
        scanner.close();
    }
}