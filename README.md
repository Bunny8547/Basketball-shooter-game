# Hoop Dreams: Precision Shooter
Creating a simple basketball shooting game called "Hoop Dreams: Precision Shooter" in Python involves using a library that supports graphics and user input. The pygame library is a great choice for this kind of game. Here's a basic example of how you might start building this game:

1: Install pygame: If you haven't installed it yet, you can do so via pip:

pip install pygame

Basic Game Setup:

Initialize pygame and set up the game window.
Load images for the basketball, hoop, and background.
Handle user input to control the shooting mechanics.
Detect if the basketball goes through the hoop.

****************************************************************************************
import pygame
import random
import math

# Initialize Pygame
pygame.init()

# Set up display
WIDTH, HEIGHT = 800, 600
window = pygame.display.set_mode((WIDTH, HEIGHT))
pygame.display.set_caption("Hoop Dreams: Precision Shooter")

# Load images
background = pygame.image.load("background.png")
hoop = pygame.image.load("hoop.png")
basketball = pygame.image.load("basketball.png")

# Define colors
WHITE = (255, 255, 255)

# Basketball properties
basketball_pos = [WIDTH // 2, HEIGHT - 50]
basketball_velocity = [0, 0]
basketball_launched = False

# Hoop properties
hoop_pos = [random.randint(200, 600), 100]

# Game variables
gravity = 0.5
power = 10

# Function to reset the basketball
def reset_basketball():
    global basketball_pos, basketball_velocity, basketball_launched
    basketball_pos = [WIDTH // 2, HEIGHT - 50]
    basketball_velocity = [0, 0]
    basketball_launched = False

# Function to check if basketball is in the hoop
def check_hoop():
    distance = math.sqrt((basketball_pos[0] - hoop_pos[0])**2 + (basketball_pos[1] - hoop_pos[1])**2)
    return distance < 50  # Adjust this value based on your hoop size

# Main game loop
running = True
while running:
    for event in pygame.event.get():
        if event.type == pygame.QUIT:
            running = False
        elif event.type == pygame.MOUSEBUTTONDOWN and not basketball_launched:
            mouse_x, mouse_y = pygame.mouse.get_pos()
            angle = math.atan2(mouse_y - basketball_pos[1], mouse_x - basketball_pos[0])
            basketball_velocity = [power * math.cos(angle), power * math.sin(angle)]
            basketball_launched = True

    # Update basketball position
    if basketball_launched:
        basketball_velocity[1] += gravity
        basketball_pos[0] += basketball_velocity[0]
        basketball_pos[1] += basketball_velocity[1]

        # Check for out of bounds
        if basketball_pos[0] < 0 or basketball_pos[0] > WIDTH or basketball_pos[1] > HEIGHT:
            reset_basketball()
        elif check_hoop():
            print("Score!")
            reset_basketball()
            hoop_pos = [random.randint(200, 600), 100]  # Move the hoop

    # Draw everything
    window.fill(WHITE)
    window.blit(background, (0, 0))
    window.blit(hoop, (hoop_pos[0] - hoop.get_width() // 2, hoop_pos[1] - hoop.get_height() // 2))
    window.blit(basketball, (basketball_pos[0] - basketball.get_width() // 2, basketball_pos[1] - basketball.get_height() // 2))
    pygame.display.flip()

    # Cap the frame rate
    pygame.time.Clock().tick(60)

pygame.quit()

2: Game Loop:

Handles events like quitting the game or shooting the basketball.
Updates the basketball's position based on its velocity and gravity.
Resets the basketball if it goes out of bounds or scores a hoop.
Draws the background, hoop, and basketball.

3:Reset and Check Functions: Ensures the game resets the basketball and checks if it scores.

Make sure to replace "background.png", "hoop.png", and "basketball.png" with the paths to your own image files.
