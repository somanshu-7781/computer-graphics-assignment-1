# computer-graphics-assignment-1
#include <graphics.h>
#include <conio.h>
#include <stdlib.h>

void dda(int x1, int y1, int x2, int y2, int style)
{
    int dx, dy, steps, i;
    float x, y, xin, yin;

    dx = x2 - x1;
    dy = y2 - y1;

    if (abs(dx) > abs(dy))
        steps = abs(dx);
    else
        steps = abs(dy);

    if (steps == 0)
    {
        putpixel(x1, y1, WHITE);
        return;
    }

    xin = (float)dx / steps;
    yin = (float)dy / steps;

    x = x1;
    y = y1;

    for (i = 0; i <= steps; i++)
    {
        if (style == 1)
        {
            /* Dotted line */
            if (i % 5 == 0)
                putpixel((int)(x + 0.5), (int)(y + 0.5), WHITE);
        }
        else
        {
           
            setcolor(WHITE);
            setfillstyle(SOLID_FILL, WHITE);
            bar((int)x - 1, (int)y - 1,
                (int)x + 2, (int)y + 2);
        }

        x = x + xin;
        y = y + yin;
    }
}


void bresenham(int x1, int y1, int x2, int y2, int style)
{
    int dx, dy, sx, sy, err, e2, count;

    dx = abs(x2 - x1);
    dy = abs(y2 - y1);

    if (x1 < x2)
        sx = 1;
    else
        sx = -1;

    if (y1 < y2)
        sy = 1;
    else
        sy = -1;

    err = dx - dy;
    count = 0;

    while (1)
    {
        if (style == 1)
        {
            /* Dashed line */
            if (count % 10 < 6)
                putpixel(x1, y1, YELLOW);
        }
        else
        {
            /* Solid line */
            putpixel(x1, y1, GREEN);
        }

        if (x1 == x2 && y1 == y2)
            break;

        e2 = 2 * err;

        if (e2 > -dy)
        {
            err = err - dy;
            x1 = x1 + sx;
        }

        if (e2 < dx)
        {
            err = err + dx;
            y1 = y1 + sy;
        }

        count++;
    }
}

int main()
{
    int gd, gm;

    gd = DETECT;
    gm = 0;

    initgraph(&gd, &gm, "C:\\TC\\BGI");
    cleardevice();

    setcolor(WHITE);
    outtextxy(40, 20, "DDA Dotted Outer Rectangle");

    /* Outer dotted rectangle */
    dda(40, 50, 250, 50, 1);
    dda(250, 50, 250, 200, 1);
    dda(250, 200, 40, 200, 1);
    dda(40, 200, 40, 50, 1);

    outtextxy(80, 95, "DDA Thick Inner Rectangle");

    /* Inner thick rectangle */
    dda(80, 110, 210, 110, 2);
    dda(210, 110, 210, 150, 2);
    dda(210, 150, 80, 150, 2);
    dda(80, 150, 80, 110, 2);

    /* Dashed diamond */
    bresenham(145, 50, 250, 125, 1);
    bresenham(250, 125, 145, 200, 1);
    bresenham(145, 200, 40, 125, 1);
    bresenham(40, 125, 145, 50, 1);

    /* Solid diamond */
    setcolor(GREEN);
    outtextxy(320, 20, "Bresenham Solid Diamond");

    bresenham(410, 70, 480, 125, 2);
    bresenham(480, 125, 410, 180, 2);
    bresenham(410, 180, 340, 125, 2);
    bresenham(340, 125, 410, 70, 2);

    getch();
    closegraph();
    return 0;
}
