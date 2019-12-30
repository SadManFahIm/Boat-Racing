#include <stdio.h>
#include <windows.h>
#include <math.h>
#include <stdlib.h>
#include<process.h>
#include <GL/glut.h>
#include <time.h>
#include <iostream>
using namespace std;

float win=0,lose=0,score=0;
int screenTime = 0;
int height = 100, width = 100 ;
int window_width = 620, window_height = 480;
int window_width2 = 300, window_height2 = 480;
int mx = window_width/4 - width/4, my = 10;
int b1x = rand() % window_width2 + 200, b1y = window_height;
int b2x = rand() % window_width + 1, b2y = window_height;
int c1x = rand() % window_width2 + 190, c1y = window_height;

float point = 0.0;

int windowFlag=0;

int boatX1, boatY1;
int bushX1, bushY1, bushX2, bushY2;
int coinX1, coinY1;

int b3x = rand() % window_width2 + 200, b3y = window_height;
int c3x = rand() % window_width2 + 190, c3y = window_height;
int bushX3, bushY3, bushX4, bushY4;
int coinX2, coinY2;

int b4x = rand() % window_width2 + 200, b4y = window_height;
int bushX5, bushY5, bushX6, bushY6;

void myInit (void)
{
    glClearColor(0.53, 0.81, 0.93, 0.0);
    glColor3f(0.0f, 0.0f, 0.0f);
    glPointSize(4.0);
    glMatrixMode(GL_PROJECTION);
    glLoadIdentity();
    gluOrtho2D(0.0, 640.0, 0.0, 480.0);
}

void reshape(int w,int h)
{
   glutReshapeWindow( 1348, 700);
}

void drawScoreValue (void * font, int s,int spc, float x, float y)
{
     glRasterPos2f(x, y);
	 glColor3f(255, 255, 255);
     int k=0,j=0;
     while(s>9)
        {
            k=s%10;
            glRasterPos2f((x-(j*spc)), y);
            glutBitmapCharacter (font, 48+k);
            j++;
            s=s/10;

        }
        glRasterPos2f((x-(j*spc)), y);
        glutBitmapCharacter (font, 48+s);
}

// ***********************************HOME PAGE CONTENTS **************************************
// ***********************************HOME PAGE CONTENTS **************************************
// ***********************************HOME PAGE CONTENTS **************************************

void startText (void * font, char *s, float x, float y){
     glRasterPos2f(x, y);
	 glColor3f(255, 0, 0);
     for (int i = 0; i < strlen (s); i++)
          glutBitmapCharacter (font, s[i]);
}
void aboutText (void * font, char *s, float x, float y){
     glRasterPos2f(x, y);
	 glColor3f(255, 0, 0);
     for (int i = 0; i < strlen (s); i++)
          glutBitmapCharacter (font, s[i]);
}

void exitText (void * font, char *s, float x, float y){
     glRasterPos2f(x, y);
	 glColor3f(255, 0, 0);
     for (int i = 0; i < strlen (s); i++)
          glutBitmapCharacter (font, s[i]);
}

void winText (void * font, char *s, float x, float y){
     glRasterPos2f(x, y);
	 glColor3f(255, 0, 0);
     for (int i = 0; i < strlen (s); i++)
          glutBitmapCharacter (font, s[i]);
}

void looseText (void * font, char *s, float x, float y){
     glRasterPos2f(x, y);
	 glColor3f(255, 0, 0);
     for (int i = 0; i < strlen (s); i++)
          glutBitmapCharacter (font, s[i]);
}

void homePage()
{
glClear (GL_COLOR_BUFFER_BIT);
    PlaySound("boatstart.wav", NULL,SND_ASYNC|SND_FILENAME|SND_LOOP);
    startText(GLUT_BITMAP_TIMES_ROMAN_24,"START GAME [S]",270,265);
    aboutText(GLUT_BITMAP_TIMES_ROMAN_24,"ABOUT US [u] ",270,230);
    exitText(GLUT_BITMAP_TIMES_ROMAN_24,"EXIT [x] ",270,195);
    glFlush();
}

//********************************************GAME PLAY CONTENTS**********************************************
//********************************************GAME PLAY CONTENTS**********************************************
//********************************************GAME PLAY CONTENTS**********************************************

void aboutUs()
{

}

void winWindow()
{
    glClear (GL_COLOR_BUFFER_BIT);
    PlaySound("gamewinsound.wav", NULL,SND_ASYNC|SND_FILENAME);
    winText(GLUT_BITMAP_TIMES_ROMAN_24,"CONGRATULATION !! YOU HAVE WON !!",240,265);
    winText(GLUT_BITMAP_TIMES_ROMAN_24,"Collide With COIN: ",240,225);
    winText(GLUT_BITMAP_TIMES_ROMAN_24,"Collide With Bush: ",240,195);
    winText(GLUT_BITMAP_TIMES_ROMAN_24,"Total Score:",240,165);
    drawScoreValue(GLUT_BITMAP_TIMES_ROMAN_24,win,6,338,224);
    drawScoreValue(GLUT_BITMAP_TIMES_ROMAN_24,lose,6,330,194);
    drawScoreValue(GLUT_BITMAP_TIMES_ROMAN_24,score,6,305,163);
    glFlush();
}

void looseWindow()
{
    glClear (GL_COLOR_BUFFER_BIT);
    PlaySound("gamelosesound.wav", NULL,SND_ASYNC|SND_FILENAME);
    looseText(GLUT_BITMAP_TIMES_ROMAN_24,"You Lost The Game",240,265);
    looseText(GLUT_BITMAP_TIMES_ROMAN_24,"Collide With COIN: ",240,225);
    looseText(GLUT_BITMAP_TIMES_ROMAN_24,"Collide With Bush: ",240,195);
    looseText(GLUT_BITMAP_TIMES_ROMAN_24,"Total Score:",240,165);
    drawScoreValue(GLUT_BITMAP_TIMES_ROMAN_24,win,6,338,224);
    drawScoreValue(GLUT_BITMAP_TIMES_ROMAN_24,lose,6,330,194);
    drawScoreValue(GLUT_BITMAP_TIMES_ROMAN_24,score,6,305,163);
    glFlush();
}


void countTime()
{
    screenTime = clock()/1000;

    score = win - lose;

    if( screenTime == 60 )
    {
        if(win > lose )
        {
            glutDisplayFunc(winWindow);
        }
        else
        {
            glutDisplayFunc(looseWindow);
        }
    }

    if( score <= 0)
    {
        score = 0;
    }
}

//////////////////////////////////////////////////////////////


void keyboardAction(int key, int x, int y)
{
    switch(key)
    {
        case GLUT_KEY_RIGHT:
             mx += 10;
            glutPostRedisplay();
            break;

        case GLUT_KEY_LEFT:
             mx -= 10;
            glutPostRedisplay();
            break;

        default:
            break;
    }
}

//////////////////////////////////////////////////////////////

void drawScore (void * font, char *s, float x, float y){
     glRasterPos2f(x, y);
	 glColor3f(0, 0, 0);
     for (int i = 0; i < strlen (s); i++)
          glutBitmapCharacter (font, s[i]);
}

void keyboard(unsigned char key, int x, int y)
{
    if(key == 'a' || key == 'A')
    {
        mx -= 10;
    }
    else if(key == 'd' || key == 'D')
    {
        mx += 10;
    }
    else if(key == 's' || key == 'S')
    {
        windowFlag = 1;
    }
    else if(key == 'u' || key == 'U')
    {
        glutDisplayFunc(aboutUs);
    }
    else if(key == 'x' || key == 'X')
    {
        exit(0);
    }

    glutPostRedisplay();
}



void translate()
{
    b1y -= 10;
    c1y -= 10;

    b3y -= 10;
    c3y -= 10;

    b4y -= 10;

    if(b1y <= -80 )
    {
       b1x = rand() % (window_width2-15) + 195 ;
       b1y = rand() % (window_height2*2 ) + window_height2;
    }

    if(c1y <= -80 )
    {
        c1x = rand() % (window_width2-15) + 195 ;
        c1y = rand() % (window_height2*2 ) + window_height2;
    }

    if(b3y <= -80 )
    {
       b3x = rand() % (window_width2-20) + 195 ;
       b3y = rand() % (window_height2*2 ) + window_height2;
    }

    if(c3y <= -80 )
    {
        c3x = rand() % (window_width2-20) + 195 ;
        c3y = rand() % (window_height2*2 ) + window_height2;
    }

    if(b4y <= -80 )
    {
       b4x = rand() % (window_width2-20) + 195 ;
       b4y = rand() % (window_height2*2 ) + window_height2;
    }

    // right side boundary
    if( mx > 370)
    {
        mx-=10;
    }

    // left side boundary
    if( mx < 60 )
    {
        mx+=10;
    }

    // collision detection

    if( boatY1>=coinY1 && boatX1+13>=coinX1 && boatX1-13<=coinX1 )
    {
        PlaySound("coincollisionsound.wav", NULL,SND_ASYNC|SND_FILENAME);
        win+=0.11;
    }

    if ( boatY1>=bushY1 && boatX1+13>=bushX1 && boatX1-13<=bushX2 )
    {

        PlaySound("bushcollisioneffect.wav", NULL,SND_ASYNC|SND_FILENAME);
        lose+=0.14;
    }

    if ( boatY1>=bushY3 && boatX1+13>=bushX3 && boatX1-13<=bushX4 )
    {
        PlaySound("bushcollisioneffect.wav", NULL,SND_ASYNC|SND_FILENAME);
        lose+=0.14;
    }

    if ( boatY1>=bushY5 && boatX1+13>=bushX5 && boatX1-13<=bushX6 )
    {
        PlaySound("bushcollisioneffect.wav", NULL,SND_ASYNC|SND_FILENAME);
        lose+=0.14;
    }

    Sleep(35);

   glutPostRedisplay();

}


void myBackground()
{
glClear (GL_COLOR_BUFFER_BIT);
glColor3f (0.0, 0.0, 0.0);
glPointSize(1.0);

//leftbank
glColor3ub (51, 153, 102);
glBegin(GL_QUADS);
glVertex2i(0,500);
glVertex2i(0,0);
glVertex2i(150,0);
glVertex2i(150,500);
glEnd();
//glFlush ();

//rightbank
glColor3ub (51, 153, 102);
glBegin(GL_QUADS);
glVertex2i(500,0);
glVertex2i(650,0);
glVertex2i(650,500);
glVertex2i(500,500);
glEnd();
//glFlush ();

////rightshore
glColor3ub (189.0,183.0,107.0);
glBegin(GL_QUADS);
glVertex2i(500,0);
glVertex2i(505,0);
glVertex2i(505,500);
glVertex2i(500,500);
glEnd();
//glFlush ();


//leftshore
glColor3ub (189.0,183.0,107.0);
glBegin(GL_QUADS);
glVertex2i(145,0);
glVertex2i(150,0);
glVertex2i(150,500);
glVertex2i(145,500);
glEnd();
//glFlush ();


glLineWidth(1.0);
glColor3ub (204.0,204.0,0.0);
glBegin(GL_LINES);
glVertex2i(600,250);
glVertex2i(600,240);

glColor3ub (127.0,255.0,0.0);
glVertex2i(601,250);
glVertex2i(601,240);
glEnd();
//glFlush ();

}

void Road(){
glBegin(GL_QUAD_STRIP);


glColor3ub(222.0,184.0,135.0);
glVertex2i(77,0);
glVertex2i(84,0);
glVertex2i(84,500);
glVertex2i(77,500);
//glFlush ();

glColor3ub (244.0,164.0,96.0);
glVertex2i(80,0);
glVertex2i(105,0);
glVertex2i(80,500);
glVertex2i(105,500);
//glFlush ();

glColor3ub(222.0,184.0,135.0);
glVertex2i(102,0);
glVertex2i(108,0);
glVertex2i(108,500);
glVertex2i(102,500);
//glFlush ();

glEnd();
//glFlush ();

glBegin(GL_QUAD_STRIP);


glColor3ub(222.0,184.0,135.0);
glVertex2i(555,0);
glVertex2i(562,0);
glVertex2i(562,500);
glVertex2i(555,500);
//glFlush ();

glColor3ub (244.0,164.0,96.0);
glVertex2i(557,0);
glVertex2i(582,0);
glVertex2i(557,500);
glVertex2i(582,500);
//glFlush ();

glColor3ub(222.0,184.0,135.0);
glVertex2i(580,0);
glVertex2i(587,0);
glVertex2i(587,500);
glVertex2i(580,500);

glEnd();


}

void bushTranslate()
{
    b2y -= 10;

    if(b2y <= -80)
    {
        b2y = rand() % (window_height * 2) + window_height;

    }

    glutPostRedisplay();
}





void roadSideBush()
{
    int bsy=b1y;

glBegin(GL_TRIANGLES);
glColor3ub(0, 255, 0);

glVertex2i(60, bsy+60);
glVertex2i(77, bsy+65);
glVertex2i(77, bsy+71);

glVertex2i(77, bsy+65);
glVertex2i(77, bsy+75);
glVertex2i(55, bsy+70);

glVertex2i(77, bsy+72);
glVertex2i(77, bsy+80);
glVertex2i(45, bsy+78);

glVertex2i(77, bsy+77);
glVertex2i(77, bsy+88);
glVertex2i(55, bsy+85);

glVertex2i(60, bsy+96);
glVertex2i(77, bsy+90);
glVertex2i(77, bsy+80);

glVertex2i(77,bsy+87);
glVertex2i(77,bsy+92);
glVertex2i(70,bsy+98);

glVertex2i(77,bsy+62);
glVertex2i(77,bsy+67);
glVertex2i(70,bsy+58);
glEnd();

glBegin(GL_TRIANGLES);
glColor3ub(0, 255, 0);

glVertex2i(60, bsy+160);
glVertex2i(77, bsy+165);
glVertex2i(77, bsy+171);

glVertex2i(77, bsy+165);
glVertex2i(77, bsy+175);
glVertex2i(55, bsy+170);

glVertex2i(77, bsy+172);
glVertex2i(77, bsy+180);
glVertex2i(45, bsy+178);

glVertex2i(77, bsy+177);
glVertex2i(77, bsy+188);
glVertex2i(55, bsy+185);

glVertex2i(60, bsy+196);
glVertex2i(77, bsy+190);
glVertex2i(77, bsy+180);

glVertex2i(77,bsy+187);
glVertex2i(77,bsy+192);
glVertex2i(70,bsy+198);

glVertex2i(77,bsy+162);
glVertex2i(77,bsy+167);
glVertex2i(70,bsy+158);

glEnd();


glBegin(GL_TRIANGLES);
glColor3ub(0, 255, 0);

glVertex2i(60, bsy+260);
glVertex2i(77, bsy+265);
glVertex2i(77, bsy+271);

glVertex2i(77, bsy+265);
glVertex2i(77, bsy+275);
glVertex2i(55, bsy+270);

glVertex2i(77, bsy+272);
glVertex2i(77, bsy+280);
glVertex2i(45, bsy+278);

glVertex2i(77, bsy+277);
glVertex2i(77, bsy+288);
glVertex2i(55, bsy+285);

glVertex2i(60, bsy+296);
glVertex2i(77, bsy+290);
glVertex2i(77, bsy+280);

glVertex2i(77,bsy+287);
glVertex2i(77,bsy+292);
glVertex2i(70,bsy+298);

glVertex2i(77,bsy+262);
glVertex2i(77,bsy+267);
glVertex2i(70,bsy+258);

glEnd();

glBegin(GL_TRIANGLES);
glColor3ub(0, 255, 0);

glVertex2i(60, bsy+360);
glVertex2i(77, bsy+365);
glVertex2i(77, bsy+371);

glVertex2i(77, bsy+365);
glVertex2i(77, bsy+375);
glVertex2i(55, bsy+370);

glVertex2i(77, bsy+372);
glVertex2i(77, bsy+380);
glVertex2i(45, bsy+378);

glVertex2i(77, bsy+377);
glVertex2i(77, bsy+388);
glVertex2i(55, bsy+385);

glVertex2i(60, bsy+396);
glVertex2i(77, bsy+390);
glVertex2i(77, bsy+380);

glVertex2i(77,bsy+387);
glVertex2i(77,bsy+392);
glVertex2i(70,bsy+398);

glVertex2i(77,bsy+362);
glVertex2i(77,bsy+367);
glVertex2i(70,bsy+358);
glEnd();

glBegin(GL_TRIANGLES);
glColor3ub(0, 255, 0);

glVertex2i(60, bsy+460);
glVertex2i(77, bsy+465);
glVertex2i(77, bsy+471);

glVertex2i(77, bsy+465);
glVertex2i(77, bsy+475);
glVertex2i(55, bsy+470);

glVertex2i(77, bsy+472);
glVertex2i(77, bsy+480);
glVertex2i(45, bsy+478);

glVertex2i(77, bsy+477);
glVertex2i(77, bsy+488);
glVertex2i(55, bsy+485);

glVertex2i(60, bsy+496);
glVertex2i(77, bsy+490);
glVertex2i(77, bsy+480);

glVertex2i(77,bsy+487);
glVertex2i(77,bsy+492);
glVertex2i(70,bsy+498);

glVertex2i(77,bsy+462);
glVertex2i(77,bsy+467);
glVertex2i(70,bsy+458);

glEnd();

glBegin(GL_TRIANGLES);
glColor3ub(0, 255, 0);

glVertex2i(125, bsy+20);
glVertex2i(108, bsy+25);
glVertex2i(108, bsy+31);

glVertex2i(108, bsy+25);
glVertex2i(108, bsy+35);
glVertex2i(130, bsy+30);

glVertex2i(108, bsy+32);
glVertex2i(108, bsy+40);
glVertex2i(140, bsy+38);

glVertex2i(108, bsy+37);
glVertex2i(108, bsy+48);
glVertex2i(140, bsy+45);

glVertex2i(125, bsy+56);
glVertex2i(108, bsy+50);
glVertex2i(108, bsy+40);

glVertex2i(108,bsy+47);
glVertex2i(108,bsy+52);
glVertex2i(115,bsy+58);

glVertex2i(108,bsy+22);
glVertex2i(108,bsy+27);
glVertex2i(115,bsy+18);

glEnd();

glBegin(GL_TRIANGLES);
glColor3ub(0, 255, 0);

glVertex2i(125, bsy+120);
glVertex2i(108, bsy+125);
glVertex2i(108, bsy+131);

glVertex2i(108, bsy+125);
glVertex2i(108, bsy+135);
glVertex2i(130, bsy+130);

glVertex2i(108, bsy+132);
glVertex2i(108, bsy+140);
glVertex2i(140, bsy+138);

glVertex2i(108, bsy+137);
glVertex2i(108, bsy+148);
glVertex2i(140, bsy+145);

glVertex2i(125, bsy+156);
glVertex2i(108, bsy+150);
glVertex2i(108, bsy+140);

glVertex2i(108,bsy+147);
glVertex2i(108,bsy+152);
glVertex2i(115,bsy+158);

glVertex2i(108,bsy+122);
glVertex2i(108,bsy+127);
glVertex2i(115,bsy+118);

glEnd();

glBegin(GL_TRIANGLES);
glColor3ub(0, 255, 0);

glVertex2i(125, bsy+220);
glVertex2i(108, bsy+225);
glVertex2i(108, bsy+231);

glVertex2i(108, bsy+225);
glVertex2i(108, bsy+235);
glVertex2i(130, bsy+230);

glVertex2i(108, bsy+232);
glVertex2i(108, bsy+240);
glVertex2i(140, bsy+238);

glVertex2i(108, bsy+237);
glVertex2i(108, bsy+248);
glVertex2i(140, bsy+245);

glVertex2i(125, bsy+256);
glVertex2i(108, bsy+250);
glVertex2i(108, bsy+240);

glVertex2i(108,bsy+247);
glVertex2i(108,bsy+252);
glVertex2i(115,bsy+258);

glVertex2i(108,bsy+222);
glVertex2i(108,bsy+227);
glVertex2i(115,bsy+218);

glEnd();

glBegin(GL_TRIANGLES);
glColor3ub(0, 255, 0);

glVertex2i(125, bsy+320);
glVertex2i(108, bsy+325);
glVertex2i(108, bsy+331);

glVertex2i(108, bsy+325);
glVertex2i(108, bsy+335);
glVertex2i(130, bsy+330);

glVertex2i(108, bsy+332);
glVertex2i(108, bsy+340);
glVertex2i(140, bsy+338);

glVertex2i(108, bsy+337);
glVertex2i(108, bsy+348);
glVertex2i(140, bsy+345);

glVertex2i(125, bsy+356);
glVertex2i(108, bsy+350);
glVertex2i(108, bsy+340);

glVertex2i(108,bsy+347);
glVertex2i(108,bsy+352);
glVertex2i(115,bsy+358);

glVertex2i(108,bsy+322);
glVertex2i(108,bsy+327);
glVertex2i(115,bsy+318);


glEnd();

glBegin(GL_TRIANGLES);
glColor3ub(0, 255, 0);

glVertex2i(125, bsy+420);
glVertex2i(108, bsy+425);
glVertex2i(108, bsy+431);

glVertex2i(108, bsy+425);
glVertex2i(108, bsy+435);
glVertex2i(130, bsy+430);

glVertex2i(108, bsy+432);
glVertex2i(108, bsy+440);
glVertex2i(140, bsy+438);

glVertex2i(108, bsy+437);
glVertex2i(108, bsy+448);
glVertex2i(140, bsy+445);

glVertex2i(125, bsy+456);
glVertex2i(108, bsy+450);
glVertex2i(108, bsy+440);

glVertex2i(108,bsy+447);
glVertex2i(108,bsy+452);
glVertex2i(115,bsy+458);

glVertex2i(108,bsy+422);
glVertex2i(108,bsy+427);
glVertex2i(115,bsy+418);
glEnd();
glFlush ();


//////////////////////////////////////////////////////////////////////////


glBegin(GL_TRIANGLES);
glColor3ub(0, 255, 0);

glVertex2i(537, bsy+60);
glVertex2i(550, bsy+65);
glVertex2i(550, bsy+71);

glVertex2i(550, bsy+65);
glVertex2i(550, bsy+75);
glVertex2i(528, bsy+70);

glVertex2i(550, bsy+72);
glVertex2i(550, bsy+80);
glVertex2i(518, bsy+78);

glVertex2i(550, bsy+77);
glVertex2i(550, bsy+88);
glVertex2i(528, bsy+85);

glVertex2i(533, bsy+96);
glVertex2i(550, bsy+90);
glVertex2i(550, bsy+80);

glVertex2i(550,bsy+87);
glVertex2i(550,bsy+92);
glVertex2i(543,bsy+98);

glVertex2i(550,bsy+62);
glVertex2i(550,bsy+67);
glVertex2i(543,bsy+58);
glEnd();

glBegin(GL_TRIANGLES);
glColor3ub(0, 255, 0);

glVertex2i(533, bsy+160);
glVertex2i(550, bsy+165);
glVertex2i(550, bsy+171);

glVertex2i(550, bsy+165);
glVertex2i(550, bsy+175);
glVertex2i(528, bsy+170);

glVertex2i(550, bsy+172);
glVertex2i(550, bsy+180);
glVertex2i(518, bsy+178);

glVertex2i(550, bsy+177);
glVertex2i(550, bsy+188);
glVertex2i(528, bsy+185);

glVertex2i(533, bsy+196);
glVertex2i(550, bsy+190);
glVertex2i(550, bsy+180);

glVertex2i(550,bsy+187);
glVertex2i(550,bsy+192);
glVertex2i(543,bsy+198);

glVertex2i(550,bsy+162);
glVertex2i(550,bsy+167);
glVertex2i(543,bsy+158);

glEnd();


glBegin(GL_TRIANGLES);
glColor3ub(0, 255, 0);

glVertex2i(533, bsy+260);
glVertex2i(550, bsy+265);
glVertex2i(550, bsy+271);

glVertex2i(550, bsy+265);
glVertex2i(550, bsy+275);
glVertex2i(528, bsy+270);

glVertex2i(550, bsy+272);
glVertex2i(550, bsy+280);
glVertex2i(518, bsy+278);

glVertex2i(550, bsy+277);
glVertex2i(550, bsy+288);
glVertex2i(528, bsy+285);

glVertex2i(533, bsy+296);
glVertex2i(550, bsy+290);
glVertex2i(550, bsy+280);

glVertex2i(550,bsy+287);
glVertex2i(550,bsy+292);
glVertex2i(543,bsy+298);

glVertex2i(550,bsy+262);
glVertex2i(550,bsy+267);
glVertex2i(543,bsy+258);

glEnd();

glBegin(GL_TRIANGLES);
glColor3ub(0, 255, 0);

glVertex2i(533, bsy+360);
glVertex2i(550, bsy+365);
glVertex2i(550, bsy+371);

glVertex2i(550, bsy+365);
glVertex2i(550, bsy+375);
glVertex2i(528, bsy+370);

glVertex2i(550, bsy+372);
glVertex2i(550, bsy+380);
glVertex2i(518, bsy+378);

glVertex2i(550, bsy+377);
glVertex2i(550, bsy+388);
glVertex2i(528, bsy+385);

glVertex2i(533, bsy+396);
glVertex2i(550, bsy+390);
glVertex2i(550, bsy+380);

glVertex2i(550,bsy+387);
glVertex2i(550,bsy+392);
glVertex2i(543,bsy+398);

glVertex2i(550,bsy+362);
glVertex2i(550,bsy+367);
glVertex2i(543,bsy+358);
glEnd();

glBegin(GL_TRIANGLES);
glColor3ub(0, 255, 0);

glVertex2i(533, bsy+460);
glVertex2i(550, bsy+465);
glVertex2i(550, bsy+471);

glVertex2i(550, bsy+465);
glVertex2i(550, bsy+475);
glVertex2i(528, bsy+470);

glVertex2i(550, bsy+472);
glVertex2i(550, bsy+480);
glVertex2i(518, bsy+478);

glVertex2i(550, bsy+477);
glVertex2i(550, bsy+488);
glVertex2i(528, bsy+485);

glVertex2i(533, bsy+496);
glVertex2i(550, bsy+490);
glVertex2i(550, bsy+480);

glVertex2i(550,bsy+487);
glVertex2i(550,bsy+492);
glVertex2i(543,bsy+498);

glVertex2i(550,bsy+462);
glVertex2i(550,bsy+467);
glVertex2i(543,bsy+458);

glEnd();

glBegin(GL_TRIANGLES);
glColor3ub(0, 255, 0);

glVertex2i(603, bsy+20);
glVertex2i(586, bsy+25);
glVertex2i(586, bsy+31);

glVertex2i(586, bsy+25);
glVertex2i(586, bsy+35);
glVertex2i(608, bsy+30);

glVertex2i(586, bsy+32);
glVertex2i(586, bsy+40);
glVertex2i(618, bsy+38);

glVertex2i(586, bsy+37);
glVertex2i(586, bsy+48);
glVertex2i(618, bsy+45);

glVertex2i(603, bsy+56);
glVertex2i(586, bsy+50);
glVertex2i(586, bsy+40);

glVertex2i(586,bsy+47);
glVertex2i(586,bsy+52);
glVertex2i(593,bsy+58);

glVertex2i(586,bsy+22);
glVertex2i(586,bsy+27);
glVertex2i(593,bsy+18);

glEnd();

glBegin(GL_TRIANGLES);
glColor3ub(0, 255, 0);

glVertex2i(603, bsy+120);
glVertex2i(586, bsy+125);
glVertex2i(586, bsy+131);

glVertex2i(586, bsy+125);
glVertex2i(586, bsy+135);
glVertex2i(608, bsy+130);

glVertex2i(586, bsy+132);
glVertex2i(586, bsy+140);
glVertex2i(618, bsy+138);

glVertex2i(586, bsy+137);
glVertex2i(586, bsy+148);
glVertex2i(618, bsy+145);

glVertex2i(603, bsy+156);
glVertex2i(586, bsy+150);
glVertex2i(586, bsy+140);

glVertex2i(586,bsy+147);
glVertex2i(586,bsy+152);
glVertex2i(603,bsy+158);

glVertex2i(586,bsy+122);
glVertex2i(586,bsy+127);
glVertex2i(593,bsy+118);

glEnd();

glBegin(GL_TRIANGLES);
glColor3ub(0, 255, 0);

glVertex2i(603, bsy+220);
glVertex2i(586, bsy+225);
glVertex2i(586, bsy+231);

glVertex2i(586, bsy+225);
glVertex2i(586, bsy+235);
glVertex2i(608, bsy+230);

glVertex2i(586, bsy+232);
glVertex2i(586, bsy+240);
glVertex2i(618, bsy+238);

glVertex2i(586, bsy+237);
glVertex2i(586, bsy+248);
glVertex2i(618, bsy+245);

glVertex2i(603, bsy+256);
glVertex2i(586, bsy+250);
glVertex2i(586, bsy+240);

glVertex2i(586,bsy+247);
glVertex2i(586,bsy+252);
glVertex2i(593,bsy+258);

glVertex2i(586,bsy+222);
glVertex2i(586,bsy+227);
glVertex2i(593,bsy+218);

glEnd();

glBegin(GL_TRIANGLES);
glColor3ub(0, 255, 0);

glVertex2i(603, bsy+320);
glVertex2i(586, bsy+325);
glVertex2i(586, bsy+331);

glVertex2i(586, bsy+325);
glVertex2i(586, bsy+335);
glVertex2i(608, bsy+330);

glVertex2i(586, bsy+332);
glVertex2i(586, bsy+340);
glVertex2i(618, bsy+338);

glVertex2i(586, bsy+337);
glVertex2i(586, bsy+348);
glVertex2i(618, bsy+345);

glVertex2i(603, bsy+356);
glVertex2i(586, bsy+350);
glVertex2i(586, bsy+340);

glVertex2i(586,bsy+347);
glVertex2i(586,bsy+352);
glVertex2i(593,bsy+358);

glVertex2i(586,bsy+322);
glVertex2i(586,bsy+327);
glVertex2i(593,bsy+318);


glEnd();

glBegin(GL_TRIANGLES);
glColor3ub(0, 255, 0);

glVertex2i(603, bsy+420);
glVertex2i(586, bsy+425);
glVertex2i(586, bsy+431);

glVertex2i(586, bsy+425);
glVertex2i(586, bsy+435);
glVertex2i(608, bsy+430);

glVertex2i(586, bsy+432);
glVertex2i(586, bsy+440);
glVertex2i(618, bsy+438);

glVertex2i(586, bsy+437);
glVertex2i(586, bsy+448);
glVertex2i(618, bsy+445);

glVertex2i(603, bsy+456);
glVertex2i(586, bsy+450);
glVertex2i(586, bsy+440);

glVertex2i(586,bsy+447);
glVertex2i(586,bsy+452);
glVertex2i(593,bsy+458);

glVertex2i(586,bsy+422);
glVertex2i(586,bsy+427);
glVertex2i(593,bsy+418);
glEnd();
glFlush ();





////////////////////////////////////////////////////////////////////



bushTranslate();
}


void Boat()
{
    int bx = mx;
    int by = my;

//boat shape
glBegin(GL_POLYGON);
glColor3ub(204, 51, 0);
glVertex2i(bx+110, by+12);
glVertex2i(bx+97, by+38);
glVertex2i(bx+97, by+95);
glVertex2i(boatX1=bx+110, boatY1=by+125);
glVertex2i(bx+123, by+95);
glVertex2i(bx+123, by+38);
glEnd();

//human1 shape + handle
glBegin(GL_POLYGON);
glColor3ub(0, 0, 0);
glVertex2i(bx+111, by+44);
glVertex2i(bx+104, by+49);
glVertex2i(bx+104, by+57);
glVertex2i(bx+111, by+62);
glVertex2i(bx+117, by+57);
glVertex2i(bx+117, by+49);
glEnd();
glBegin(GL_LINES);
glColor3ub(0, 0, 0);
glVertex2i(bx+108, by+71);
glVertex2i(bx+138, by+44);
glVertex2i(bx+104, by+51);
glVertex2i(bx+108, by+71);
glVertex2i(bx+117, by+49);
glVertex2i(bx+121, by+59);
glEnd();
//human2 shape + handle
glBegin(GL_POLYGON);
glColor3ub(0, 0, 0);
glVertex2i(bx+111, by+71);
glVertex2i(bx+104, by+76);
glVertex2i(bx+104, by+84);
glVertex2i(bx+111, by+89);
glVertex2i(bx+117, by+84);
glVertex2i(bx+117, by+76);
glEnd();
glBegin(GL_LINES);
glColor3ub(0, 0, 0);
glVertex2i(bx+108, by+98);
glVertex2i(bx+138, by+71);
glVertex2i(bx+104, by+78);
glVertex2i(bx+108, by+98);
glVertex2i(bx+117, by+76);
glVertex2i(bx+121, by+86);
glEnd();
//glFlush ();
}

void Coin()
{
    int cx=c1x;
    int cy=c1y;

glBegin(GL_POLYGON);
glColor3ub(255, 165, 0);
glVertex2i(coinX1=cx+26, coinY1=cy+12);
glVertex2i(cx+19, cy+15);
glVertex2i(cx+16, cy+22);
glVertex2i(cx+19, cy+28);
glVertex2i(cx+26, cy+31);
glVertex2i(cx+32, cy+28);
glVertex2i(cx+35, cy+22);
glVertex2i(cx+33, cy+15);
glEnd();
//glFlush ();
}

void Bush()
{
    int bsx=b1x;
    int bsy=b1y;

glBegin(GL_TRIANGLES);
glColor3ub(0, 255, 0);
glVertex2i(bsx+45, bsy+71);
glVertex2i(bushX2=bsx+40, bushY2=bsy+55);  // right most value
glVertex2i(bsx+34, bsy+55);
glVertex2i(bsx+40, bsy+81);
glVertex2i(bsx+40, bsy+55);
glVertex2i(bsx+30, bsy+55);
glVertex2i(bsx+31, bsy+88);
glVertex2i(bsx+36, bsy+55);
glVertex2i(bsx+26, bsy+55);
glVertex2i(bsx+24, bsy+84);
glVertex2i(bsx+31, bsy+55);
glVertex2i(bsx+19, bsy+55);
glVertex2i(bsx+17, bsy+92);
glVertex2i(bsx+22, bsy+55);
glVertex2i(bsx+12, bsy+55);
glVertex2i(bsx+9, bsy+80);
glVertex2i(bsx+20, bsy+55);
glVertex2i(bsx+8, bsy+55);
glVertex2i(bsx+2, bsy+73);
glVertex2i(bsx+13, bsy+55);
glVertex2i(bushX1=bsx+7, bushY1=bsy+55);  //left most value
glEnd();
//glFlush ();

}

void Bush2()
{
    int bsx=b3x;
    int bsy=b3y;

glBegin(GL_TRIANGLES);
glColor3ub(0, 255, 0);
glVertex2i(bsx+45, bsy+71);
glVertex2i(bushX4=bsx+40, bushY4=bsy+55);  // right most value
glVertex2i(bsx+34, bsy+55);
glVertex2i(bsx+40, bsy+81);
glVertex2i(bsx+40, bsy+55);
glVertex2i(bsx+30, bsy+55);
glVertex2i(bsx+31, bsy+88);
glVertex2i(bsx+36, bsy+55);
glVertex2i(bsx+26, bsy+55);
glVertex2i(bsx+24, bsy+84);
glVertex2i(bsx+31, bsy+55);
glVertex2i(bsx+19, bsy+55);
glVertex2i(bsx+17, bsy+92);
glVertex2i(bsx+22, bsy+55);
glVertex2i(bsx+12, bsy+55);
glVertex2i(bsx+9, bsy+80);
glVertex2i(bsx+20, bsy+55);
glVertex2i(bsx+8, bsy+55);
glVertex2i(bsx+2, bsy+73);
glVertex2i(bsx+13, bsy+55);
glVertex2i(bushX3=bsx+7, bushY3=bsy+55);  //left most value
glEnd();
//glFlush ();

}

void Bush3()
{
    int bsx=b4x;
    int bsy=b4y;

glBegin(GL_TRIANGLES);
glColor3ub(0, 255, 0);
glVertex2i(bsx+45, bsy+71);
glVertex2i(bushX5=bsx+40, bushY5=bsy+55);  // right most value
glVertex2i(bsx+34, bsy+55);
glVertex2i(bsx+40, bsy+81);
glVertex2i(bsx+40, bsy+55);
glVertex2i(bsx+30, bsy+55);
glVertex2i(bsx+31, bsy+88);
glVertex2i(bsx+36, bsy+55);
glVertex2i(bsx+26, bsy+55);
glVertex2i(bsx+24, bsy+84);
glVertex2i(bsx+31, bsy+55);
glVertex2i(bsx+19, bsy+55);
glVertex2i(bsx+17, bsy+92);
glVertex2i(bsx+22, bsy+55);
glVertex2i(bsx+12, bsy+55);
glVertex2i(bsx+9, bsy+80);
glVertex2i(bsx+20, bsy+55);
glVertex2i(bsx+8, bsy+55);
glVertex2i(bsx+2, bsy+73);
glVertex2i(bsx+13, bsy+55);
glVertex2i(bushX6=bsx+7, bushY6=bsy+55);  //left most value
glEnd();
//glFlush ();

}

void myDisplay()
{
    if (windowFlag!=0)
    {

       glClear (GL_COLOR_BUFFER_BIT);

       myBackground();
       Road();
       Bush();
       Bush2();
       Bush3();
       roadSideBush();
       Coin();
       Boat();

       drawScoreValue(GLUT_BITMAP_TIMES_ROMAN_24,screenTime,6,580,420);
       drawScoreValue(GLUT_BITMAP_TIMES_ROMAN_24,win,6,40,460);
       drawScoreValue(GLUT_BITMAP_TIMES_ROMAN_24,lose,6,40,420);
       drawScore(GLUT_BITMAP_TIMES_ROMAN_24,"Total Time: 1 Minute",530,460);
       drawScore(GLUT_BITMAP_TIMES_ROMAN_24,"Counter:",530,420);
       drawScore(GLUT_BITMAP_TIMES_ROMAN_24,"Coin: ",10,460);
       drawScore(GLUT_BITMAP_TIMES_ROMAN_24,"bush:", 10,420);

       glFlush();

        countTime();

       translate();
    }
    else
    {
        homePage();
    }
}

int main(int argc, char** argv)
{
    glutInit(&argc, argv);
    glutInitDisplayMode (GLUT_SINGLE | GLUT_RGB);
    glutInitWindowSize (1348, 700);
    glutInitWindowPosition (0.0,0.0);
    glutCreateWindow ("Boat Racing");
    glutDisplayFunc(myDisplay);
    glutReshapeFunc(reshape);
    glutKeyboardFunc(keyboard);
    glutSpecialFunc(keyboardAction);
    myInit ();
    srand(clock());
    glutMainLoop();

    return 0;
}


