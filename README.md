# Python_project
 import pygame
import time
import random

pygame.init()

# Размер экрана
width = 800
height = 600

# Цвета
black = (0, 0, 0)
white = (255, 255, 255)
red = (255, 0, 0)
green = (0, 255, 0)

# Размер змейки
block_size = 20

# Скорость змейки
speed = 20

screen = pygame.display.set_mode((width, height))
pygame.display.set_caption('Snake Game')

# Основная функция игры
def gameLoop():
    game_over = False
    game_close = False

    x = width / 2
    y = height / 2

    x_change = 0
    y_change = 0

    snakeList = []
    length = 1

    
    foodx = round(random.randrange(0, width - block_size) / 20) * 20
    foody = round(random.randrange(0, height - block_size) / 20) * 20

    while not game_over:
        while game_close == True:
            screen.fill(white)
            font = pygame.font.SysFont(None, 30)
            text = font.render("You Lost! Press Q-Quit or C-Play Again", True, red)
            screen.blit(text, [width / 3, height / 3])
            pygame.display.update()

            for event in pygame.event.get():
                if event.type == pygame.KEYDOWN:
                    if event.key == pygame.K_q:
                        game_over = True
                        game_close = False
                    if event.key == pygame.K_c:
                        gameLoop()

        for event in pygame.event.get():
            if event.type == pygame.QUIT:
                game_over = True
            if event.type == pygame.KEYDOWN:
                if event.key == pygame.K_LEFT:
                    x_change = -block_size
                    y_change = 0
                elif event.key == pygame.K_RIGHT:
                    x_change = block_size
                    y_change = 0
                elif event.key == pygame.K_UP:
                    y_change = -block_size
                    x_change = 0
                elif event.key == pygame.K_DOWN:
                    y_change = block_size
                    x_change = 0

        if x >= width or x < 0 or y >= height or y < 0:
            game_close = True

        x += x_change
        y += y_change
        screen.fill(black)
        pygame.draw.rect(screen, green, [foodx, foody, block_size, block_size])

        snakeHead = []
        snakeHead.append(x)
        snakeHead.append(y)
        snakeList.append(snakeHead)
        
        if len(snakeList) > length:
            del snakeList[0]
        
        for segment in snakeList[:-1]:
            if segment == snakeHead:
                game_close = True
        
        for segment in snakeList:
            pygame.draw.rect(screen, white, [segment[0], segment[1], block_size, block_size])

        pygame.display.update()

        if x == foodx and y == foody:
            length += 1
            foodx = round(random.randrange(0, width - block_size) / 20) * 20
            foody = round(random.randrange(0, height - block_size) / 20) * 20

        time.sleep(0.1)

    pygame.quit()
    quit()

gameLoop()



