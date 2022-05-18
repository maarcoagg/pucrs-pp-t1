O objetivo principal deste trabalho é implementar, usando a ferramenta OpenMP, uma versão paralela de um programa que calcula a área de um Conjunto de Mandelbrot. Ao mesmo tempo que serão usadas as diretivas de OpenMP vistas em aula sobre um problema real, será possível também fazer uma reflexão sobre as questões que devem ser levadas em consideração quando um código sequencial é paralelizado.

O Conjunto de Mandelbrot (2022) é um conjunto de números complexos c para os quais, a partir de uma condição inicial z=c, a iteração de z=z2+c não diverge. Para determinar se um ponto c está neste conjunto, deve-se realizar determinado número de iterações sobre z, sendo que, se ao final dessas iterações, |z| > 2, então o ponto está fora do conjunto de Mandelbrot.

Neste trabalho será utilizada uma versão sequencial de um código que realiza um cálculo estimativo da área de um Conjunto de Mandelbrot (BULL, 2019). Essa versão de código sequencial encontra-se no final deste enunciado. Nesse código, geram-se pontos de forma homogênea sobre uma área quadrada e para cada ponto realiza-se a iteração desse ponto usando a fórmula indicada 2000 vezes. Se depois dessas 2000 iterações o módulo de z for maior do que 2, então o ponto é considerado fora do Conjunto de Mandelbrot. Contando-se quantos dos pontos estão dentro do conjunto, pode-se obter uma estimativa da área do conjunto.

Sua tarefa é paralelizar este código com as diretivas de OpenMP, buscando colocá-las nos melhores pontos e usando as cláusulas adequadas. Procure identificar qual laço será paralelizado, quais são as suas variáveis compartilhadas e privadas, se há seções críticas ou possibilidade de usar reduções e qual a melhor cláusula para escalonamento das iterações de cada laço paralelizado.

Após paralelizar a versão sequencial, o programa deve ser executado em um nodo do cluster grad com 2, 4, 8 e 16 threads, e o número de pontos (NPOINTS) variando de 500 até 5.000, de 500 em 500. Observe que cada resultado paralelo deve gerar resultados iguais à respectiva execução sequencial, porém com tempos de execução provavelmente menores.

Cada aluno ou grupo (de no máximo 2 integrantes) deve entregar um relatório em .PDF com: descrição dos testes realizados, descrição do processador, resultados obtidos (gráficos mostrando os resultados de várias execuções), análise dos resultados e conclusões.

Os itens para avaliação são:

[x] formação de um grupo de no máximo 2 integrantes em até uma semana após a divulgação do enunciado do trabalho;

[x] execução da versão sequencial abaixo (para viabilizar o cálculo de speed-up e eficiência);

[ ] implementação da versão paralela do algoritmo em C e OpenMP (considerando a estratégia e a complexidade da solução paralela);

[ ] cálculo de speed up e de eficiência para os testes paralelos executados;

[ ] análise do balanceamento da carga na execução do programa paralelo;

[ ] relatório escrito usando template de artigo disponibilizado no moodle (em formato .PDF), com no mínimo 2 páginas de texto e com gráficos;

[ ] clareza do código-fonte da versão paralela (utilização de comentários e nomes de variáveis adequadas), que deverá ser entregue compactado juntamente com o artigo;

[ ] arquivo compactado no formato ZIP incluindo artigo em PDF, código-fonte e demais arquivos necessários à compilação e execução;

[ ] submissão através do moodle até o dia e o horário definidos na respectiva sala de entrega;

[ ] análise do número de horas máquina usadas pelo grupo no LAD durante o desenvolvimento do trabalho.

Código-fonte da versão sequencial (obtido a partir de http://www.archer.ac.uk/training/courses/2019/11/openmp_online/OpenMP-Online.tar)

#include <stdio.h>
#include <stdlib.h>
#include <math.h>
#include <omp.h>

# define NPOINTS 2000

# define MAXITER 2000

struct complex{
double real;
double imag;
};

int main(){
int numoutside = 0;
double area, error, ztemp;
double start, finish;
struct complex z, c;

/\*

-
-
-     Outer loops run over npoints, initialise z=c
-
-     Inner loop has the iteration z=z*z+c, and threshold test
  \*/

start = omp_get_wtime();

for (int i=0; i<NPOINTS; i++) {
for (int j=0; j<NPOINTS; j++) {
c.real = -2.0+2.5*(double)(i)/(double)(NPOINTS)+1.0e-7;
c.imag = 1.125*(double)(j)/(double)(NPOINTS)+1.0e-7;
z=c;
for (int iter=0; iter<MAXITER; iter++){
ztemp=(z.real*z.real)-(z.imag*z.imag)+c.real;
z.imag=z.real*z.imag*2+c.imag;
z.real=ztemp;
if ((z.real*z.real+z.imag*z.imag)>4.0e0) {
numoutside++;
break;
}
}
}
}

finish = omp_get_wtime();

/\*

- Calculate area and error and output the results
  \*/

       area=2.0*2.5*1.125*(double)(NPOINTS*NPOINTS-numoutside)/(double)(NPOINTS*NPOINTS);
       error=area/(double)NPOINTS;

       printf("Area of Mandlebrot set = %12.8f +/- %12.8f\n",area,error);
       printf("Time = %12.8f seconds\n",finish-start);

}
Referências

BULL, Mark. Shared Memory Programming with OpenMP - Exercise Notes. [S.l.]: ARCHER, 11 nov. 2019. Disponível em: <http://www.archer.ac.uk/training/courses/2019/11/openmp_online/OpenMP-Online-Exercises.pdf>. Acesso em 27 abr. 2022.

CONJUNTO DE MANDELBROT. In: WIKIPÉDIA, a enciclopédia livre. Flórida: Wikimedia Foundation, 2022. Disponível em: <https://pt.wikipedia.org/w/index.php?title=Conjunto_de_Mandelbrot&oldid=57754742>. Acesso em: 27 abr. 2022.
