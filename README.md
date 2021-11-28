# Graphical-Editor-c-sharp
Graphical Editor (Console App)

To run tests: #run with c#

------original challenge------

Coding challenge

Graphical editors allow users to edit images in the same way text editors let us modify documents. Images are represented as an M x N array of pixels with each pixel given colour.
Produce a program that simulates a simple interactive graphical editor.

Input

The input consists of a line containing a sequence of commands. Each command is represented by a single capital letter at the start of the line. Arguments to the command are separated by spaces and follow the command character.

Pixel co-ordinates are represented by a pair of integers: 1) a column number between 1 and M, and 2) a row number between 1 and N. Where 1 <= M, N <= 250. The origin sits in the upper-left of the table. Colours are specified by capital letters.

Commands
The editor supports 7 commands:
1. I M N. Create a new M x N image with all pixels coloured white (O).
2. C. Clears the table, setting all pixels to white (O).
3. L X Y C. Colours the pixel (X,Y) with colour C.
4. V X Y1 Y2 C. Draw a vertical segment of colour C in column X between rows Y1 and Y2
(inclusive).
5. H X1 X2 Y C. Draw a horizontal segment of colour C in row Y between columns X1 and 
X2
(inclusive).
6. F X Y C. Fill the region R with the colour C. R is defined as: Pixel (X,Y) belongs to R. Any 
other pixel which is the same colour as (X,Y) and shares a common side with any pixel in R 
also belongs to this region.
7. S. Show the contents of the current image
8. X. Terminate the session
Example
In the example below, > denotes input, => denotes program output.

> I 5 6
> L 2 3 U
> S
=>
OOOOO
OOOOO
OUOOO
OOOOO
OOOOO
OOOOO
Página 2 de 2
> F 3 3 R
> V 2 3 4 N
> H 3 4 2 K
> S
=>
RRRRR
RRKKR
RNRRR
RNRRR
RRRRR
RRRRR


Submission

We prefer submissions in Ruby although we will accept them in other languages. 
Please provide an executable solution with any source files in a common archive format (ZIP,  TAR etc.).


using System;
using System.Collections.Generic;
using System.Linq;
using System.Text;
using System.Threading.Tasks;

namespace grafico_editor
{
    class Program
    {

       
        static void preencher(ref string[,] myArr2, int yy, int xx, ref string fnc, ref int m, ref int n)
        {
            // verifica se é maior que 0 os valores introduzido e se é menor que os valores da matriz definidos no inicio exemplo I 5 5
            // ter atenção se o valores introduzidos de preenchimento são maiores que 0 e menores que a dimensão da matriz
            if(yy>=0 && xx>=0 && yy < m && xx < n)
            {
                // verifica se é igual à cor antiga 
                if (myArr2[yy, xx] == "O")
                {
                    //preenche com a nova cor
                    myArr2[yy, xx] = fnc;

                preencher(ref myArr2, yy + 1, xx, ref fnc, ref m, ref n);
                preencher(ref myArr2, yy, xx - 1, ref fnc, ref m, ref n);
                preencher(ref myArr2, yy - 1, xx, ref fnc, ref m, ref n);
                preencher(ref myArr2, yy, xx + 1, ref fnc, ref m, ref n);
               
                }
            }
                   
        }

        static void Main(string[] args)
        {
                int m = 0, n = 0;
                int mPtr = m;
                int nPtr = n;
                string c;

           

            Console.WriteLine("The input consists of a line containing a sequence of commands:");
                String[,] myArr2 = new String[250, 250];
                bool sair = true;
                do
                {

                    string opcao = Console.ReadLine();
                    
                    var strlist = opcao.Split(' ');

                    switch (strlist[0])
                    {
                        case "I":
                            m = Convert.ToInt32(strlist[1]);
                            n = Convert.ToInt32(strlist[2]);

                            for (int j = 0; j < n; j++)
                            {
                                for (int i = 0; i < m; i++)
                                {
                                    myArr2[j, i] = "O";
                                }
                            }
                            break;
                        case "C":
                                for (int j = 0; j < n; j++)
                                {
                                    for (int i = 0; i < m; i++)
                                    {
                                        myArr2[j, i] = "O";
                                    }
                                }
                            break;
                        case "L":
                            int x = Convert.ToInt32(strlist[2]) - 1;
                            int y = Convert.ToInt32(strlist[1]) - 1;
                            string color = strlist[3];
                            myArr2[x, y] = strlist[3];

                            break;
                        case "F":
                            int fx = Convert.ToInt32(strlist[2]) - 1;
                            int fy = Convert.ToInt32(strlist[1]) - 1;
                            string fc = strlist[3];
                            c = myArr2[fx, fy];

                      
                            preencher(ref myArr2, fy, fx, ref fc, ref m, ref n);
                            
                        break;
                        case "V":
                            int vx = Convert.ToInt32(strlist[1]) - 1;
                            int vy1 = Convert.ToInt32(strlist[2]) - 1;
                            int vy2 = Convert.ToInt32(strlist[3]) - 1;

                            c = strlist[4];
                            if (vy2 > vy1)
                            {
                                for (int v = vy1; v <= vy2; v++)
                                {
                                    myArr2[v, vx] = c;
                                    //myArr2[v, vx] = c;
                                }
                            } else
                            {
                                for (int v = vy2; v <= vy1; v++)
                                {
                                    myArr2[v, vx] = c;
                                    //myArr2[v, vx] = c;
                                }
                            }
                            
                            break;
                        case "H":
                            int hx1 = Convert.ToInt32(strlist[1]) - 1;
                            int hx2 = Convert.ToInt32(strlist[2]) - 1;
                            int hy = Convert.ToInt32(strlist[3]) - 1;
                            c = strlist[4];

                            if (hx1 < hx2) { 
                                for (int h = hx1; h <= hx2; h++)
                                {
                                    myArr2[hy, h] = c;            
                                }
                            }
                            else
                            {   
                                for (int h = hx2; h <= hx1; h++)
                                {
                                    myArr2[hy, h] = c;
                                }
                            }

                        break;
                        case "S":
                                for (int j = 0; j < n; j++)
                                {
                                    for (int i = 0; i < m; i++)
                                    {
                                        Console.Write(myArr2[j, i]);
                                    }
                                    Console.WriteLine();
                                }
                            break;
                        case "X":
                            sair = false;
                            break;
                        default:
                            Console.WriteLine("Escolha uma das opçõe ");
                            break;
                    }
                } while (sair);
        }

    }
}
