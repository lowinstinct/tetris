# Oluwatimileyin Soyege
# Mrs Sukovieff
# Computer Science 30
# 2 December 2021- Created Project
# 16 December 2021 - Officially Announced the Title of Project
# 6 January 2022 - Imported Pygame
# 7 January 2022 - Created a class of blocks and defined our function
# 7 January 2022 - Drew size of blocks
# 11 January 2022 - organized the instructions of moving up and down
# 12 January 2022 - Created an AI file
# 14 January 2022 - Created the blocks to move



# first we import the pygame window to get the game to work
import pygame, random

# This will set up the screen and window for our Tetris game
screen = pygame.display.set_mode((250,500))
pygame.display.set_caption("Tetris")
done = False
quickdrop = False
locked = False
fallingblocks = []
setblocks = []
clock = pygame.time.Clock()
fallcooldown = 0
movementcooldown = 0
rotatecooldown = 0
brickcenter = [0,0]


#We then make a class of blocks
class Block:
  
  def __init__(self, x, y, color):
    self.x = x + 12
    self.y = y + 12
    self.color = color
    fallingblocks.append(self)
  
  # This must define the fall function
  def fall(self):
    self.y += 25
  # This must define the move function
  def move(self):
    if pressed[pygame.K_a] or pressed[pygame.K_LEFT]: 
      self.x -= 25
      brickcenter[0] -= 6.25
    if pressed[pygame.K_d] or pressed[pygame.K_RIGHT]: 
      self.x += 25
      brickcenter[0] += 6.25
  
  # Then, we must draw blocks to organize it more properly and for it to look good
  def drawblock(self): #just to make it look nice
    pygame.draw.rect(screen, self.color[0], pygame.Rect(self.x-12,self.y-12,25,25))
    pygame.draw.polygon(screen, self.color[1], ((self.x-12,self.y-12),(self.x-9,self.y-9),(self.x+9,self.y-9),(self.x+12,self.y-12)))
    pygame.draw.polygon(screen, self.color[2], ((self.x-12,self.y-12),(self.x-9,self.y-9),(self.x-9,self.y+9),(self.x-12,self.y+12)))
    pygame.draw.polygon(screen, self.color[3], ((self.x-12,self.y+12),(self.x-9,self.y+9),(self.x+9,self.y+9),(self.x+12,self.y+12)))
    pygame.draw.polygon(screen, self.color[4], ((self.x+12,self.y+12),(self.x+9,self.y+9),(self.x+9,self.y-9),(self.x+12,self.y-12)))

# We then create a spawn, which generates the blocks and colors in any size or shape you want
def spawn():
  blocknum = random.randint(0,6)
  global brickcenter
  
  if blocknum == 0:
    # I block
    colors = [(129,184,231),
    (179,223,250),
    (146,202,238),
    (76,126,189),
    (96,157,213)]
    
    Block(100,0,colors)
    Block(100,25,colors)
    Block(100,50,colors)
    Block(100,75,colors)
    brickcenter = [125,50]
  elif blocknum == 1:
    #J block
    colors = [(77,110,177),
    (149,178,229),
    (104,145,203),
    (49,63,136),
    (63,85,158)]
    
    Block(125,0,colors)
    Block(125,25,colors)
    Block(125,50,colors)
    Block(100,50,colors)
    brickcenter = [137,37]
  elif blocknum == 2:
    # L block
    colors = [(219,127,44),
    (243,191,122),
    (229,158,69),
    (166,71,43),
    (193,98,44)]
    
    Block(100,0,colors)
    Block(100,25,colors)
    Block(100,50,colors)
    Block(125,50,colors) 
    brickcenter = [112,37] 
  elif blocknum == 3:
    # O block
    colors = [(248,222,49),
    (246,243,139),
    (245,235,86),
    (183,160,54),
    (213,190,55)]
    
    Block(100,0,colors)
    Block(100,25,colors)
    Block(125,0,colors)
    Block(125,25,colors)
    brickcenter = [125,25]
  elif blocknum == 4:
    #S block
    colors = [(156,195,76),
    (204,218,127),
    (174,208,79),
    (109,157,75),
    (140,183,93)]

    Block(100,0,colors)
    Block(100,25,colors)
    Block(125,0,colors)
    Block(75,25,colors)  
    brickcenter = [112,37]  
  elif blocknum == 5:
    #Z block
    colors = [(204,42,40),
    (226,138,132),
    (213,90,69),
    (151,34,42),
    (181,37,43)]
    
    Block(100,0,colors)
    Block(150,25,colors)
    Block(125,0,colors)
    Block(125,25,colors)
    brickcenter = [137,37] 
  else:
    #T block
    colors = [(147,68,149),
    (187,145,194),
    (156,101,167),
    (108,45,123),
    (128,47,135)]
    
    Block(100,0,colors)
    Block(100,25,colors)
    Block(125,0,colors)
    Block(75,0,colors)
    brickcenter = [112,12] 

def rotate():
  for fallingblock in fallingblocks:
    centerx = fallingblock.x - brickcenter[0]
    centery = fallingblock.y - brickcenter[1]
    centerx, centery = -centery, centerx
    fallingblock.x = brickcenter[0] + centerx
    fallingblock.y = brickcenter[1] + centery

spawn()

while not done: #main loop

  screen.fill((0,0,32))
  pressed = pygame.key.get_pressed()
  
  for gridy in range(25, 475):
    pygame.draw.line(screen, (255,255,255), (0,gridy), (250, gridy)) #horizontal grid lines
  
  for gridx in range(25, 225):
    pygame.draw.line(screen, (255,255,255), (gridx,0), (gridx, 500)) #vertical grid lines
  
  for fallingblock in fallingblocks:
    fallingblock.drawblock()

  if movementcooldown >= 5: 
    for fallingblock in fallingblocks: fallingblock.move()
    movementcooldown = 0

  for setblock in setblocks:
    setblock.drawblock()
    if setblock.y < 25: done = True 
 
  if fallcooldown >= 50: #makes all blocks fall at once
    for fallingblock in fallingblocks:
      fallingblock.fall()
      brickcenter[1] += 6.25
    fallcooldown = 0
  
  # With this control, you can rotate the piece and you can flip it
  if pressed[pygame.K_UP] and rotatecooldown >= 10: 
    rotate()
    rotatecooldown = 0
  
  # if you want the piece to go the ground instantly
  if pressed[pygame.K_SPACE]: quickdrop = True 
 
  # This initiates the movements of when you fall
  if quickdrop: fallcooldown = 50 
  
  # The more you press down, the faster it goes
  elif pressed[pygame.K_DOWN]: fallcooldown += 8 
  
  else: fallcooldown += 1 #default speed

#if fallingblock collides with setblock
  for fallingblock in fallingblocks:
    for setblock in setblocks:
      if pygame.Rect(fallingblock.x - 11, fallingblock.y - 12, 23, 26).colliderect(pygame.Rect(setblock.x - 11, setblock.y - 12, 23, 26)): 
        locked = True
         #if block hits the bottom
    if fallingblock.y >= 487 and not locked: 
      locked = True

# if block is in final state
  if locked: 
    for i in range(0,4): setblocks.append(Block(fallingblocks[i].x-12,fallingblocks[i].y-12,fallingblocks[i].color))
    fallingblocks = []
    locked = False
    quickdrop = False
    spawn()

  movementcooldown += 1
  rotatecooldown += 1
  clock.tick(50)
  pygame.display.flip()
  
  if pressed[pygame.K_ESCAPE]: done = True

# This is the end of the game
  for event in pygame.event.get():
	  if event.type == pygame.QUIT:
		  done = True

# Thanks for tuning in, hopefully you enjoyed the game!
