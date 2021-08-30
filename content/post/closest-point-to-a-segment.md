+++
title = "How to calculate the closest point to a segment"
date = "2010-06-06"
tags = [
    "c++",
    "programming",
    "geometry",
    "snippet",
]
thumbnail = "images/posts/geometria.png"
featured = false
+++

![](/images/posts/geometria.png "")

Hi, below there is a little snippet that calculates the closest point to a segment. It is written in C++ 
but can be easily translated to any other language. 

Disclaimer: given its age, it uses hungarian notation, and some names are in Spanish, but it was deeply 
tested and works as expected. 

```c++
//--------------------------------------------------------------------------------------------------------
//author: Adrián Deccico - http://adrian.org.ar
/*
Description: calculate the closest point belonging to a given segment.
Parameters:
nSx1, nSy1 =  First point of the segment
nSx2, int nSy2 = Second point of the segment
nPx, int nPy = The point to analize
*nCercaX,  *nCercaY = The closest point to the given point (nPx,nPy) that belongs to the segment
Returns: The distance between the given point and the point found of the segment
*/
double ElPuntoMasCercanoAUnSegmento(double nSx1, double nSy1, double nSx2,
double nSy2, double nPx, double nPy,
double *nCercaX,  double *nCercaY)
{
 
//caracterizo la recta del segmento dado Y=m*X + b
double m, b;
 
bool bEsHorizontalOVertical = false;
 
//el segmento es vertical
if (nSx2 == nSx1){
    *nCercaX = nSx1;
    *nCercaY = nPy;
    bEsHorizontalOVertical = true;
}
 
//el segmento es horizontal
if (nSy2 == nSy1){
    *nCercaY = nSy1;
    *nCercaX = nPx;
    bEsHorizontalOVertical = true;
}
 
//el segmento está caracterizado por una recta "AX + BY + C = 0" con
// A y B distintos de cero
if (!bEsHorizontalOVertical)
{
    m= (nSy2 - nSy1) / (nSx2 - nSx1);
    b = nSy1 - (nSx1 * m);
 
    //caracterizo la recta que une al punto dado con el punto más
    // cercano dentro del segmento Y=m1*X + b1
    double m1, b1;
 
    m1 = -1 / m;
    b1 = -m1 * nPx + nPy;
 
    //encuentro el punto de intersección
    *nCercaX =  (b1 - b) / (m - m1);
    *nCercaY =  m * *nCercaX + b;
}
 
//verificar que el punto encontrado está dentro del segmento
if (*nCercaX &lt; min(nSx1,nSx2)
||  *nCercaX &gt; max(nSx1,nSx2)
||  *nCercaY &lt; min(nSy1,nSy2)
||  *nCercaY &gt; max(nSy1,nSy2) )
{
    //el punto más cercano será uno de los dos extremos (el más cercano al punto)
    if (dDistanciaEntrePuntos(nSx1, nSy1, nPx, nPy) <= dDistanciaEntrePuntos(nSx2, nSy2, nPx, nPy))
    {
        *nCercaX = nSx1;
        *nCercaY = nSy1;
    }
    else
    {
        *nCercaX = nSx2;
        *nCercaY = nSy2;
    }
}
 
return dDistanciaEntrePuntos(nPx, nPy, *nCercaX , *nCercaY);
 
}
//--------------------------------------------------------------------------------------------
 
//------------------------------------------------------------------------
//return the distance between two points in a x,y plane.
double dDistanciaEntrePuntos(double lOldX, double lOldY,
double lNewX, double lNewY)
{
    double x = lNewX - lOldX;
    double y = lNewY - lOldY;
 
    return sqrt(pow(x,2) + pow(y,2));
}
//------------------------------------------------------------------------
```