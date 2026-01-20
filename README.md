# TicTacToe


import java.util.Scanner;

import java.util.Random;

public class TicTacToeGame {

    
    
    static abstract class Player {
        String name;
        char mark;

        public Player(String name, char mark) {
            this.name = name;
            this.mark = mark;
        }

        abstract void makeMove();
    }

 
    static class HumanPlayer extends Player {

        public HumanPlayer(String name, char mark) {
            super(name, mark);
        }

        @Override
        void makeMove() {
            Scanner sc = new Scanner(System.in);
            int row, col;

            do {
                System.out.println(name + ", enter row and column (0-2): ");
                row = sc.nextInt();
                col = sc.nextInt();
            } while (!TicTacToe.placeMark(row, col, mark));
        }
    }

    static class AIPlayer extends Player {

        public AIPlayer(String name, char mark) {
            super(name, mark);
        }

        @Override
        void makeMove() {
            Random r = new Random();
            int row, col;

            do {
                row = r.nextInt(3);
                col = r.nextInt(3);
            } while (!TicTacToe.placeMark(row, col, mark));

            System.out.println(name + " placed at: " + row + " " + col);
        }
    }

    static class TicTacToe {

        static char[][] board;

        public TicTacToe() {
            board = new char[3][3];
            initBoard();
        }

        static void initBoard() {
            for (int i = 0; i < 3; i++) {
                for (int j = 0; j < 3; j++) {
                    board[i][j] = ' ';
                }
            }
        }
        static void displayBoard() {
            System.out.println();
            System.out.println("    0   1   2");
            System.out.println("  -------------");
            for (int i = 0; i < 3; i++) {
                System.out.print(i + " | ");
                for (int j = 0; j < 3; j++) {
                    System.out.print(board[i][j] + " | ");
                }
                System.out.println();
                System.out.println("  -------------");
            }
            System.out.println();
        }


        static boolean placeMark(int row, int col, char mark) {
            if (row >= 0 && row <= 2 && col >= 0 && col <= 2) {
                if (board[row][col] == ' ') {
                    board[row][col] = mark;
                    return true;
                }
            }
            System.out.println("Invalid move! Try again.");
            return false;
        }

        static boolean checkRowWin() {
            for (int i = 0; i < 3; i++) {
                if (board[i][0] != ' ' &&
                    board[i][0] == board[i][1] &&
                    board[i][1] == board[i][2]) {
                    return true;
                }
            }
            return false;
        }

        static boolean checkColumnWin() {
            for (int j = 0; j < 3; j++) {
                if (board[0][j] != ' ' &&
                    board[0][j] == board[1][j] &&
                    board[1][j] == board[2][j]) {
                    return true;
                }
            }
            return false;
        }

        static boolean checkDiagonalWin() {
            if (board[0][0] != ' ' &&
                board[0][0] == board[1][1] &&
                board[1][1] == board[2][2]) {
                return true;
            }

            if (board[0][2] != ' ' &&
                board[0][2] == board[1][1] &&
                board[1][1] == board[2][0]) {
                return true;
            }

            return false;
        }

        static boolean checkWin() {
            return checkRowWin() || checkColumnWin() || checkDiagonalWin();
        }

        static boolean checkDraw() {
            for (int i = 0; i < 3; i++) {
                for (int j = 0; j < 3; j++) {
                    if (board[i][j] == ' ') {
                        return false;
                    }
                }
            }
            return true;
        }
    }

    public static void main(String[] args) {

        TicTacToe game = new TicTacToe();
        TicTacToe.displayBoard();

        // Change here:
        Player p1 = new HumanPlayer("Bob", 'X');
        Player p2 = new AIPlayer("TapAI", 'O');  
        // For 2 humans, use:
        // Player p2 = new HumanPlayer("Priya", 'O');

        Player currentPlayer = p1;

        while (true) {

            System.out.println(currentPlayer.name + "'s turn");
            currentPlayer.makeMove();
            TicTacToe.displayBoard();

            if (TicTacToe.checkWin()) {
                System.out.println(currentPlayer.name + " has WON the game!");
                break;
            }

            if (TicTacToe.checkDraw()) {
                System.out.println("Game is a DRAW!");
                break;
            }

            // Switch turn
            if (currentPlayer == p1) {
                currentPlayer = p2;
            } else {
                currentPlayer = p1;
            }
        }
    }
}
