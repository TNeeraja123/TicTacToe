# TicTacToe
import random
import time

# Display the game board with box formatting
def display_board(board):
    print("\n")
    print("     |      |     ")
    print(f"  {board[0]}  |  {board[1]}  |  {board[2]}  ")
    print("____ | ____ | _____")
    print("     |      |      ")
    print(f"  {board[3]}  |  {board[4]}  |  {board[5]}  ")
    print("_____|_____ | _____")
    print("     |      |     ")
    print(f"  {board[6]}  |  {board[7]}  |  {board[8]}  ")
    print("     |      |     ")
    print("\n")

# Check if a player has won
def check_win(board, player):
    win_conditions = [(0, 1, 2), (3, 4, 5), (6, 7, 8), 
                      (0, 3, 6), (1, 4, 7), (2, 5, 8), 
                      (0, 4, 8), (2, 4, 6)]
    return any(board[a] == board[b] == board[c] == player for a, b, c in win_conditions)

# Check if the board is full
def check_full(board):
    return all(space != ' ' for space in board)

# Player move
def player_move(board, player):
    while True:
        try:
            move = int(input(f"Player {player}, enter your move (1-9): ")) - 1
            if board[move] == ' ':
                board[move] = player
                break
            else:
                print("This space is already taken. Choose another one.")
        except (IndexError, ValueError):
            print("Invalid input. Please enter a number between 1 and 9.")

# Computer move with basic AI
def computer_move(board, player):
    # Check if computer can win
    for i in range(9):
        if board[i] == ' ':
            board[i] = player
            if check_win(board, player):
                return
            board[i] = ' '
    
    # Block player's winning move
    opponent = 'X' if player == 'O' else 'O'
    for i in range(9):
        if board[i] == ' ':
            board[i] = opponent
            if check_win(board, opponent):
                board[i] = player
                return
            board[i] = ' '
    
    # Choose a random available spot
    available_moves = [i for i, spot in enumerate(board) if spot == ' ']
    board[random.choice(available_moves)] = player

def main():
    print("Welcome to Tic Tac Toe!")
    board = [' '] * 9
    current_player = 'X'
    
    game_mode = input("Enter '1' for two-player mode or '2' to play against the computer: ")
    computer = game_mode == '2'

    while True:
        display_board(board)
        
        if computer and current_player == 'O':
            computer_move(board, current_player)
        else:
            player_move(board, current_player)
        
        display_board(board)
        print("\n" + "-"*20 + "\n")  # Provide a gap after each move
        time.sleep(1)  # Pause for 1 second to give players time to see the updated board

        if check_win(board, current_player):
            display_board(board)
            print(f"Player {current_player} wins!")
            break
        
        if check_full(board):
            display_board(board)
            print("It's a tie!")
            break
        
        current_player = 'O' if current_player == 'X' else 'X'

if __name__ == "__main__":
    main()
