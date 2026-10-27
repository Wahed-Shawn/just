```
1.DDA
#include <stdio.h>
#include <graphics.h>
#include <conio.h>
#include <math.h>
#include <stdlib.h>
#include <dos.h>

float round (float a);

int main() {
    int gd = DETECT,gm;
    initgraph(&gd, &gm, "");

    int x1,y1,x2,y2,steps,k;
    float xincr,yincr,x,y,dx,dy;

    scanf("%d%d", &x1,&y1);
    scanf("%d%d", &x2,&y2);

    dx = x2 - x1;
    dy = y2 - y1;

    if(abs(dx) > abs(dy))
        steps = abs(dx);
    else
        steps = abs(dy);

    xincr = dx/steps;
    yincr = dy/steps;

    x = x1;
    y = y1;


    for(k = 1; k <= steps; k++){
        delay(10);
        x += xincr;
        y += yincr;
        putpixel(round(x),round(y), WHITE);
    }
    outtextxy(200,200, "DDA");
    outtextxy(x1+5,y1-5, "(x1,y1)");
    outtextxy(x2+5,y2-5, "(x2,y2)");

    getch();
    closegraph();
    return 0;
}

     float round(float a){
        int b = a + 0.5;
        return b;
    }


2.Bressenham line drawing
#include <stdio.h>
#include <graphics.h>
#include <conio.h>
#include <math.h>
#include <stdlib.h>
#include <dos.h>

float round (float a);

int main() {
    int gd = DETECT,gm;
    initgraph(&gd, &gm, "");

    int x,y,x1,y1,x2,y2,p,dx,dy;
    scanf("%d%d",&x1,&y1);
    scanf("%d%d",&x2,&y2);

    x = x1;
    y = y1; 

    dx = x2 - x1;
    dy = y2 - y1;

    putpixel(x,y,2);
    p = (2*dy - dx);

    while(x <= x2){
        if(p < 0){
            x = x + 1;
            p = p + 2 * dy;
        }

        else{
            x = x+1;
            y = y+1;
            p = p + 2*(dy - dx);
        }
        putpixel(x,y,7);
    }
   
    getch();
    closegraph();
    return 0;
}

3.Mid point circle 
#include <stdio.h>
#include <graphics.h>
#include <conio.h>
#include <math.h>
#include <stdlib.h>
#include <dos.h>

void draw_circle(int xc, int yc, int r);
void symmetry(int xc, int yc, int x, int y);

float round (float a);

int main() {
    int gd = DETECT,gm;
    initgraph(&gd, &gm, "");

    int xc,yc, r;
    scanf ("%d%d%d", &xc,&yc,&r);

    draw_circle(xc,yc,r);
   
    getch();
    closegraph();
    return 0;
}

void draw_circle(int xc, int yc, int r){
    int x = 0;
    int y = r;
    int p = 1 - r;

    symmetry(xc,yc,x,y);

    while(x < y){
        x++;

        if(p < 0){
            p = p + 2*x + 3;
        }
        else {
            y--;
            p = p + 2*(x-y) + 5;
        }
        symmetry(xc,yc,x,y);
    }
}

void symmetry(int xc, int yc, int x, int y){
    putpixel(xc+x,yc+y,GREEN);
    putpixel(xc-x,yc+y,GREEN);
    putpixel(xc+x,yc-y,GREEN);
    putpixel(xc-x,yc-y,GREEN);
    putpixel(xc+y,yc+x,GREEN);
    putpixel(xc-y,yc+x,GREEN);
    putpixel(xc+y,yc-x,GREEN);
    putpixel(xc-y,yc-x,GREEN);
}

4.Ellips generation algorithm
#include <stdio.h>
#include <graphics.h>
#include <conio.h>
#include <math.h>
#include <stdlib.h>
#include <dos.h>


void drawEllipse(int xc, int yc, int rx, int ry);

int main() {
    int gd = DETECT,gm;
    initgraph(&gd, &gm, "");

    int xc,yc,rx,ry;

    scanf("%d%d%d%d", &xc,&yc,&rx,&ry);

    drawEllipse(xc,yc,rx,ry);
   
    getch();
    closegraph();
    return 0;
}

void drawEllipse(int xc, int yc, int rx, int ry)
{
    float x = 0;
    float y = ry;
    float rx2 = rx * rx;
    float ry2 = ry * ry;
    float dx = 2 * ry2 * x;
    float dy = 2 * rx2 * y;

    float p1 = ry2 - (rx2 * ry) + (0.25 * rx2);

    while (dx < dy)
    {
        putpixel(xc + x, yc + y, WHITE);
        putpixel(xc - x, yc + y, WHITE);
        putpixel(xc + x, yc - y, WHITE);
        putpixel(xc - x, yc - y, WHITE);

        if (p1 < 0)
        {
            x++;
            dx = dx + (2 * ry2);
            p1 = p1 + dx + ry2;
        }
        else
        {
            x++;
            y--;
            dx = dx + (2 * ry2);
            dy = dy - (2 * rx2);
            p1 = p1 + dx - dy + ry2;
        }
    }

    float p2 = ry2 * (x + 0.5) * (x + 0.5)
             + rx2 * (y - 1) * (y - 1)
             - rx2 * ry2;

    while (y >= 0)
    {
        putpixel(xc + x, yc + y, WHITE);
        putpixel(xc - x, yc + y, WHITE);
        putpixel(xc + x, yc - y, WHITE);
        putpixel(xc - x, yc - y, WHITE);

        if (p2 > 0)
        {
            y--;
            dy = dy - (2 * rx2);
            p2 = p2 + rx2 - dy;
        }
        else
        {
            y--;
            x++;
            dx = dx + (2 * ry2);
            dy = dy - (2 * rx2);
            p2 = p2 + dx - dy + rx2;
        }
    }
}

5.Create various types of text and fonts
#include <stdio.h>
#include <graphics.h>
#include <conio.h>
#include <math.h>
#include <stdlib.h>
#include <dos.h>


int main() {
    int gd = DETECT,gm;
    initgraph(&gd, &gm, "");

    int x = 50, y = 50;

    for(int font = 0; font <= 4; font++){
        settextstyle(font, HORIZ_DIR, 1 + font);
        setcolor(font + 1);
        outtextxy(x,y,"Text with different fonts");
        y += 50;
    }

    x = 500, y = 80;
    for( font = 0; font <= 2; font++){
        settextstyle(font,VERT_DIR, 1+font);
        setcolor(font+1);
        outtextxy(x,y, "Vertical texr");
        x += 50;
    }
   
    getch();
    closegraph();
    return 0;
}
```
