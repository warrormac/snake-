#include <iostream>
#include <windows.h>
#include <conio.h>
#include <stdlib.h>



#define ARRIBA    72
#define IZQUIERDA 75
#define DERECHA   77
#define ABAJO     80     // numeros asociados a las teclas del teclado
#define ESC       27


using namespace std;




int body [200][2];
char tecla;
int n=1;
int tam = 10;
int x = 10 , y = 12;
int xc=30, yc=15;
int dir=3  ;


void gotoxy(int x, int y)  //dibujadora da coordenadas
{
    HANDLE ident;  //identificador
    ident = GetStdHandle(STD_OUTPUT_HANDLE);
    COORD pos1; //coordenadas
    pos1.X=x;
    pos1.Y=y;
    SetConsoleCursorPosition(ident,pos1); //da la posicion a lo que quieras

}

void pintar() //margen
{
    for (int i =2;i<78;i++)
    {
        gotoxy(i,3);
        cout<<"-";
        gotoxy(i,23);
        cout<<"-";
    }
    for (int i=4;i<24;i++)
    {
        gotoxy(2,i);
        cout<<"|";
        gotoxy(77,i);
        cout<<"|";
    }


}

void posguardada()
{
    body[n][0]=x;
    body[n][1]=y;
    n++;
    if(n==tam)
        n=1;
}

void dibujarcuerpo()
{
    for(int i=1;i<tam;i++)
    {
        gotoxy(body[i][0],body[i][1]);
        cout<<"*";
    }
}

void borrar()
{
    gotoxy(body[n][0],body[n][1]);
    cout<<" ";

}

void movimiento()
{
        if(kbhit())
    {
        tecla = getch();
        switch (tecla)
        {

        case ARRIBA:
            if(dir != 2)
            {
                dir = 1;
                break;
            }
        case ABAJO:
            if(dir != 1)
            {
                dir = 2;
                break;
            }
        case DERECHA:
            if(dir !=4)
            {
                dir =3;
                break;

            }
        case IZQUIERDA:
            if(dir!=3)
            {
                dir = 4;
                break;
            }
        }
    }
}

void comida()
{
    if(xc == x and yc == y)
        {

        xc = (rand()%73)+4;
        yc = (rand()%19)+4;
        tam++;
        gotoxy(xc,yc);
        cout<<"^";
     }
}

bool choque_pared()
{
    if(y==2 or y==24 or x==1 or x==78)
    {
        return true;
    }
    for(int j = tam -1;j>0;j--)
    {
        if(body[j][0]==x and body[j][1]==y)
            return true;
    }
}


int main()
{
    pintar();
        gotoxy(xc,yc);
        cout<<"^";

while(tecla!=ESC and !choque_pared())
{
    borrar();
    posguardada();
    dibujarcuerpo();
    comida();
    movimiento();
    movimiento();

if(dir == 1)
{
     y--;
}
if(dir == 2)
{
     y++;
}
if(dir == 3)
{
     x++;
}
if(dir == 4)
{
     x--;
}

    Sleep(80);
}


system("pause");

}
