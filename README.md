# jogo da velha (fordius)

import tkinter as tk
from tkinter import messagebox

# Função para verificar se há um vencedor
def check_winner():
    # Possíveis combinações vencedoras
    combinations = [
        [0, 1, 2], [3, 4, 5], [6, 7, 8],  # Linhas
        [0, 3, 6], [1, 4, 7], [2, 5, 8],  # Colunas
        [0, 4, 8], [2, 4, 6]              # Diagonais
    ]
    
    for combo in combinations:
        if board[combo[0]] == board[combo[1]] == board[combo[2]] != "":
            return board[combo[0]]
    
    # Se não houver vencedor, verificar empate
    if all(cell != "" for cell in board):
        return "Empate"
    
    return None

# Função chamada quando o botão é pressionado
def on_button_click(index):
    global current_player
    
    if board[index] == "" and not game_over:
        board[index] = current_player
        buttons[index].config(text=current_player)
        
        winner = check_winner()
        
        if winner:
            end_game(winner)
        else:
            # Alternar jogador
            current_player = "O" if current_player == "X" else "X"

# Função para encerrar o jogo
def end_game(winner):
    global game_over
    game_over = True
    
    if winner == "Empate":
        messagebox.showinfo("Fim do Jogo", "O jogo terminou em empate!")
    else:
        messagebox.showinfo("Fim do Jogo", f"O jogador {winner} venceu!")
    
    # Reiniciar o jogo
    reset_board()

# Função para reiniciar o tabuleiro
def reset_board():
    global board, game_over, current_player
    board = [""] * 9
    game_over = False
    current_player = "X"
    
    for button in buttons:
        button.config(text="")

# Inicializando a janela do Tkinter
root = tk.Tk()
root.title("Jogo da Velha")

# Variáveis do jogo
board = [""] * 9
game_over = False
current_player = "X"

# Criando os botões do tabuleiro
buttons = []
for i in range(9):
    button = tk.Button(root, text="", font=("Arial", 24), width=7, height=3, command=lambda i=i: on_button_click(i))
    button.grid(row=i//3, column=i%3)
    buttons.append(button)

# Iniciando o loop principal da interface
root.mainloop()
