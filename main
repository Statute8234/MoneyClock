import pygame
import random
import string
import math
import sys
import time
pygame.init()

# pygame screen info
screen = pygame.display.set_mode((1200, 600))
screen_info = pygame.display.Info()
screen_width = screen_info.current_w
screen_height = screen_info.current_h

background_colour = (255,255,255)
screen.fill(background_colour)
pygame.display.flip()

# colors
BLACK = (0, 0, 0)
RED = (255, 0, 0)
GREEN = (0, 255, 0)
GRAY = (128, 128, 128)
LIGHT_GRAY = (96, 96, 96)
YELLOW = (255, 255, 0)
WHITE = (255, 255, 255)
# price line
goBack = False
R = 222
theta = 0
value = theta * 950 / (260 * 12)

class ScrollableRectangle:
    def __init__(self,x,y,rectangle_size = (0,0), main_text="", info_text=""):
        global screen_height
        self.x = x
        self.y = y
        self.scroll_position = 0
        self.rectangle_width = rectangle_size[0]
        self.rectangle_height = rectangle_size[1]
        self.scroll_height = screen_height
        self.main_text = main_text
        self.info_text = info_text
        self.font_main = pygame.font.Font(None, 24)
        self.font_info = pygame.font.Font(None, 18)
        self.creation_time = time.strftime("%H:%M:%S")
        self.rect_color = WHITE

    def draw(self,screen):
        pygame.draw.rect(screen, LIGHT_GRAY, (self.x, self.y, 50, self.scroll_height))  # show scroll area
        # Choose rectangle color based on index
        if self.y % 100 == 0:  # Every other rectangle
            self.rect_color = YELLOW  # Yellow
        else:
            self.rect_color = WHITE  # White

        pygame.draw.rect(screen, self.rect_color, (self.x + 50, self.y - self.scroll_position, self.rectangle_width, self.rectangle_height))

        main_text_surface = self.font_main.render("Name: " + self.main_text, True, BLACK)
        main_text_rect = main_text_surface.get_rect(topleft=(self.x + 60, self.y - self.scroll_position))
        screen.blit(main_text_surface, main_text_rect)
        
        info_text_surface = self.font_info.render("sold for $" + self.info_text, True, BLACK)
        info_text_rect = info_text_surface.get_rect(topleft=(self.x + 60, self.y - self.scroll_position + 24))
        screen.blit(info_text_surface, info_text_rect)

        time_text_surface = self.font_main.render(self.creation_time, True, BLACK)
        time_text_rect = time_text_surface.get_rect(topleft=(self.x + 460, self.y - self.scroll_position))
        screen.blit(time_text_surface, time_text_rect)

    def scroll(self,delta):
        self.scroll_position += delta

# print text around the clock
def Print_Lage_Text(text, position):
    font = pygame.font.SysFont("Times", 20, True, False)
    surface = font.render(text, True, BLACK)
    screen.blit(surface, position)

# convert the number into Degrees around the clock
def Convert_Degrees(R, theta):
    angle_offset = -90
    adjusted_theta = theta + angle_offset
    y = math.cos(2 * math.pi * theta / 360) * R
    x = math.sin(2 * math.pi * theta / 360) * R
    return x + 300, -(y - 300) - 10

def evenly_place_numbers_on_circle(center_x, center_y, radius, num_divisions):
    positions = []
    for i in range(num_divisions):
        angle = 360 * (i / num_divisions) - 90
        x = center_x + radius * math.cos(math.radians(angle))
        y = center_y + radius * math.sin(math.radians(angle))
        positions.append((x, y))
    return positions

# prints the total cost of the bet
def Print_Cost(text, position):
    font = pygame.font.SysFont("Times", 22, True, False)
    surface = font.render(text, True, BLACK)
    screen.blit(surface, position)

def generate_random_combination(length):
    alphabet = string.ascii_lowercase
    random_combination = ''.join(random.choice(alphabet.upper()) for _ in range(length))
    return random_combination

random_letters = generate_random_combination(4)

rectangle = []
int_y = 0
def MakeRec():
    global int_y,rectangle 
    rectangle.append(ScrollableRectangle(650,int_y,rectangle_size = (490,50),main_text = random_letters, info_text = str(value)))
    int_y += 50

class Button:
    def __init__(self, x, y, width, height, text, active_color, inactive_color):
        self.x = x
        self.y = y
        self.width = width
        self.height = height
        self.text = text
        self.active_color = active_color
        self.inactive_color = inactive_color
        self.color = self.inactive_color
        self.font = pygame.font.Font(None, 36)
        self.clicked = False

    def draw(self, screen):
        pygame.draw.rect(screen, self.color, (self.x, self.y, self.width, self.height))
        text_surface = self.font.render(self.text, True, BLACK)
        text_rect = text_surface.get_rect(center=(self.x + self.width // 2, self.y + self.height // 2))
        screen.blit(text_surface, text_rect)

    def handle_event(self, event):
        global random_letters, goBack
        if event.type == pygame.MOUSEBUTTONDOWN:
            mouse_pos = pygame.mouse.get_pos()
            if self.x < mouse_pos[0] < self.x + self.width and self.y < mouse_pos[1] < self.y + self.height:
                self.clicked = True
                self.color = self.inactive_color
                MakeRec()
                random_letters = generate_random_combination(3)
                goBack = True

    def reset(self):
        self.clicked = False
        self.color = self.active_color

button = Button(300, 350, 100, 50, "Buy", GRAY, GREEN)


running = True
while running:
    for event in pygame.event.get():
        # Check for QUIT event
        if event.type == pygame.QUIT:
            running = False
        elif event.type == pygame.MOUSEBUTTONDOWN:
            if event.button == 4:  # Scroll up
                for rect in rectangle:
                    rect.scroll(-10)
            if event.button == 5:  # Scroll down
                for rect in rectangle:
                    rect.scroll(10)
            button.handle_event(event)
            if button.clicked:
                goBack = True
    
    #print(mouse)
    screen.fill(background_colour)
    pygame.draw.circle(screen,BLACK,(325,300), 250, 2)
    for rect in rectangle:
        rect.draw(screen)

    number_positions = evenly_place_numbers_on_circle(300, 290, 270, 12)
    for i in range(0, 12):  # Changed the range to start from 0
        angle = i * (360 / 12)  # Calculate angle in degrees
        number = 1.00 + i * ((99.00 - 1.00) / 11)
        Print_Lage_Text("${:.2f}".format(number), Convert_Degrees(277, angle))

    # price line
    if goBack == False:
        if random.randint(0,360) == 0:
            MakeRec()
            random_letters = generate_random_combination(3)
            goBack = True
        else:
            theta += 1
    else:
        if theta > 0:
            if goBack:
                theta -= 2
        else:
            goBack = False

    # Update the button color based on goBack
    if goBack:
        button.color = GRAY
    else:
        button.color = GREEN

    pygame.draw.line(screen, RED, (325, 300), Convert_Degrees(R, theta), 4)
    # display value
    smallfont = pygame.font.SysFont('Times', 18)
    text_name = smallfont.render("Name: " + random_letters, True, (0,0,0))
    value = round(abs(theta * 950 / (260 * 12)),2)
    text = smallfont.render("${:.2f}".format(value), True, (0,0,0))
    screen.blit(text_name, (300, 300))
    screen.blit(text, (300, 320))
    # button
    button.draw(screen)
    if button.clicked:
        button.reset()

    pygame.display.update()
pygame.quit()
sys.exit()
