title: Opengl绘制交互式的三次Bezier曲线
date: 2016-05-06 15:12:59
tags: OpengGL
---

### 功能要求
+ 使用鼠标在程序窗口内任意选取 4 个控制顶点；
+ 根据选定的控制顶点，绘制出对应的三次 Bezier 曲线，以及其控制多边形；
<!-- more -->

### 实现代码
```
#include <gl/glut.h>
#include <iostream>
using namespace std;

//用于记录下四个顶点
int vertexX[4], vertexY[4];
//用于跟踪已经写下的顶点
int num = 0;
//是否已经完成曲线的绘制
bool finished = false;

//n 表示对t=[0,1]进行的切分次数
void bezier(int n) {
	float b0, b1, b2, b3;
	float interval = 1.0 / n; //要用1.0，进行类型转换
	float t;

	glBegin(GL_LINE_STRIP);
	for (int i = 0; i < n; i++) {
		t = i * interval;

		b0 = (1 - t) * (1 - t) * (1 - t);
		b1 = 3 * t * (1 - t) * (1 - t);
		b2 = 3 * t * t * (1 - t);
		b3 = t * t * t;

		cout << b3 << endl;
		glVertex2f(b0 * vertexX[0] + b1 * vertexX[0] + b2 * vertexX[2] + b3 * vertexX[3],
			b0 * vertexY[0] + b1 * vertexY[1] + b2 * vertexY[2] + b3 * vertexY[3]);

	}
	glEnd();
}

void init() {
	glClearColor(0.0, 0.0, 0.0, 0.0);
	glShadeModel(GL_FLAT);
}

void display() {
	glClear(GL_COLOR_BUFFER_BIT);

	glLineWidth(1.5);
	glColor3f(1.0, 1.0, 1.0);
	glBegin(GL_LINE_STRIP);
	cout << num << endl;
	for (int i = 0; i < num; i++) {
		glVertex3f(vertexX[i], vertexY[i], 0.0);
	}
	glEnd();

	glColor3f(1.0, 1.0, 1.0);
	if (num == 4) {
		bezier(30);
	}

	glPointSize(5.0f);
	glBegin(GL_POINTS);
	for (int i = 0; i < 4; i++) {
		glVertex2f(vertexX[i], vertexY[i]);
	}
	glEnd();

	glFlush();
	glutSwapBuffers();
}

void reshape(int w, int h) {
	glViewport(0, 0, (GLsizei)w, (GLsizei)h);
	glMatrixMode(GL_PROJECTION);
	glLoadIdentity();
	gluOrtho2D(0.0, (GLsizei)w, (GLsizei)h, 0.0);
	glMatrixMode(GL_MODELVIEW);
	glLoadIdentity();
}

void mouse(int button, int state, int x, int y) {
	if (!finished) {
		if (button == GLUT_LEFT_BUTTON && state == GLUT_DOWN) {
			vertexX[num] = x;
			vertexY[num] = y;
			++num;

			if (num == 4) {
				finished = true;
			}

			glutPostRedisplay();
		}
	}
}

int main(int argc, char** argv) {
	glutInit(&argc, argv);
	glutInitDisplayMode(GLUT_DOUBLE | GLUT_RGB);
	glutInitWindowPosition(200, 200);
	glutInitWindowSize(500, 500);
	glutCreateWindow("Bezier show time");
	init();

	glutDisplayFunc(display);
	glutReshapeFunc(reshape);
	glutMouseFunc(mouse);

	glutMainLoop();
	return 0;
}
```

### 参考链接
+ http://www.cnblogs.com/caster99/p/4743637.html

