# TICTACTOE
#TIC TAC TOE game using computer graphics 
#include <graphics.h>
#include <conio.h>
#include <iostream>
using namespace std;

int board[3][3]; // 0 = empty, 1 = X, 2 = O
int currentPlayer = 1;

// Draw X
void drawX(int x, int y) {
    setcolor(RED);
    setlinestyle(SOLID_LINE, 0, 5);
    line(x+20, y+20, x+180, y+180);
    line(x+180, y+20, x+20, y+180);
}

// Draw O
void drawO(int x, int y) {
    setcolor(BLUE);
    setlinestyle(SOLID_LINE, 0, 5);
    circle(x+100, y+100, 80);
}

void drawBoard() {
    setcolor(WHITE);
    setlinestyle(SOLID_LINE, 0, 5);

    // Vertical lines
    line(200, 0, 200, 600);
    line(400, 0, 400, 600);

    // Horizontal lines
    line(0, 200, 600, 200);
    line(0, 400, 600, 400);

    // Draw existing X/O
    int x = 0, y = 0;
    for(int i=0;i<3;i++){
        for(int j=0;j<3;j++){
            int px = j * 200;
            int py = i * 200;
            if(board[i][j] == 1) drawX(px, py);
            else if(board[i][j] == 2) drawO(px, py);
        }
    }
}

// Winner check
int checkWinner() {
    // Rows & Columns
    for(int i=0;i<3;i++){
        if(board[i][0] == board[i][1] && board[i][1] == board[i][2] && board[i][0] != 0)
            return board[i][0];
        if(board[0][i] == board[1][i] && board[1][i] == board[2][i] && board[0][i] != 0)
            return board[0][i];
    }

    // Diagonals
    if(board[0][0] == board[1][1] && board[1][1] == board[2][2] && board[0][0] != 0)
        return board[0][0];

    if(board[0][2] == board[1][1] && board[1][1] == board[2][0] && board[0][2] != 0)
        return board[0][2];

    return 0;
}

// Check if full
bool isFull(){
    for(int i=0;i<3;i++)
        for(int j=0;j<3;j++)
            if(board[i][j] == 0) return false;
    return true;
}

int main() {
    int gd = DETECT, gm;
    initgraph(&gd, &gm, (char*)"");

    setbkcolor(BLACK);
    cleardevice();

    // Initialize board
    for(int i=0;i<3;i++)
        for(int j=0;j<3;j++)
            board[i][j] = 0;

    int winner = 0;

    while(true) {
        drawBoard();

        if(winner != 0){
            setcolor(YELLOW);
            settextstyle(10, 0, 4);
            if(winner == 1)
                outtextxy(150, 250, (char*)"PLAYER X WINS!");
            else
                outtextxy(150, 250, (char*)"PLAYER O WINS!");

            getch();
            break;
        }

        if(isFull()){
            setcolor(YELLOW);
            settextstyle(10, 0, 4);
            outtextxy(180, 250, (char*)"DRAW!");
            getch();
            break;
        }

        if(ismouseclick(WM_LBUTTONDOWN)){
            int x, y;
            getmouseclick(WM_LBUTTONDOWN, x, y);

            int r = y / 200;
            int c = x / 200;

            if(board[r][c] == 0){
                board[r][c] = currentPlayer;
                currentPlayer = (currentPlayer == 1) ? 2 : 1;
            }

            winner = checkWinner();
        }
    }

    getch();
    closegraph();
    return 0;
}



   
     
