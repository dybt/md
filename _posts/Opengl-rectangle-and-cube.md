title: opengl绘制正方形和实现三维对象显示效果
date: 2016-04-03 09:57:18
tags: OpenGL
---

#### 实现正方形
+ 代码
```
#include <GL/glut.h>
void myDisplay(void) {
	glClear(GL_COLOR_BUFFER_BIT);
	glRectf(-0.5f, -0.5f, 0.5f, 0.5f);
	glFlush();
}

int main(int argc, char *argv[]){
	glutInit(&argc, argv);
	glutInitDisplayMode(GLUT_RGB | GLUT_SINGLE);
	glutInitWindowPosition(100, 100);
	glutInitWindowSize(400, 400);
	glutCreateWindow("Rectangle");

	glutDisplayFunc(&myDisplay);
	glutMainLoop();
	return 0;
}
```

<!-- more -->

#### 实现三维显示效果
+ 代码
```
#include <GL/glut.h>
#include <GL/gl.h>

/* 定义场景的观察点， 瞄准的参考点，以及朝上方向的坐标 */
GLfloat eyeX = 0.0f, eyeY = 0.0f, eyeZ = 5.0f;
GLfloat refX = 0.0f, refY = 0.0f, refZ = -2.0f;
GLfloat upX = 0.0f, upY = 2.0f, upZ = 0.0f;

void drawCube(void) {
	glClear(GL_COLOR_BUFFER_BIT);
	glColor3f(0.0f, 1.0f, 0.0f);
	glutSolidCube(2.0);                                   /* 画2个单元长的实心立方体 */
	glColor3f(0.0, 0.0, 0.0); 
	glLineWidth(2.0);                                     /* 设置边框宽度 */
	glutWireCube(2.0);                                    /* 画立方体边框 */
	glFlush();
}

void display(void) {
	drawCube();
}

void init(void) {
	glClearColor(1.0, 1.0, 1.0, 0.0);
	glClear(GL_COLOR_BUFFER_BIT | GL_DEPTH_BUFFER_BIT);

	/* 模型转换 */
	glMatrixMode(GL_MODELVIEW);
	gluLookAt(eyeX, eyeY, eyeZ, refX, refY, refZ, upX, upY, upZ);

	/* 旋转 */
	float ratX = 20.0;
	float ratY = 20.0;
	float ratZ = 45.0;
	
	glRotatef(ratZ, 0.0f, 0.0f, 1.0f);
	glRotatef(ratY, 0.0f, 1.0f, 0.0f);
	glRotatef(ratX, 1.0f, 0.0f, 0.0f);

	glMatrixMode(GL_PROJECTION);
	glLoadIdentity();
	glFrustum(-1.0f, 1.0f, -1.0f, 1.0f, 2.0f, 20.0f);
}
int main(int argc, char *argv[]) {
	glutInit(&argc, argv);                                /* initialize GLUT */
	glutInitDisplayMode(GLUT_RGBA | GLUT_SINGLE | GLUT_DEPTH);
	glutCreateWindow("Hello projected Cube!");            /* create window with title */
	init();
	glutDisplayFunc(display);
	glutMainLoop();                                       /* Event Handler Loops */
	return 0;
}
```

+ 实现说明
    - 主要是通过glutSolidCube画出实体，然后通过glutSolidCube画出边框来实现；然后通过调节照相机来达到效果

#### 参考链接
+ https://www.opengl.org/resources/libraries/glut/spec3/spec3.html
+ https://www.opengl.org/sdk/docs/man4/