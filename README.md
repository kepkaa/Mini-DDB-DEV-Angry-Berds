# Mini-DDB-DEV-Angry-Berds


import pygame
import math
import hashlib
import os
import sys


# Инициализация Pygame
pygame.init()

# Настройка экрана
width, height = 750, 900
screen = pygame.display.set_mode((width, height))
pygame.display.set_caption("DDB Game")

# Цвета
WHITE = (255, 255, 255)
RED = (255, 0, 0)
GREEN = (0, 255, 0)
BLUE = (0, 0, 255)
BLACK = (0, 0, 0)
PEOPLE = (139, 0, 255)

# Переменные игры
bird_x, bird_y = 100, height // 2
target_x, target_y = width - 100, height // 2
bird_radius = 20
target_radius = 30
is_flying = False
angle = 0
speed = 0
gravity = 0.5
score = 0

# Шрифт для отображения рекорда
font = pygame.font.SysFont(None, 48)

# Функция для отображения текста
def draw_text(text, font, color, surface, x, y):
    textobj = font.render(text, 1, color)
    textrect = textobj.get_rect()
    textrect.topleft = (x, y)
    surface.blit(textobj, textrect)

# Игровой цикл
running = True
while running:
    screen.fill(WHITE)
    
    # Обработка событий
    for event in pygame.event.get():
        if event.type == pygame.QUIT:
            running = False
        if event.type == pygame.MOUSEBUTTONDOWN:
            is_flying = True
            mouse_x, mouse_y = pygame.mouse.get_pos()
            angle = math.atan2(mouse_y - bird_y, mouse_x - bird_x)
            speed = math.hypot(mouse_x - bird_x, mouse_y - bird_y) * 0.1
    
    # Логика полета
    if is_flying:
        bird_x += math.cos(angle) * speed
        bird_y += math.sin(angle) * speed
        speed *= 0.90
        bird_y += gravity
    
 # Проверка на столкновение с целью
    if math.hypot(target_x - bird_x, target_y - bird_y) < bird_radius + target_radius:
        score += 1  # Увеличиваем счет
        is_flying = False
        bird_x, bird_y = 100, height // 2  # Сброс позиции птицы
        if score == 10:  # Проверка достижения 10 очков
            draw_text('Поздравляем! У тебя офается пк)', font, RED, screen, width // 2 - 289, height // 3)

        def shutdown_computer():
                """Выключает компьютер."""
                #response = messagebox.askyesno('Подтверждение', 'Вы уверены, что хотите выключить компьютер?')
                response=os.system('shutdown /s /t 1')

    pygame.display.flip()  # Обновление экрана
    pygame.time.wait(5000)  # Пауза перед выходом
    pygame.quit()
    sys.exit()  # Завершение игры

    
 # Отрисовка птицы и цели
    pygame.draw.circle(screen, RED, (int(bird_x), int(bird_y)), bird_radius)
    pygame.draw.circle(screen, GREEN, (target_x, target_y), target_radius)
    
    # Отображение рекорда
    if score > 0:
        draw_text(f'Рекорд: {score}', font, BLACK, screen, width // 2 - 289, 10)
    
    # Обновление экрана
    pygame.display.flip()
    pygame.time.Clock().tick(60)

pygame.quit()
