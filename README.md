# juego
Juego en phyton
import random

def drawBoard(tablero):
    
    print('   |    |')
    print(' ' + tablero[1] + ' | ' + tablero[2] + '  | ' + tablero[3])
    print('   |    |')
    print('--------------')
    print('   |    |')
    print(' ' + tablero[4] + ' | ' + tablero[5] + '  | ' + tablero[6])
    print('   |    |')
    print('--------------')
    print('   |    |')
    print(' ' + tablero[7] + ' | ' + tablero[8] + '  | ' + tablero[9])
    print('   |    |')

def inputPlayerLetter():
    
    letter = ''
    while not (letter == 'X' or letter == 'O'):
        print('ESCOGE UNA FICHA X o O?')
        letter = input().upper()

   
    if letter == 'X':
        return ['X', 'O']
    else:
        return ['O', 'X']

def whoGoesFirst():
   
    if random.randint(0, 1) == 0:
        return 'COMPUTADORA'
    else:
        return 'TU'

def playAgain():
   
    print('VOLVER A JUAGR? (y o n)')
    return input().lower().startswith('y')

def makeMove(board, letter, move):
    board[move] = letter

def isWinner(bo, le):
   
    return ((bo[7] == le and bo[8] == le and bo[9] == le) or 
    (bo[4] == le and bo[5] == le and bo[6] == le) or 
    (bo[1] == le and bo[2] == le and bo[3] == le) or 
    (bo[7] == le and bo[4] == le and bo[1] == le) or 
    (bo[8] == le and bo[5] == le and bo[2] == le) or 
    (bo[9] == le and bo[6] == le and bo[3] == le) or 
    (bo[7] == le and bo[5] == le and bo[3] == le) or 
    (bo[9] == le and bo[5] == le and bo[1] == le)) 

def getBoardCopy(board):
   
    dupeBoard = []

    for i in board:
        dupeBoard.append(i)

    return dupeBoard

def isSpaceFree(board, move):
   
    return board[move] == ' '

def getPlayerMove(board):
  
    move = ' '
    while move not in '1 2 3 4 5 6 7 8 9'.split() or not isSpaceFree(board, int(move)):
        print('TU SIGUIENTE MOVIMIENTO? (1-9)')
        move = input()
    return int(move)

def chooseRandomMoveFromList(board, movesList):
    
    possibleMoves = []
    for i in movesList:
        if isSpaceFree(board, i):
            possibleMoves.append(i)

    if len(possibleMoves) != 0:
        return random.choice(possibleMoves)
    else:
        return None

def getComputerMove(board, computerLetter):
    
    if computerLetter == 'X':
        playerLetter = 'O'
    else:
        playerLetter = 'X'

   
    for i in range(1, 10):
        copy = getBoardCopy(board)
        if isSpaceFree(copy, i):
            makeMove(copy, computerLetter, i)
            if isWinner(copy, computerLetter):
                return i

    
    for i in range(1, 10):
        copy = getBoardCopy(board)
        if isSpaceFree(copy, i):
            makeMove(copy, playerLetter, i)
            if isWinner(copy, playerLetter):
                return i

    
    move = chooseRandomMoveFromList(board, [1, 3, 7, 9])
    if move != None:
        return move

    
    if isSpaceFree(board, 5):
        return 5

    
    return chooseRandomMoveFromList(board, [2, 4, 6, 8])

def isBoardFull(board):
    
    for i in range(1, 10):
        if isSpaceFree(board, i):
            return False
    return True


print('JUEGO DE TRES EN RAYA')

while True:
   
    theBoard = [' '] * 10
    playerLetter, computerLetter = inputPlayerLetter()
    turn = whoGoesFirst()
    print('LA ' + turn + ' ES PRIMERO.')
    gameIsPlaying = True

    while gameIsPlaying:
        if turn == 'TU':
           
            drawBoard(theBoard)
            move = getPlayerMove(theBoard)
            makeMove(theBoard, playerLetter, move)

            if isWinner(theBoard, playerLetter):
                drawBoard(theBoard)
                print('FELICIDADES LO LOGRASTE!')
                gameIsPlaying = False
            else:
                if isBoardFull(theBoard):
                    drawBoard(theBoard)
                    print('NINGUNO GANO!')
                    break
                else:
                    turn = 'COMPUTADORA'

        else:
           
            move = getComputerMove(theBoard, computerLetter)
            makeMove(theBoard, computerLetter, move)

            if isWinner(theBoard, computerLetter):
                drawBoard(theBoard)
                print('PERDISTE ESFUERZATE MAS.')
                gameIsPlaying = False
            else:
                if isBoardFull(theBoard):
                    drawBoard(theBoard)
                    print('EMPATE!')
                    break
                else:
                    turn = 'TU'

    if not playAgain():
        break
