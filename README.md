import pygame
from pygame.locals import *
from OpenGL.GL import *
from OpenGL.GLU import *
import math

# Initialize Pygame
pygame.init()

# Set display size
display = (800, 600)
pygame.display.set_mode(display, DOUBLEBUF | OPENGL)

# Set window caption with your full name
pygame.display.set_caption("03 Lab 1 - Ivan Britz")

# Set perspective and translate object
gluPerspective(45, (display[0] / display[1]), 0.1, 50.0)
glTranslatef(0, 0, -5)

# Define cube vertices
vertices = [
    (1, 1, 1),   # 0
    (1, 1, -1),  # 1
    (1, -1, -1), # 2
    (1, -1, 1),  # 3
    (-1, 1, 1),  # 4
    (-1, -1, -1),# 5
    (-1, -1, 1), # 6
    (-1, 1, -1)  # 7
]

# Define cube edges (pairs of vertices)
edges = [
    (0, 1), (1, 2), (2, 3), (3, 0),  # front square
    (4, 7), (7, 5), (5, 6), (6, 4),  # back square
    (3, 6), (0, 4), (2, 5), (1, 7)   # connections
]

# Function to draw cube
def draw_cube(color):
    glColor3f(*color)  # Apply current color
    glBegin(GL_LINES)
    for edge in edges:
        for vertex in edge:
            glVertex3fv(vertices[vertex])
    glEnd()

# Function to generate color cycle (using sine waves)
def get_color(t):
    r = (math.sin(t) + 1) / 2
    g = (math.sin(t + 2) + 1) / 2
    b = (math.sin(t + 4) + 1) / 2
    return (r, g, b)

# Main loop
t = 0
while True:
    for event in pygame.event.get():
        if event.type == pygame.QUIT:
            pygame.quit()
            quit()

    # Rotate cube
    glRotatef(1, 1, 1, 1)

    # Clear screen and depth buffer
    glClear(GL_COLOR_BUFFER_BIT | GL_DEPTH_BUFFER_BIT)

    # Get dynamic color
    color = get_color(t)
    draw_cube(color)

    # Update display
    pygame.display.flip()
    pygame.time.wait(15)

    # Increase time for color cycling
    t += 0.05
