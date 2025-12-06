+++
title = 'Introdução a Computação Gráfica'
date = 2025-11-22T21:41:15-03:00
Tags = []
Categories = ["Computação Gráfica"]
draft = false
+++

# Computação Gráfica

No futuro próximo quero transformar essa pagina de notas em um livro sobre introdução a computação gráfica. No final do texto tem um link para o arquivo que estou usando para dicas e referências.

# Rasterização de linhas

## Digital Differential Analyzer (DDA)

### Link para aula

[https://www.youtube.com/watch?v=x6zbuyA-0S4](https://www.youtube.com/watch?v=x6zbuyA-0S4)

### Conceito

Ele é um algoritmo usado para traçar linhas em uma grade de pixels em um sistema de exibição rasterizado, como uma tela de computador. Ele é amplamente utilizado em gráficos computacionais e é uma das técnicas mais simples e populares para renderizar linhas.

O algoritmo DDA é bastante simples e eficiente. Ele utiliza a diferença incremental entre as coordenadas dos pontos finais para determinar os incrementos de x e y necessários para percorrer a linha. Com base nesses incrementos, o algoritmo atualiza as coordenadas x e y a cada iteração, preenchendo os pontos intermediários ao longo da linha.

![Imagem 1 ](Untitled.png)

Imagem 1 

A seguir está uma descrição básica do algoritmo DDA:

1. Receba as coordenadas dos pontos iniciais $(x_0, y_0)$ e finais $(x_1, y_1)$.
2. Calcule as diferenças incrementais entre os pontos finais e iniciais: $dx = x_1 - x_0$ e $dy = y_1 - y_0$.
3. Calcule o número de iterações necessárias para percorrer a linha. Geralmente, é determinado pela maior diferença entre $dx$ e $dy$, ou seja, $steps = max(|dx|, |dy|)$.
4. Calcule os incrementos para x e y: $x_{inc} = dx / steps$ e $y_{inc} = dy / steps$.
5. Inicialize as coordenadas atuais: $x = x_0$ e $y = y_0$.
6. Repita o seguinte loop steps vezes:
    
    6.1. Desenhe o ponto (x, y) na tela ou faça qualquer operação desejada com ele.
    
    6.2. Atualize as coordenadas atuais: $x = x + x_{inc}$ e $y = y + y_{inc}$.
    

## Código

```c

void digital_differential_analyzer(vec2_t* start, vec2_t* end, uint32_t color) {

    int y = start->y;
    int delta_x = end->x - start->x;
    int delta_y = end->y - start->y;

    float m = delta_y / delta_x;

    draw_pixel(start->x, start->y, color);

    for (int x = start->x +1; x < end->x; x++) {
        y += m;
        draw_pixel(x, round(y), color);
    }

    draw_pixel(end->x, end->y, color);
}
```

O algoritmo DDA é simples e rápido de ser executado, pois evita a necessidade de cálculos complexos ou de usar funções trigonométricas. No entanto, ele pode apresentar algumas limitações, como a geração de linhas com espessura variável e o arredondamento impreciso de coordenadas, o que pode levar a problemas de aliasing e imprecisão visual em linhas diagonais muito longas. Apesar de suas limitações, o algoritmo DDA é um dos métodos mais básicos e amplamente utilizados para a renderização de linhas em gráficos por computador, e serve como base para outros algoritmos mais avançados e eficientes, como o algoritmo de Bresenham.

## Algoritmo do ponto médio (Bresenham)

### Link para aula

[https://www.youtube.com/watch?v=tygja6rr62M](https://www.youtube.com/watch?v=tygja6rr62M)

### Conceito

O algoritmo do ponto médio, também conhecido como algoritmo de Bresenham, é um algoritmo comumente utilizado na rasterização de linhas em computação gráfica. Ele é eficiente e preciso, permitindo o desenho de linhas com qualidade em um espaço discreto, como uma tela de computador.

O algoritmo do ponto médio utiliza o valor do "ponto médio" para determinar qual próximo pixel deve ser plotado ao desenhar uma linha. Com base nas coordenadas dos pontos finais da linha, o algoritmo calcula o ponto médio inicial e utiliza a informação do ponto médio para decidir qual próximo ponto deve ser desenhado.

![Imagem 2](Untitled%201.png)

Imagem 2

Aqui está uma descrição básica do algoritmo do ponto médio:

1. Receba as coordenadas dos pontos iniciais (x0, y0) e finais (x1, y1).
2. Calcule as diferenças incrementais entre os pontos finais e iniciais: dx = x1 - x0 e dy = y1 - y0.
3. Inicialize o valor do ponto médio como: d = 2 * dy - dx.
4. Inicialize as coordenadas atuais como: x = x0 e y = y0.
5. Repita o seguinte loop até que x atinja o valor de x1:
    
    5.1. Desenhe o ponto (x, y) na tela ou faça qualquer operação desejada com ele.
    
    5.2. Se d for positivo, atualize as coordenadas atuais: x = x + 1 e y = y + 1.
    
    5.3. Se d for negativo ou zero, atualize apenas a coordenada x: x = x + 1.
    
    5.4. Atualize o valor do ponto médio: d = d + 2 * dy - 2 * dx.
    

## Código

```c
void mid_point_line(vec2_t* start, vec2_t* end, uint32_t color) {
    int delta_x = end->x - start->x;
    int delta_y = end->y - start->y;
    int d = 2 * delta_y - delta_x;
    int inc_L = 2 * delta_y;
    int inc_NE = 2 * (delta_y - delta_x);

    int x = start->x;
    int y = start->y;
    draw_pixel(x, y, color);

    while(x < end->x) {
        if (d <= 0) {
            d += inc_L;
            x++;
        } else {
            d += inc_NE;
            x++;
            y++;
        }
        draw_pixel(x, y, color);
    }

}
```

O algoritmo do ponto médio é capaz de desenhar linhas com precisão, evitando o arredondamento impreciso de coordenadas que pode ocorrer em outros métodos. Ele realiza cálculos usando apenas operações de adição e subtração, sem a necessidade de multiplicação ou divisão, tornando-o eficiente em termos de desempenho.

Além disso, o algoritmo do ponto médio pode ser estendido para desenhar linhas com diferentes inclinações, lidar com diferentes casos de direção (para linhas descendentes ou ascendentes) e até mesmo ser adaptado para desenhar linhas com diferentes espessuras.

O algoritmo do ponto médio é amplamente utilizado na rasterização de linhas em computação gráfica, sendo a base para muitos outros algoritmos e técnicas mais avançados. Ele é particularmente adequado para desenhar linhas em dispositivos de exibição que possuem restrições de hardware e não suportam operações mais complexas.

# Pipeline gráfico

## Link para a aula

[https://www.youtube.com/watch?v=u3S-QUh6Pxc](https://www.youtube.com/watch?v=u3S-QUh6Pxc) 

## Conceito

O pipeline gráfico é uma sequência de etapas ou estágios que um sistema de renderização 3D, como uma placa de vídeo, percorre para transformar dados de geometria em pixels renderizados na tela. É um conceito fundamental na computação gráfica e descreve o fluxo de trabalho para processar e exibir imagens em tempo real.

1. Geometria e Transformações: Nesta etapa, a geometria dos objetos tridimensionais é definida, incluindo a posição dos vértices, as informações de textura, normais e outras propriedades. Em seguida, as transformações geométricas, como rotações, escalas e translações, são aplicadas para posicionar e orientar a geometria no espaço tridimensional. 
2. Clipping: Nesta etapa, ocorre o recorte (clipping) da geometria para determinar quais partes dela estão dentro do frustum de visualização (a porção do espaço 3D que é visível na tela). A geometria que está fora do frustum é descartada para otimizar o desempenho. 
    
    ![Untitled](Untitled%202.png)
    
3. Rasterização: Aqui, a geometria é convertida em primitivas, como triângulos, linhas ou pontos, que são os elementos básicos usados para renderizar imagens. A rasterização divide essas primitivas em fragmentos, que são pequenas áreas retangulares na tela. Cada fragmento é processado separadamente nas etapas subsequentes.
4. Teste de Profundidade e Culling: Nesta etapa, os fragmentos são testados em relação à profundidade para determinar quais estão mais próximos do observador e, portanto, devem ser renderizados. Além disso, ocorre o culling, que descarta fragmentos que estão voltados para fora da câmera ou fora do campo de visão, economizando tempo de processamento.
    
    ![Untitled](Untitled%203.png)
    
5. Shading: Aqui, ocorre o processo de determinar a cor final de cada fragmento com base em informações de iluminação e materiais. Isso envolve a aplicação de técnicas de iluminação, sombreamento, texturização e outras propriedades visuais para calcular a cor resultante.
6. Testes e Blending: Nesta etapa, são realizados testes adicionais, como o teste de transparência ou teste de stencil, para determinar como o fragmento se mistura com o que já está presente no framebuffer. Também é onde ocorre o blending, que combina a cor do fragmento com a cor existente no framebuffer para produzir o resultado final.
7. Saída para o Framebuffer: O resultado final, que é a cor de cada fragmento processado, é escrito no framebuffer. O framebuffer é a área de memória que representa a imagem exibida na tela.

# Transformações geométricas em 2D

São técnicas fundamentais na computação gráfica para manipular objetos e imagens em um espaço bidimensional. Essas transformações permitem mover, redimensionar, rotacionar e aplicar outros tipos de alterações em objetos gráficos. Algumas das transformações geométricas comuns em 2D são:

## Link para a aula

[https://www.youtube.com/watch?v=ZyPOxPdFdzA](https://www.youtube.com/watch?v=ZyPOxPdFdzA)

## Escala

A escala permite aumentar ou diminuir o tamanho de um objeto em relação a um ponto central. É possível escalar um objeto uniformemente em todas as direções ou separadamente ao longo dos eixos x e y. Por exemplo, para aumentar o tamanho de um objeto em 2 vezes, você multiplicaria as coordenadas x e y de cada ponto por 2. Existe, basicamente, dois tipos de escalas. Escala isotópica que possui valores iguais, tanto no eixo x quanto no eixo y e a escala anisotrópica que possui valores diferentes.

### Escala isotrópica

![Screenshot from 2023-06-30 18-45-18.png](Screenshot_from_2023-06-30_18-45-18.png)

### Escala anisotrópica

![Screenshot from 2023-06-30 18-52-16.png](Screenshot_from_2023-06-30_18-52-16.png)

### Espelhamento

É quando trocamos o valor do sinal de uma, ou ate mesmo das duas coordenadas, fazendo com que a imagem, naquele eixo, fique espelhada.

![Screenshot from 2023-06-30 18-56-17.png](Screenshot_from_2023-06-30_18-56-17.png)

## Rotação

A rotação envolve girar um objeto em torno de um ponto central. A rotação pode ser realizada em relação a um ângulo específico ou usando seno e cosseno para determinar as novas coordenadas de cada ponto.

![Screenshot from 2023-06-30 19-17-49.png](Screenshot_from_2023-06-30_19-17-49.png)

Na imagem acima, está mostrando como é feito o processo de rotação. Onde são aplicadas as coordenadas polares, onde a matriz é construída, que vai fazer essa transformação e retornar um novo par $(x,y)$ com as novas coordenadas $(x,y)$.

## Cisalhamento

 O cisalhamento é uma transformação que distorce um objeto ao inclinar sua forma em uma direção específica. Pode ser um cisalhamento horizontal (ao longo do eixo x) ou um cisalhamento vertical (ao longo do eixo y). Essa transformação envolve a adição ou subtração de uma proporção da coordenada y à coordenada x (no caso do cisalhamento horizontal) ou vice-versa.

### Cisalhamento no eixo X

![Screenshot from 2023-07-06 16-37-35.png](Screenshot_from_2023-07-06_16-37-35.png)

O processo acontece como mostrado na imagem. A coordenada $y$ permanece a mesma, mas a coordenada $x$ é deslocada ao longo do seu eixo. E para calcular o novo ponto $x^\prime$ é feito uma conta simples, onde ele é o x anterior mais uma contante de proporcionalidade($m_x$) vezes y. Lembrando, que isso é para o eixo x, mas também pode haver cisalhamento no eixo y e, também, nos dois ao mesmo tempo.

## Translação

 A translação envolve mover um objeto em uma direção específica, adicionando uma quantidade constante às suas coordenadas x e y. Por exemplo, se você deseja mover um objeto 2D para a direita em 50 unidades, você adicionaria 50 aos valores de x de todos os pontos do objeto. 

![Screenshot from 2023-07-06 17-15-56.png](Screenshot_from_2023-07-06_17-15-56.png)

Como mostrado a imagem acima, nos não conseguimos construir uma matriz genérica que posso se encaixa em todos os casos, já que dentro da matriz temos a presença tanto do x, quanto do y, que são coordenadas dos nossos vértices. Ou seja, para cada vértice que a figura possuir teremos que criar uma matriz diferente. Isso acontece porque a translação não é uma transformação linear, mas uma transformação afim. Para resolver esse problema temos as coordenadas homogêneas, que com elas nos conseguimos criar uma matriz de transformação.

# Coordenadas homogêneas

## Link para aula

[https://www.youtube.com/watch?v=6d70BXeS9iE](https://www.youtube.com/watch?v=6d70BXeS9iE)

Em um contexto bidimensional, as coordenadas homogêneas podem ser usadas para representar pontos e vetores em um espaço 2D de forma mais conveniente e flexível. Elas são representadas como um vetor com três componentes: (x, y, w)

Ou seja, nos passamos as coordenadas que estão em um espaço euclidiano $R²$ para o espaço projetivo, que nesse caso é $R³$. 

$$
(x,y) \rightarrow (xw,yw,w),\ w \ne 0
$$

Em coordenadas homogêneas 2D, a coordenada (x, y) representa a posição do ponto ou a direção do vetor no espaço 2D, enquanto a componente homogênea (w) é normalmente definida como 1 para pontos e 0 para vetores, mas pode ser qualquer valor, como mostrado na imagem abaixo.

![Screenshot from 2023-07-06 18-16-43.png](Screenshot_from_2023-07-06_18-16-43.png)

Obs: Um ponto no nosso espaço euclidiano corresponde a uma reta no espaço projetivo que passa pela origem.

Para voltar ao nosso espaço euclidiano, é só dividir as novas coordenadas pela componente($w$), que é a coordenada homogênea e, por fim, descartá-la, como posta na figura.

![Screenshot from 2023-07-06 18-28-11.png](Screenshot_from_2023-07-06_18-28-11.png)

## Translação 2D usando Coordenadas Homogêneas

![Screenshot from 2023-07-06 19-24-43.png](Screenshot_from_2023-07-06_19-24-43.png)

Como mostrado na imagem acima, depois que passamos para o espaço projetivo nós conseguimos criar a matriz de translação corretamente. Além disso, se você perceber bem,  a matriz de translação nada mais é que uma matriz de cisalhamento de uma espaço 3D, ou seja, a translação 2D é um cisalhamento do espaço 3D. Por exemplo, se você translada um objeto no eixo x, o que vai acontecer, no espaço projetivo, é um cisalhamento no eixo x com relação ao eixo w, e o mesmo acontece com a translação 2D no eixo y. Um exemplo pode ser visto na aula 9 e no minuto 29:00.

## Transformações no Espaço Projetivo

![Screenshot from 2023-07-22 20-54-14.png](Screenshot_from_2023-07-22_20-54-14.png)

## Aplicações das coordenadas homogêneas

- Unificação do tratamento das transformações geométricas
- Projeção perspectiva
- Técnicas de rendering
- etc

# Transformações geométricas em 3D

## Link

[https://www.youtube.com/watch?v=r-8yxvaeqfI](https://www.youtube.com/watch?v=r-8yxvaeqfI)

## Translação

Bom, o calculo é praticamente o mesmo no que a gente tem no espaço 2D, a única diferença, agora, é que temos uma coordenada(eixo $z$) a mais para fazer o deslocamento. Como a gente pode ver na imagem abaixo.

![Screenshot from 2023-09-09 18-25-12.png](Screenshot_from_2023-09-09_18-25-12.png)

## Escala

Assim como na translação, o calculo da escala em 3D é praticamente igual no 2D, só o acréscimo do eixo $z$ na matriz. No exemplo da imagem abaixo, a matriz está no espaço projetivo.

![Screenshot from 2023-09-09 18-31-00.png](Screenshot_from_2023-09-09_18-31-00.png)

## Cisalhamento

Agora, o cisalhamento 3D fica um pouco diferente do 2D. Como no 2D a gente tem penas 2 dimensões(por isso 2D 🙃), o cisalhamento do eixo x fica em função do eixo y, e vice e versa. Entretanto, no espaço 3D, por termos 3 coordenadas, temos que x em função de y e z; y em função de x e z; z em função de x e y. Por isso, tempos 4 variáveis na matriz de transformação, como mostra a figura abaixo. 

![Screenshot from 2023-09-09 18-36-56.png](Screenshot_from_2023-09-09_18-36-56.png)

Isso ocorre em um cisalhamento geral, em todas as coordenadas. Mas podemos aplicar a transformação em apenas um eixo, fazendo, assim, a matriz de transformação mais simples, como mostra a imagem abaixo.

![Screenshot from 2023-09-09 19-04-15.png](Screenshot_from_2023-09-09_19-04-15.png)

## Rotação

Bom, as nossas rotações vão ser representadas por matrizes, como vem sendo todas as nossas transformações, e cada matriz vai representar uma rotação em torno de cada eixo. Cada seção abaixo vai mostrar uma imagem de como fica cada matriz de rotação e seu efeito no objeto.

### Rotação no eixo x

![Screenshot from 2023-09-09 19-22-24.png](Screenshot_from_2023-09-09_19-22-24.png)

### Rotação no eixo y

![Screenshot from 2023-09-09 19-23-11.png](Screenshot_from_2023-09-09_19-23-11.png)

### Rotação no eixo z

![Screenshot from 2023-09-09 19-23-46.png](Screenshot_from_2023-09-09_19-23-46.png)

Como a gente pode ver as matrizes de transformação nas imagens, o eixo que ocorre a rotação ele não é alterado

### Características desse sistema de rotação

- Cada rotação é realizada em torno de uma eixo(fixo) do espaço;
- Cada rotação é representada por 9 números, ou seja, acaba gastando mais memoria do que se fossem armazenadas como ângulos de Euler, que seria apenas 3 números;
- Difícil de interpretação para pessoas “normais”, que não são da área, mas com os ângulos de Euler fica mais fácil de ser interpretado;
- Sujeito a [Gimbal Luck](https://pt.wikipedia.org/wiki/Gimbal_lock): Esse fenômeno ocorre quando a gente aplica varias rotações no objeto e acaba deixando dois dos eixos em paralelo, travando-o , com isso, tirando um dos grau de liberdade do objeto. Solução: [Quarternos](https://pt.wikipedia.org/wiki/Quaterni%C3%A3o).

## Transformações Inversas

Bom, as transformações inversas são transformações que revertam o efeito de uma transformação original, restaurando o objeto à sua forma original. Para cada transformação geométrica, há uma transformação inversa correspondente que desfaz o efeito da transformação original.

## Translação

![Screenshot from 2023-11-09 17-23-52.png](Screenshot_from_2023-11-09_17-23-52.png)

## Escala

![Screenshot from 2023-11-09 17-28-43.png](Screenshot_from_2023-11-09_17-28-43.png)

## Rotação

![Screenshot from 2023-11-09 17-52-41.png](Screenshot_from_2023-11-09_17-52-41.png)

Como a gente pode ver na imagem, há dois métodos, que no final tem o mesmo resultado. O primeiro é quanto você inverte o sinal do angulo $\theta$. Ou seja, se eu fiz uma rotação $\theta$ e quiser voltar para a posição inicial é só fazer $-\theta$. Já no segundo método, ele trabalha com um teorema da geometria, que diz que a matriz inversa de uma matriz ortonormal é igual a sua transposta.

## Rotação em torno de pontos arbitrários

O grande problema que temos é que, se o nosso objeto não estiver na origem ele não vai rotacionar no próprio  eixo, como mostrado no exemplo da esquerda na imagem abaixo. A pergunta que fica é, como a gente faz com que o objeto, estão em qualquer ligar no espaço consiga rotacionar no próprio eixo, como mostrado no exemplo da direita na imagem abaixo. 

![Screenshot from 2023-11-09 18-01-05.png](Screenshot_from_2023-11-09_18-01-05.png)

Para resolver esse problema nos fazemos o seguinte. Primeiro, pegamos o objeto e transladamos ele para a origem, ou seja coordenadas (0,0), aplicando $T(d_x, d_y)$. Depois aplicamos a rotação desejada $R(\theta)$. E,  por fim, voltamos ele para a posição em que ele foi encontrado, antes dessas transformações aplicando $T(-d_x, -d_y)$. Então, nossa matriz de transformação final é:

$$
M = T(-d_x,-d_y) \cdot R(\theta) \cdot T(d_x,d_y) 
$$

![Screenshot from 2023-11-09 18-13-56.png](Screenshot_from_2023-11-09_18-13-56.png)

## Escala em ponto arbitrário

Bom, como foi visto na rotação o processo é basicamente o mesmo. Ou seja, consiste em a gente transladar o objeto para origem e depois aplicar a escalar e, por fim ,voltar ele para a sua posição inicial. A imagem abaixo mostra como esse processo é feito:

![Screenshot from 2023-11-20 09-36-21.png](Screenshot_from_2023-11-20_09-36-21.png)

## Composição de transformações

Nesta parte da aula, o professor nos mostra duas abordagens de aplicação das transformações e o ganhos com relação a performance. Na primeira consiste em ir aplicando as transformações, da direita para esquerda, uma por uma, ai, no final, teremos o objeto transformado. Já, na segunda abordagem, ele mostra o processo de juntar todas as transformações e uma única matriz. A imagem abaixo nos mostra a quantidade de operação de cada uma dessa duas abordagens:

![Screenshot from 2023-11-20 09-52-02.png](Screenshot_from_2023-11-20_09-52-02.png)

Como podemos ver, a primeira abordagem parece ser mais eficiente que a segunda, mas isso é so para um caso isolado. Por exemplo, na imagens estamos querendo aplicar transformações em um vértice hipotético, mas se agora tivermos 10 mil vértices. Como ficaria esse custo de operação de cada uma das abordagens? A imagens abaixo nos ilustra a resposta:

![Screenshot from 2023-11-20 09-54-15.png](Screenshot_from_2023-11-20_09-54-15.png)

# Matriz de modelagem

link: [https://www.youtube.com/watch?v=n3znz0KjGRA](https://www.youtube.com/watch?v=n3znz0KjGRA) 

Ela faz parte do processo de transformação de um objeto desde suas coordenadas locais (espaço do objeto) até o sistema de coordenadas do mundo (espaço do universo). Ou seja, é uma matriz que possui todas as transformações necessária para fazer essa transição.  

![Screenshot from 2023-11-21 10-08-38.png](Screenshot_from_2023-11-21_10-08-38.png)

## Vetores Normais

No nosso contexto de computação gráfica, é um vetor que possui ângulo de 90 graus, ou seja, é perpendicular, com um triângulo e, geralmente, eles possuem comprimento igual á 1. Além disso, eles descrevem a orientação dos triângulos e com base nisso conseguimos descrever a iluminação dos triângulos.

![Screenshot from 2023-11-21 10-21-24.png](Screenshot_from_2023-11-21_10-21-24.png)

A imagem acima mostra como a gente consegue calcular os vetores normais de um triângulo. Bom, e por que os vetores normais são importantes no nosso contexto de matriz de modelagem? Porque quando aplicamos uma transformação em um objeto, seja ela uma rotação, por exemplo, nos mudamos as coordenadas desse objeto, e com isso, mudamos  a posição dos nosso vetores. Assim sendo, eu não posso esquecer de atualizar os vetores de cada um dos meus triângulos

## Transformando vetores normais

![Screenshot from 2023-11-21 10-28-42.png](Screenshot_from_2023-11-21_10-28-42.png)

Há transformações que alteram os vetores e outras que não, como a gente pode ver na imagem acima. 

![Screenshot from 2023-11-21 10-34-00.png](Screenshot_from_2023-11-21_10-34-00.png)

E, na imagem acima, mostra como é deduzido a matriz de transformação usada nos vetores normais. No final, a gente vai pegar a matriz de modelagem M, calcular a sua inversa e depois a sua transposta, para, então, aplicar ela no nosso vetor normal:  $\vec{n_t} = (M^{-1})^T\vec{n}$

# Mudança de base

link: [https://www.youtube.com/watch?v=o1MGjVcDzvo](https://www.youtube.com/watch?v=o1MGjVcDzvo)

## O que são bases?

Aqui, no conceito da computação gráfica, a gente vai pegar a definição da álgebra linear. Em álgebra linear, uma base é um conjunto de vetores que é linearmente independente e que pode gerar (por meio de combinações lineares) todo o espaço vetorial ao qual esses vetores pertencem. Se $v_1, v_2, \dots, v_n$ é uma base para um espaço vetorial $V$, então qualquer vetor $v \in V$ pode ser expresso de forma única como uma combinação linear dos vetores da base:

$$
v = c_1 v_1 + c_2 v_2 + \ldots + c_n v_n
$$

Aqui, $c_1, c_2, \ldots, c_n$ são coeficientes escalares.

## Bases Ortonormais

São um tipo especial de bases em álgebra linear em que os vetores são unitários (ou seja, têm comprimento 1) e são mutuamente perpendiculares. Isso significa que, se $v_i$ e $v_j$ são dois vetores diferentes na base, então o produto interno $\langle v_i, v_j \rangle$ é zero, indicando que eles são perpendiculares, e o comprimento de cada vetor $\|\mathbf{v}_i\| = 1$.

Formalmente, uma base $\{\mathbf{v}_1, \mathbf{v}_2, \ldots, \mathbf{v}_n\}$ em um espaço vetorial com um produto interno é chamada de ortonormal se:

1. **Norma Unitária:**
$\|\mathbf{v}_i\| = 1 \quad \forall \ i = 1, 2, \ldots, n$ 
2. **Mutuamente Perpendiculares:**
$\langle \mathbf{v}_i, \mathbf{v}_j \rangle = 0 \quad \forall i \neq j$
3. **Base:**
Os vetores $\mathbf{v}_1, \mathbf{v}_2, \ldots, \mathbf{v}_n$ geram todo o espaço vetorial.

Bases ortonormais são especialmente úteis em várias aplicações, incluindo transformações lineares, decomposições espectrais e resolução de sistemas lineares. Muitas vezes, é mais fácil trabalhar com bases ortonormais em cálculos e simplificações, pois os produtos internos envolvendo esses vetores têm propriedades convenientes.

![Screenshot from 2023-12-01 18-46-09.png](Screenshot_from_2023-12-01_18-46-09.png)

## Mesma origem

As imagens abaixo mostram como é o processo, a lógica matemática usada na mudança de bases onde o vetor possui a mesma origem. A gente vai mudar o vetor $\vec{w
}$, que esta na base verde(base ortonormal) para a base azul, que é ortogonal.

![Screenshot from 2023-12-01 19-18-23.png](Screenshot_from_2023-12-01_19-18-23.png)

Como a gente pode ver na imagem acima, conseguimos rescrever o vetor $\vec{w}$ sendo igual ao vetor $\vec{a}$(que é o vetor com os escalares).

![Screenshot from 2023-12-01 20-20-32.png](Screenshot_from_2023-12-01_20-20-32.png)

Nessa imagem a gente consegue ver como pegar o vetor $\vec{b}$, que é o vetor de escalar da base azul. Ou seja, tudo que a gente precisa é saber essas coordenadas e nos teremos o nosso vetor $\vec{w}$ na matriz azul. E, como é mostrado na imagem acima, $\vec{b} = B^{⁻1}\vec{a}$, mas calcular a inversa de uma matriz é muito custoso. Então, segundo um teorema matemático, a inversa de uma matriz ortogonal é a sua transposta. Portanto, a gente consegue substituir essa inversa por uma transposta e a expressão final fica assim: $\vec{b} = B^{T}\vec{a}$

## Origens diferentes

Se as duas bases estão em origens diferentes, como é mostrado na imagem abaixo, o que a gente pode fazer é trazer a origem da base azul de encontro com a base verde. Agora que as duas estão na mesma origem nós conseguimos aplicar o processo que foi apresentado acima. Mudança de bases de mesma origem.

![Screenshot from 2023-12-02 14-21-15.png](Screenshot_from_2023-12-02_14-21-15.png)

Para ficar mais simples ainda, se a gente traçar um vetor($\vec{t}$) que vai da origem da base verde($o_1$) para a origem da base azul($o_2$) nós conseguimos trazer o vetor $\vec{a}$ tirando essa diferença que é mostrada pelo vetor $\vec{t}$. Ou seja, transladando o vetor $\vec{a}$ para a base azul.

A imagem abaixo mostra o passo a passo que a gente pode seguir caso as origens da base forem diferentes, como na maioria dos casos vão ser.

![Screenshot from 2023-12-02 14-22-42.png](Screenshot_from_2023-12-02_14-22-42.png)

Assim, como foi dito na aula, esse primeiro passo, nada mais é que uma transformação de translação, ou seja, a gente pode reescrever essa equação como uma matriz de translação. E  essa segundo passo é uma multiplicação matriz vetor.

![Screenshot from 2023-12-02 14-43-14.png](Screenshot_from_2023-12-02_14-43-14.png)

A gente pode reescrever esses dois passos em uma única equação que, no final, fique uma matriz de mudança multiplicado por um vetor. Como a gente pode ver na imagem acima, a matriz de base($M_{base}$) é a igual a matriz transposta($B^T$) da nossa base de destino multiplicado pela matriz de translação($T$).

# Matriz View

link: [https://www.youtube.com/watch?v=QqckGHpHR2U](https://www.youtube.com/watch?v=QqckGHpHR2U)

![Screenshot from 2023-12-13 10-54-46.png](Screenshot_from_2023-12-13_10-54-46.png)

Bom, antes de começa a falar realmente sobre a matriz view, temos que falar sobre o tipo de sistema de coordenadas que o nosso programa vai usar. Como podemos ver na imagem acima, existe praticamente dois tipos de sistemas, o da mão esquerda e o da mão direita. Não exite um sistema melhor que o outro para se usar no programa, isso vai mais do gosto do programador mesmo. Mas, na maioria dos software, como o OpenGL, por exemplo, usa-se o sistema da mão direita, então, assim como o professor eu vou adotá-lo também.

![Screenshot from 2023-12-13 11-09-20.png](Screenshot_from_2023-12-13_11-09-20.png)

Como a gente pode ver na imagem acima, para projetar um objeto que está no espaço do universo no espaço da câmera basta fazer uma mudança de base. Agora, antes de passarmos os vértices de um universo para o outro, temos que definir como sera nossa câmera.

## Definição da Câmera

Primeiro de tudo, nossa câmera tera uma posição(ou seja um ponto) no espaço do universo. Ela vai possuir um vetor direção, um vetor up($\vec{u}$, que nos diz qual é a parte de cima da câmera). E, na maior parte das vezes, o vetor up vai estar paralelo ao eixo y. Quando isso não vai acontecer? Quando quisermos que a câmera fique invertida, ou seja, o nosso vetor vai estar apontando para baixo, para o eixo -y por exemplo. 

![Screenshot from 2023-12-29 17-03-47.png](Screenshot_from_2023-12-29_17-03-47.png)

Agora, como construímos a nossa base ortonormal da nossa câmera? Primeiro a gente vai criar o vetor $\vec{z_{cam}}$, que é o nosso vetor direção normalizado e com sentido contrario. Depois, vamos precisar de um vetor que seja ortogonal com $\vec{z_{cam}}$ para formarmos uma base. E nós não podemos usar o vetor direção e nem o vetor up, já que os tres são coplanares, ou seja, estão no mesmo plano. Então, temos que fazer um produto vetorial do vetor $\vec{z_{cam}}$ com o vetor up e com isso geramos o vetor $\vec{x_{cam}}$, depois a gente normaliza o vetor para que ele tenha comprimento igual a 1. E, por fim, calculamos o nosso $\vec{y_{cam}}$ fazendo o produto vetorial entre $\vec{z_{cam}}$ e $\vec{x_{cam}}$. Como mostrado na imagem abaixo.

![Screenshot from 2023-12-29 17-18-57.png](Screenshot_from_2023-12-29_17-18-57.png)

Depois de construir a nossa base da câmera, nos conseguimos criar, finalmente, a nossa matriz view que vai transformar os pontos do espaço do universo para o espaço da câmera. Nós fazemos essa mudança de base como mostra a imagem abaixo:

![Screenshot from 2023-12-29 17-28-16.png](Screenshot_from_2023-12-29_17-28-16.png)

# Matriz de Projeção

link: [https://www.youtube.com/watch?v=f3S77wAvSdY](https://www.youtube.com/watch?v=f3S77wAvSdY)

Vamos relembrar onde aplicamos a matriz de projeção no nosso pipeline gráfico. Se vocês puxarem da memoria o pipeline se estrutura assim:  

Aplicação → Esp. objeto → Esp. universo → Esp. câmera → Esp. recorte → Esp. canônico → Tela

Ou seja, todos esses processos são executados para cada frame da nossa aplicação. E, a matriz de projeção, fica entre o espaço da câmera e espaço do recorte e a homogenização, que também é abordada nessa aula, entra antes do espaço canônico. 

## Distorção da perspectiva

 Aqui o nosso objetivo é criar uma modelo matemático que consiga simular esse efeito de distorção, maior é objeto que estive próximo da câmera e menor quando mais distante.

![Screenshot from 2024-01-12 16-03-38.png](Screenshot_from_2024-01-12_16-03-38.png)

Como mostrado na imagem acima, nos vamos precisar de um centro de projeção, que chamamos de $c$, e queremos saber a posição de um dado ponto p depois de aplicado a distorção. Em primeiro momento vamos dizer que o eixo $z$ projetado($z'$) é zero, pois ele está sendo projetado em cima do nosso plano de visualização. Portando, só nos resta saber as coordenadas $x$ e $y$ desse ponto. E como conseguimos isso? A resposta esta na imagem acima, com semelhança de triângulos. O ponto $p$ esta a uma distancia $z$ do centro de projeção e possui uma altura $y$, já o ponto $p'$ está a uma distancia $d$ do centro de projeção e possui uma altura $y'$. Com tudo isso dito, nos temos os nosso triângulos e conseguimos calcular as coordenadas $x'$ e $y'$ de $p'$.

![Screenshot from 2024-01-12 16-32-30.png](Screenshot_from_2024-01-12_16-32-30.png)

Um exemplo de como fica o objeto depois de aplicado a distorção de perspectiva e, depois, levado para para o espaço canônico. 

## Coordenada Z

- $z' = 0$ perde a informação de profundidade que é necesaria para determinar a oclusão
- $z' = z$ perde a uniformidade dos cálculos quanto temos que usar matrizes

Solução seria fazer o calculo do $z'$ igual ao calculo do $x'$ e do $y'$:

$$
z' =\frac{z}{1-\frac{z}{d}}
$$

![Screenshot from 2024-01-12 17-25-15.png](Screenshot_from_2024-01-12_17-25-15.png)

## Espaço de Recorte(Clipping space)

É onde a gente consegue saber quais objetos estão no nosso campo de visão(Frustum), quais objetos serão renderizados. E os sistemas de renderização sabem quais objetos serão renderizados com base na sua coordenada homogenia. Na imagem abaixo mostra como é feito a transformação do Esp. câmera → Esp. recorte → Esp. canônico.

![Screenshot from 2024-01-12 17-53-48.png](Screenshot_from_2024-01-12_17-53-48.png)

### Matriz de correção “corrigida”

Como nossa câmera está mais próxima da cena e o nosso centro de projeção está longe isso vai resultar em uma distorção de perspectiva incorreta. Temos duas alternativas para resolver o problema. A primeira seria mover $d$ (distância $d$) o centro de projeção para a frente da câmera. Já, na segunda opção, seria mover a cena $d$ unidades para perto da câmera e com isso manter o centro de projeção no lugar. A imagem abaixo mostra como fica a matriz de projeção final. 

![Screenshot from 2024-01-12 18-29-12.png](Screenshot_from_2024-01-12_18-29-12.png)

# Matriz Viewport

link: [https://www.youtube.com/watch?v=mAEtClnLmqc](https://www.youtube.com/watch?v=mAEtClnLmqc)

Resumidamente, ela é responsável por trazer o cena que esta no espaço canônico para a nossa tela. A imagem abaixo ilustra bem isso.

![Screenshot from 2024-01-12 19-06-04.png](Screenshot_from_2024-01-12_19-06-04.png)

Agora, como fazemos essa transformação? Bom, como os pixels da nossa tela só podem ter valores positivos tempos que da uma jeito de jogar a nossa cena para um espaço que possua valores positivos. Como o espaço canônico vai do (-1,-1) até (1,1) podemos transladar a nossa cena para o quadrante (0,0) até (1,1) e, agora com os valores positivos, podemos aplicar uma transformação de escala para que a cena atenda as dimensões de altura e largura da tela. A imagem abaixo mostra como a gente consegue fazer a matriz que faz todas as transformações. 

![Screenshot from 2024-01-12 19-10-53.png](Screenshot_from_2024-01-12_19-10-53.png)

# Determinando Visibilidade

## Algoritmo do pintor

link: [https://www.youtube.com/watch?v=45uFQgzenPg](https://www.youtube.com/watch?v=45uFQgzenPg)

### Oclusão

Em computação gráfica, oclusão refere-se ao fenômeno de um objeto sendo bloqueado, total ou parcialmente, por outro objeto em relação à visão do observador. Em outras palavras, a oclusão ocorre quando um objeto está na frente de outro, causando, assim,  a "sombra" ou bloqueio parcial do objeto que está atrás. 

![Screenshot from 2024-01-25 16-34-48.png](Screenshot_from_2024-01-25_16-34-48.png)

Para resolver esse problema podemos inverter a ordem dos objetos que vão ser renderizados, ou seja, renderizando os mais distantes e depois os mais próximos. Ai que entra o algoritmo do pintor.

### Como funciona

Bom, como foi comentado acima, a estrategia é clara - começar a renderização do objeto mais distante até o mais próximo da câmera - E para fazer isso temos que ordenar esses objetos com base na sua distancia. Portanto, vamos precisar de um algoritmo de sort e o mais rápido que temos é $O(nlogn)$. Consequentemente, se a câmera mudar ou se a cena for modificada, teremos que reordenar os objetos. Agora, para calcular quem esta próximo ou longe da câmera temos duas opções. Ou pegamos a coordena z de cada objeto e vemos que esta mais distante, ou calculamos o centroide para cada objeto usando as suas coordenadas e vemos qual o centroide mais distante.

### Problemas

![Screenshot from 2024-01-25 17-03-28.png](Screenshot_from_2024-01-25_17-03-28.png)

### Quando funciona

![Screenshot from 2024-01-25 17-04-41.png](Screenshot_from_2024-01-25_17-04-41.png)

![Screenshot from 2024-01-25 17-08-29.png](Screenshot_from_2024-01-25_17-08-29.png)

## Algoritmo do z-buff

link: [https://www.youtube.com/watch?v=7XfHp1_I40Q](https://www.youtube.com/watch?v=7XfHp1_I40Q)

## **Backface culling**

link: [https://www.youtube.com/watch?v=G1sTpDeUaYk](https://www.youtube.com/watch?v=G1sTpDeUaYk)

# Introdução ao OpenGL

[OpenGL](https://www.notion.so/OpenGL-0669736821cc4a3b8da12fa2a985be18?pvs=21)

# Links para as aulas

1. [https://www.youtube.com/watch?v=WX5XPqEU8M4](https://www.youtube.com/watch?v=WX5XPqEU8M4)
2. [https://www.youtube.com/watch?v=fgYLj4l0T1k](https://www.youtube.com/watch?v=fgYLj4l0T1k) 
3. [https://www.youtube.com/watch?v=L8uuPuNUXfo](https://www.youtube.com/watch?v=L8uuPuNUXfo) 
4. [https://www.youtube.com/watch?v=RtTioDoD-ZA](https://www.youtube.com/watch?v=RtTioDoD-ZA) 
5. [https://www.youtube.com/watch?v=x6zbuyA-0S4](https://www.youtube.com/watch?v=x6zbuyA-0S4) 
6. [https://www.youtube.com/watch?v=tygja6rr62M](https://www.youtube.com/watch?v=tygja6rr62M) 
7. [https://www.youtube.com/watch?v=u3S-QUh6Pxc](https://www.youtube.com/watch?v=u3S-QUh6Pxc) 
8. [https://www.youtube.com/watch?v=ZyPOxPdFdzA](https://www.youtube.com/watch?v=ZyPOxPdFdzA) 
9. [https://www.youtube.com/watch?v=6d70BXeS9iE](https://www.youtube.com/watch?v=6d70BXeS9iE) 
10. [https://www.youtube.com/watch?v=r-8yxvaeqfI](https://www.youtube.com/watch?v=r-8yxvaeqfI) 
11. [https://www.youtube.com/watch?v=n3znz0KjGRA](https://www.youtube.com/watch?v=n3znz0KjGRA) 
12. [https://www.youtube.com/watch?v=o1MGjVcDzvo](https://www.youtube.com/watch?v=o1MGjVcDzvo)  
13. [https://www.youtube.com/watch?v=QqckGHpHR2U](https://www.youtube.com/watch?v=QqckGHpHR2U)  
14. [https://www.youtube.com/watch?v=f3S77wAvSdY](https://www.youtube.com/watch?v=f3S77wAvSdY) 
15. [https://www.youtube.com/watch?v=mAEtClnLmqc](https://www.youtube.com/watch?v=mAEtClnLmqc) 
16. [https://www.youtube.com/watch?v=45uFQgzenPg](https://www.youtube.com/watch?v=45uFQgzenPg) 
17. [https://www.youtube.com/watch?v=7XfHp1_I40Q](https://www.youtube.com/watch?v=7XfHp1_I40Q) 
18. [https://www.youtube.com/watch?v=G1sTpDeUaYk](https://www.youtube.com/watch?v=G1sTpDeUaYk) 
19. [https://www.youtube.com/watch?v=O22n2JiH55c](https://www.youtube.com/watch?v=O22n2JiH55c) 
20. [https://www.youtube.com/watch?v=W_n34BKvBO8](https://www.youtube.com/watch?v=W_n34BKvBO8) 
21. [https://www.youtube.com/watch?v=xUy1toRAqh8](https://www.youtube.com/watch?v=xUy1toRAqh8) 
22. [https://www.youtube.com/watch?v=bg_QN8gsIhg](https://www.youtube.com/watch?v=bg_QN8gsIhg) 
23. [https://www.youtube.com/watch?v=AYu6niUu8ug](https://www.youtube.com/watch?v=AYu6niUu8ug) 
24. [https://www.youtube.com/watch?v=LCYpsgvJU24](https://www.youtube.com/watch?v=LCYpsgvJU24)
25. [https://www.youtube.com/watch?v=P2LTAUO1TdA](https://www.youtube.com/watch?v=P2LTAUO1TdA) 

## Link para álgebra

[https://www.youtube.com/playlist?list=PLZHQObOWTQDPD3MizzM2xVFitgF8hE_ab](https://www.youtube.com/playlist?list=PLZHQObOWTQDPD3MizzM2xVFitgF8hE_ab) 

## Link para o canal two minute papers

[https://www.youtube.com/c/K%C3%A1rolyZsolnai](https://www.youtube.com/c/K%C3%A1rolyZsolnai)  

# Livro de Introdução à Computação Gráfica

# Referências

OpenGL SuperBible: [http://cosmic-rays.ru/books62/2015Graham.pdf](http://cosmic-rays.ru/books62/2015Graham.pdf)

[https://en.wikipedia.org/wiki/Graphics_pipeline](https://en.wikipedia.org/wiki/Graphics_pipeline)

[https://pt.wikipedia.org/wiki/Gimbal_lock](https://pt.wikipedia.org/wiki/Gimbal_lock)

imagem 1: [https://graficaciong2equipo04.files.wordpress.com/2018/02/575e3-dda.jpg?w=640](https://graficaciong2equipo04.files.wordpress.com/2018/02/575e3-dda.jpg?w=640)

imagem 2: [https://media.geeksforgeeks.org/wp-content/uploads/BresenhamLine.png](https://media.geeksforgeeks.org/wp-content/uploads/BresenhamLine.png)

[Scratchapixel 4.0, Learn Computer Graphics Programming](https://www.scratchapixel.com/index.html)

https://math.hws.edu/eck/cs424/downloads/graphicsbook-linked.pdf

https://learnopengl.com/PBR/Theory
