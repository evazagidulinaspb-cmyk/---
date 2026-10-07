```python
import pygame

pygame.init()

w, h = 600, 600
size = 200
screen = pygame.display.set_mode((w, h))
pygame.display.set_caption("Крестики нолики")

WHITE = (255, 255, 255)
BLACK = (0, 0, 0)
RED = (220, 60, 60)
BLUE = (60, 130, 220)
PINK = (255, 105, 180)
font = pygame.font.SysFont(None, 160)

whose_move = 1
game_over = False
winner_flag = 0
game_matrix = [[0, 0, 0], [0, 0, 0], [0, 0, 0]]


def draw_game():
    screen.fill(WHITE)

    pygame.draw.line(screen, BLACK, (size, 0), (size, h), 8)
    pygame.draw.line(screen, BLACK, (2 * size, 0), (2 * size, h), 8)
    pygame.draw.line(screen, BLACK, (0, size), (w, size), 8)
    pygame.draw.line(screen, BLACK, (0, 2 * size), (w, 2 * size), 8)

    for i in range(3):
        for j in range(3):
            x = j * size + 100
            y = i * size + 100
            if game_matrix[i][j] == 1:
                pygame.draw.line(screen, RED, (x - 30, y - 30), (x + 30, y + 30), 15)
                pygame.draw.line(screen, RED, (x + 30, y - 30), (x - 30, y + 30), 15)
            elif game_matrix[i][j] == 2:
                pygame.draw.circle(screen, BLUE, (x, y), 40, 13)

    if game_over:
        if winner_flag == 1:
            message = "You won!"
        elif winner_flag == 2:
            message = "You lose"
        else:
            message = "Try again"
        text = font.render(message, True, PINK)
        screen.blit(text, (60, 250))


def check():
    m = game_matrix
    if m[0][0] == m[0][1] == m[0][2] and m[0][0] != 0:
        return m[0][0]
    if m[1][0] == m[1][1] == m[1][2] and m[1][0] != 0:
        return m[1][0]
    if m[2][0] == m[2][1] == m[2][2] and m[2][0] != 0:
        return m[2][0]
    if m[0][0] == m[1][0] == m[2][0] and m[0][0] != 0:
        return m[0][0]
    if m[0][1] == m[1][1] == m[2][1] and m[0][1] != 0:
        return m[0][1]
    if m[0][2] == m[1][2] == m[2][2] and m[0][2] != 0:
        return m[0][2]
    if m[0][0] == m[1][1] == m[2][2] and m[0][0] != 0:
        return m[0][0]
    if m[0][2] == m[1][1] == m[2][0] and m[0][2] != 0:
        return m[0][2]

    return 0


def end():
    for i in range(3):
        for j in range(3):
            if game_matrix[i][j] == 0:
                return False
    return True


def comp_move():
    m = game_matrix
    for i in range(3):
        for j in range(3):
            if m[i][j] == 0:
                m[i][j] = 2
                if check() == 2:
                    return
                m[i][j] = 0
    for i in range(3):
        for j in range(3):
            if m[i][j] == 0:
                m[i][j] = 1
                if check() == 1:
                    m[i][j] = 2
                    return
                m[i][j] = 0
    if m[1][1] == 0:
        m[1][1] = 2
        return
    for i in range(3):
        for j in range(3):
            if m[i][j] == 0:
                m[i][j] = 2
                return

def restart():
    global whose_move, game_over, winner_flag, game_matrix
    whose_move = 1
    game_over = False
    winner_flag = 0
    game_matrix = [[0, 0, 0], [0, 0, 0], [0, 0, 0]]


done = False
while not done:
    for event in pygame.event.get():
        if event.type == pygame.QUIT:
            done = True
        if event.type == pygame.MOUSEBUTTONDOWN and not game_over and whose_move == 1:
            x, y = event.pos
            i = y // size
            j = x // size
            if game_matrix[i][j] == 0:
                game_matrix[i][j] = 1
                whose_move = 2
    if pygame.key.get_pressed()[pygame.K_r]:
        restart()
    if not game_over and whose_move == 2:
        comp_move()
        whose_move = 1

    who_wins = check()
    if who_wins != 0:
        winner_flag = who_wins
        game_over = True
    elif end():
        winner_flag = 0
        game_over = True

    draw_game()

    pygame.display.flip()
pygame.quit()
