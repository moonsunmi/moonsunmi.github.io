---
layout: post
title:  "연오의 파이썬 공부 16"
date:   2022-08-08 22:23:28 +0900
categories: yeono-python
---






## 12장


이벤트란 의미 있는 사건을 정의해 둔 것이다. 이벤트를 이용하면 특정한 사건이 일어났을 때 그에 맞는 코드를 실행하도록 준비해 둘 수 있다.




코드 12-3
```python
import pygame
import time

SCREEN_WIDTH = 400
SCREEN_HEIGHT = 80

RED = 255, 0, 0
GREEN = 0, 255, 0
BLUE = 0, 0, 255
PURPLE = 127, 0, 127
GRAY = 127, 127, 127
WHITE = 255, 255, 255


pygame.init()

screen = pygame.display.set_mode((SCREEN_WIDTH, SCREEN_HEIGHT))

rect = pygame.Rect((0, 0), (SCREEN_WIDTH, SCREEN_HEIGHT))
pygame.draw.rect(screen, WHITE, rect)

rect = pygame.Rect((0, 0), (40, 40))
pygame.draw.rect(screen, GREEN, rect)

rect = pygame.Rect((360, 60), (60, 20))
pygame.draw.rect(screen, RED, rect)

pygame.display.update()

time.sleep(3)
```



코드 12-9
```python
import pygame

SCREEN_WIDTH = 400
SCREEN_HEIGHT = 400
BLOCK_SIZE = 20

RED = 255, 0, 0
GREEN = 0, 255, 0
BLUE = 0, 0, 255
PURPLE = 127, 0, 127
GRAY = 127, 127, 127
WHITE = 255, 255, 255


def draw_background(screen):
   background = pygame.Rect((0, 0), (SCREEN_WIDTH, SCREEN_HEIGHT))
   pygame.draw.rect(screen, WHITE, background)


def draw_block(screen, color, position):

   block = pygame.Rect((position[1] * BLOCK_SIZE, position[0] * BLOCK_SIZE), (BLOCK_SIZE, BLOCK_SIZE))
   pygame.draw.rect(screen, color, block)


pygame.init()
screen = pygame.display.set_mode((SCREEN_WIDTH, SCREEN_HEIGHT))

block_position = [0, 0]

while True:
   events = pygame.event.get()
   for event in events:
       if event.type == pygame.QUIT:
           exit()
       if event.type == pygame.KEYDOWN:
           if event.type == pygame.K_RIGHT:
               block_position[1] += 1
           elif event.type == pygame.K_LEFT:
               block_position[1] -= 1
           elif event.type == pygame.K_UP:
               block_position[0] -= 1
           elif event.type == pygame.K_DOWN:
               block_position[0] += 1

   draw_background(screen)
   draw_block(screen, GREEN, block_position)
   pygame.display.update()
```




코드 12-11
```python
import pygame
import time
from datetime import datetime
from datetime import timedelta

SCREEN_WIDTH = 400
SCREEN_HEIGHT = 400
BLOCK_SIZE = 20

RED = 255, 0, 0
GREEN = 0, 255, 0
BLUE = 0, 0, 255
PURPLE = 127, 0, 127
GRAY = 127, 127, 127
WHITE = 255, 255, 255

DIRECTION_ON_KEY = {
   pygame.K_UP: 'north',
   pygame.K_DOWN: 'south',
   pygame.K_LEFT: 'west',
   pygame.K_RIGHT: 'east'
}


def draw_background(screen):
   background = pygame.Rect((0, 0), (SCREEN_WIDTH, SCREEN_HEIGHT))
   pygame.draw.rect(screen, WHITE, background)


def draw_block(screen, color, position):
   block = pygame.Rect((position[1] * BLOCK_SIZE, position[0] * BLOCK_SIZE), (BLOCK_SIZE, BLOCK_SIZE))
   pygame.draw.rect(screen, color, block)


pygame.init()
screen = pygame.display.set_mode((SCREEN_WIDTH, SCREEN_HEIGHT))

block_direction = 'east'
block_position = [0, 0]
last_moved_time = datetime.now()

while True:
   events = pygame.event.get()
   for event in events:
       if event.type == pygame.QUIT:
           exit()
       if event.type == pygame.KEYDOWN:
           if event.key in DIRECTION_ON_KEY:
               block_direction = DIRECTION_ON_KEY[event.key]

           if event.key == pygame.K_UP:  # 입력된 키가 위쪽 화살표 키인 경우
               block_position[0] -= 1  # 블록의 y 좌표에서 1을 뺀다.
           elif event.key == pygame.K_DOWN:  # 입력된 키가 아래쪽 화살표 키인 경우
               block_position[0] += 1  # 블록의 y 좌표에서 1을 더한다.
           elif event.key == pygame.K_LEFT:  # 입력된 키가 왼쪽 화살표 키인 경우
               block_position[1] -= 1  # 블록의 x 좌표에서 1을 뺀다.
           elif event.key == pygame.K_RIGHT:  # 입력된 키가 왼쪽 화살표 키인 경우
               block_position[1] += 1  # 블록의 x 좌표에서 1을 더한다.

   if timedelta(seconds=1) <= datetime.now() - last_moved_time:
       if block_direction == 'north':
           block_position[0] -= 1
       elif block_direction == 'south':
           block_position[0] += 1
       elif block_direction == 'west':
           block_position[1] -= 1
       elif block_direction == 'east':
           block_position[1] += 1

       last_moved_time = datetime.now()

   draw_background(screen)
   draw_block(screen, GREEN, block_position)
   pygame.display.update()
```



