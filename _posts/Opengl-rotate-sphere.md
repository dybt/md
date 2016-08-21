title: opengl实现星球旋转
date: 2016-04-03 10:14:35
tags: OpenGL
---

#### 功能要求
+ 使用不同尺寸的线框球体（Wire Sphere）表示大小两个星球；
+ 使用平移和旋转操作实现小星球自转和绕大星球旋转的功能，键盘事件响应如下：
    - d 和 shift+d:分别控制小星球正反两个方向的自转；
    - y 和 shift+y:分别控制小星球绕大星球的正方向和反方向旋转；

<!-- more -->
#### 实现
+ 代码
```
#include <GL/glut.h>
#include <math.h>
#include <stdio.h>

#define PI 3.14159265358979

GLdouble distance = 0.6f;

GLdouble smallX = distance;
GLdouble smallY = 0.0f;
GLdouble smallZ = 0.0f;

GLdouble gzAngle = 0; //旋转的角度
GLsizei zzAngle = 0; //自转

void drawSphere(GLdouble radius, GLdouble x, GLdouble y, GLdouble z, bool isAngle) {
	glMatrixMode(GL_MODELVIEW);
	glPushMatrix();
	glTranslated(x, y, z);
	if (isAngle == true) {
		glRotated(zzAngle, 0.0, 0.0, 1.0);
	}
	glutWireSphere(radius, 20, 10);
	glPopMatrix();
}

void display(void) {

	glClear(GL_COLOR_BUFFER_BIT);
	glColor3f(1.0, 1.0, 1.0);

	// draw big sphere
	drawSphere(0.3,0,0,0,false);

	//draw small sphere, 通过redisplay调用可以刷新位置
	drawSphere(0.1, smallX, smallY, smallZ, true);

	glFlush();
	//glutSwapBuffers();
}

void keyboard(unsigned char key, int x, int y) {
	float angle;
	switch (key) {
		//自转
	case 'd':
		zzAngle = (zzAngle + 10) % 360;
		break;
	case 'D':
		zzAngle = (zzAngle - 10) % 360;
		break;
	case 'y':
		gzAngle = gzAngle + PI / 90;
		angle= cos(gzAngle);
		smallX = (GLdouble)distance * cos(gzAngle);
		smallY = (GLdouble)distance * sin(gzAngle);
		break;
	case 'Y':
		gzAngle = gzAngle - PI / 90;
		smallX = (GLdouble)distance * cos(gzAngle);
		smallY = (GLdouble)distance * sin(gzAngle);
		break;
	}
	glutPostRedisplay();
}

void init() {
	glClearColor(0.0, 0.0, 0.0, 0.0);
	glShadeModel(GL_FLAT);
}

void reshape(int w, int h)
{
	glViewport(0, 0, (GLsizei)w, (GLsizei)h);
	glMatrixMode(GL_PROJECTION);
	glLoadIdentity();
	gluPerspective(60.0, (GLfloat)w / (GLfloat)h, 0, 20.0);
	glMatrixMode(GL_MODELVIEW);
	glLoadIdentity();
	gluLookAt(1.5, 0.0, 0, 0.0, 0.0, 0.0, 0.0, 1.0, distance + 7);
}

int main(int argc, char *argv[]) {

	glutInit(&argc, argv);
	glutInitDisplayMode(GLUT_RGB | GLUT_SINGLE);
	glutInitWindowPosition(100, 100);
	glutInitWindowSize(600, 600);
	glutCreateWindow("WireSphere");

	init();
	glutDisplayFunc(display);
	glutKeyboardFunc(keyboard);
	glutReshapeFunc(reshape);
	glutMainLoop();

	return 0;

}
```
+ 实现说明：
    - 主要是利用glutPostRedisplay来达到动态的效果，通过变量的变换来设定旋转的角度，无论是自转还是公转；然后在通过对应的按键时去修改角度的值，并通过glutPostRedisplay()重新绘制出实体对应的坐标信息

#### 参考链接
+ http://blog.csdn.net/rosary226/article/details/5678414
