from OpenGL.GL import *
from OpenGL.GLUT import *
from OpenGL.GLU import *
import sys

angle = 0.0
width, height = 600, 600

def hsv_to_rgb(h, s, v):
    h_i = int(h * 6)
    f = h * 6 - h_i
    p = v * (1 - s)
    q = v * (1 - f * s)
    t = v * (1 - (1 - f) * s)
    h_i = h_i % 6
    if h_i == 0: return v, t, p
    if h_i == 1: return q, v, p
    if h_i == 2: return p, v, t
    if h_i == 3: return p, q, v
    if h_i == 4: return t, p, v
    if h_i == 5: return v, p, q

def init():
    glClearColor(0, 0, 0, 1)
    glEnable(GL_DEPTH_TEST)

def reshape(w, h):
    global width, height
    width, height = w, h
    glViewport(0, 0, w, h)
    glMatrixMode(GL_PROJECTION)
    glLoadIdentity()
    gluPerspective(45, w / h if h != 0 else 1, 0.1, 50.0)
    glMatrixMode(GL_MODELVIEW)

def draw_cube():
    global angle
    glClear(GL_COLOR_BUFFER_BIT | GL_DEPTH_BUFFER_BIT)
    glLoadIdentity()
    glTranslatef(0, 0, -7)
    glRotatef(angle, 1, 1, 0)

    # Color cycles with rotation
    hue = (angle % 360) / 360.0
    r, g, b = hsv_to_rgb(hue, 1, 1)
    glColor3f(r, g, b)

    # Draw cube edges only
    glBegin(GL_LINES)
    # Front face
    glVertex3f(-1, -1,  1); glVertex3f( 1, -1,  1)
    glVertex3f( 1, -1,  1); glVertex3f( 1,  1,  1)
    glVertex3f( 1,  1,  1); glVertex3f(-1,  1,  1)
    glVertex3f(-1,  1,  1); glVertex3f(-1, -1,  1)

    # Back face
    glVertex3f(-1, -1, -1); glVertex3f( 1, -1, -1)
    glVertex3f( 1, -1, -1); glVertex3f( 1,  1, -1)
    glVertex3f( 1,  1, -1); glVertex3f(-1,  1, -1)
    glVertex3f(-1,  1, -1); glVertex3f(-1, -1, -1)

    # Connections between front and back
    glVertex3f(-1, -1,  1); glVertex3f(-1, -1, -1)
    glVertex3f( 1, -1,  1); glVertex3f( 1, -1, -1)
    glVertex3f( 1,  1,  1); glVertex3f( 1,  1, -1)
    glVertex3f(-1,  1,  1); glVertex3f(-1,  1, -1)
    glEnd()

    glutSwapBuffers()

def update(value):
    global angle
    angle += 2
    if angle > 360:
        angle -= 360
    glutPostRedisplay()
    glutTimerFunc(16, update, 0)

def main():
    glutInit(sys.argv)
    glutInitDisplayMode(GLUT_DOUBLE | GLUT_RGB | GLUT_DEPTH)
    glutInitWindowSize(width, height)
    glutCreateWindow(b"Rotating Colorful Wireframe Cube")
    glutDisplayFunc(draw_cube)
    glutReshapeFunc(reshape)
    glutTimerFunc(0, update, 0)
    init()
    glutMainLoop()

if __name__ == "__main__":
    main()
