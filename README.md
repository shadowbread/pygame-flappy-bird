# Flappy Bird
___
## Код
*Импорт и создание переменных*
```python
import pygame
import random
pygame.init()

screen = pygame.display.set_mode((400, 400))
player_x = 50
player_y = 50
fpsclck = pygame.time.Clock()
tube_x = 300
tube_y = 150
score = 0
mode = "game"
record = 0

run = True
```
*Рекорд*
```python
def get_record(): # достать рекорд из файла
    global record
    try:
        open('record.txt', 'r')
    except FileNotFoundError:
        f = open('record.txt', 'w')
        f.write("0")
        f.close()
    r = open('record.txt', 'r')
    record = int(r.read())
get_record()

def check_record(): # проверка на рекорд и запись его в файл
    global score, record
    if score > record:
        record = score
        f = open('record.txt', 'w')
        f.write(str(record))
        f.close()
    score = 0
```
*Игровой цикл*
```python
def game():
    global screen, player_x, player_y, tube_x, tube_y, score, record
    pygame.display.set_caption("[Flappy bird] Очки: " + str(score) + " Рекорд: " + str(record)) # переименнование окна
    # отрисовка спрайтов
    screen.fill((0, 0, 0))
    pygame.draw.rect(screen, (0, 255, 0), (tube_x, 0, 50, 400))
    pygame.draw.rect(screen, (0, 200, 0), (tube_x - 10, tube_y - 20, 70, 90))
    pygame.draw.rect(screen, (0, 0, 0), (tube_x - 10, tube_y, 70, 50))
    pygame.draw.rect(screen, (255, 255, 0), (player_x, player_y, 20, 20))
    # проверка на проигрыш
    if player_y < 350:
        player_y += 3
    if tube_x > -50:
        tube_x -= 2
    if tube_x < 50:
        # если проиграли:
        if player_y >= tube_y and player_y < tube_y + 50:
            score += 1
        else:
            check_record()
        tube_x = 490
        tube_y = random.randint(50, 300)
    # обработчик клавиш
    keys = pygame.key.get_pressed()
    if keys[pygame.K_SPACE] and player_y > 5:
        player_y -= 5
```
*Основной цикл*
```python
while run:
    game()
    for event in pygame.event.get():
        if event.type == pygame.QUIT:
            run = False
            pygame.quit()
    pygame.display.update()
    fpsclck.tick(50)
```
