# task2 tic tac toe game 
import random

def print_board(board):
    for row in board:
        print(" ".join(row))

def is_winner(board, player):
    # Check rows, columns, and diagonals for a win
    return any(all(cell == player for cell in row) for row in board) or \
           any(all(row[i] == player for row in board) for i in range(3)) or \
           all(board[i][i] == player for i in range(3)) or \
           all(board[i][2 - i] == player for i in range(3))

def is_board_full(board):
    return all(cell != ' ' for row in board for cell in row)

def get_empty_cells(board):
    return [(i, j) for i in range(3) for j in range(3) if board[i][j] == ' ']

def minimax(board, depth, maximizing_player):
    if is_winner(board, 'O'):
        return -1
    elif is_winner(board, 'X'):
        return 1
    elif is_board_full(board):
        return 0

    if maximizing_player:
        max_eval = float('-inf')
        for i, j in get_empty_cells(board):
            board[i][j] = 'X'
            eval = minimax(board, depth + 1, False)
            board[i][j] = ' '
            max_eval = max(max_eval, eval)
        return max_eval
    else:
        min_eval = float('inf')
        for i, j in get_empty_cells(board):
            board[i][j] = 'O'
            eval = minimax(board, depth + 1, True)
            board[i][j] = ' '
            min_eval = min(min_eval, eval)
        return min_eval

def best_move(board):
    best_val = float('-inf')
    move = None
    for i, j in get_empty_cells(board):
        board[i][j] = 'X'
        eval = minimax(board, 0, False)
        board[i][j] = ' '
        if eval > best_val:
            best_val = eval
            move = (i, j)
    return move

def play_tic_tac_toe():
    board = [[' ' for _ in range(3)] for _ in range(3)]
    player = 'O' if random.choice([True, False]) else 'X'

    while not is_board_full(board):
        print_board(board)

        if player == 'O':
            i, j = best_move(board)
        else:
            i, j = map(int, input("Enter your move (row col): ").split())

        if board[i][j] == ' ':
            board[i][j] = player

            if is_winner(board, player):
                print_board(board)
                print(f"{player} wins!")
                break
            else:
                player = 'O' if player == 'X' else 'X'
        else:
            print("Cell already occupied. Try again.")

    if not is_winner(board, 'O') and not is_winner(board, 'X'):
        print_board(board)
        print("It's a draw!")

# Example usage
play_tic_tac_toe(\
