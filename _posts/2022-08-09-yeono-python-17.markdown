---
layout: post
title: "연오의 파이썬 공부 17"
date: 2022-08-09 22:22:12 +0900
tags: yeono-python
---

뱀 게임 완성

snake.py

```python
import pygame
import random
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

TURN_INTERVAL = timedelta(seconds=0.3)


class SnakeCollisionException(Exception):
   """뱀 충돌 예외"""
   pass


class Snake:
   """뱀 클래스"""
   color = GREEN

   def __init__(self):
       self.positions = [(9, 6), (9, 7), (9, 8), (9, 9)]
       self.direction = 'north'

   def draw(self, screen):
       for position in self.positions:
           draw_block(screen, self.color, position)

   def crawl(self):
       """뱀이 현재 방향으로 한 칸 기어간다."""
       head_position = self.positions[0]
       y, x = head_position
       if self.direction == 'north':
           self.positions = [(y - 1, x)] + self.positions[:-1]
       elif self.direction == 'south':
           self.positions = [(y + 1, x)] + self.positions[:-1]
       elif self.direction == 'west':
           self.positions = [(y, x - 1)] + self.positions[:-1]
       elif self.direction == 'east':
           self.positions = [(y, x + 1)] + self.positions[:-1]

   def turn(self, direction):
       self.direction = direction

   def grow(self):
       head_position = self.positions[0]
       y, x = head_position
       if self.direction == 'north':
           self.positions.append((y - 1, x))
       elif self.direction == 'south':
           self.positions.append((y + 1, x))
       elif self.direction == 'west':
           self.positions.append((y, x - 1))
       elif self.direction == 'east':
           self.positions.append((y, x + 1))


class Apple:
   """사과 클래스"""
   color = RED

   def __init__(self, position=(5, 5)):
       self.position = position

   def draw(self, screen):
       draw_block(screen, self.color, self.position)


class GameBoard:
   """게임판 클래스"""
   width = 20
   height = 20

   def __init__(self):
       self.snake = Snake()
       self.apple = Apple()

   def draw(self, screen):
       self.apple.draw(screen)
       self.snake.draw(screen)

   def process_turn(self):
       """게임을 한 차례 진행한다."""
       self.snake.crawl()

       if self.snake.positions[0] in self.snake.positions[1:]:
           raise SnakeCollisionException

       if self.snake.positions[0] == self.apple.position:
           self.snake.grow()
           self.put_new_apple()

   def put_new_apple(self):
       self.apple = Apple((random.randint(0, 19), random.randint(0, 19)))
       for position in self.snake.positions:
           print('www')
           if self.apple.position == position:
               self.put_new_apple()
               break


def draw_background(screen):
   background = pygame.Rect((0, 0), (SCREEN_WIDTH, SCREEN_HEIGHT))
   pygame.draw.rect(screen, WHITE, background)


def draw_block(screen, color, position):
   block = pygame.Rect((position[1] * BLOCK_SIZE, position[0] * BLOCK_SIZE), (BLOCK_SIZE, BLOCK_SIZE))
   pygame.draw.rect(screen, color, block)


pygame.init()
screen = pygame.display.set_mode((SCREEN_WIDTH, SCREEN_HEIGHT))

game_board = GameBoard()
block_direction = 'east'
block_position = [0, 0]
last_turn_time = datetime.now()

while True:
   events = pygame.event.get()
   for event in events:
       if event.type == pygame.QUIT:
           exit()

       if event.type == pygame.KEYDOWN:
           if event.key in DIRECTION_ON_KEY:
               game_board.snake.turn(DIRECTION_ON_KEY[event.key])

   if TURN_INTERVAL < datetime.now() - last_turn_time:
       try:
           game_board.process_turn()
       except SnakeCollisionException:
           exit()
       last_turn_time = datetime.now()

   draw_background(screen)
   game_board.draw(screen)
   pygame.display.update()

```
