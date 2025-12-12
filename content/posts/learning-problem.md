+++
title = 'O Problema da Aprendizagem'
date = 2025-11-22T21:08:10-03:00
draft = false
Tags = ["Learning Problem"]
Categories = ["Inteligencia Artificial"]
+++

## Prefacio
A proposta desta série de posts é apresentar, de forma clara e acessível, os fundamentos matemáticos que tornam o aprendizado de máquina (Machine Learning) possível. Fundamentos que muitas vezes ficam de fora dos cursos online ou aparecem apenas de maneira superficial.

Minha inspiração vem do trabalho de David F. Rogers e J. Alan Adams no livro Mathematical Elements for Computer Graphics: uma obra que tornou conceitos matemáticos complexos compreensíveis para programadores e estudantes. Quero fazer algo semelhante aqui, mas voltado para o aprendizado de máquina.

Todos os posts serão escritos em português (pt-BR). Embora exista bastante material em inglês espalhado pela internet, ainda há poucos recursos em português que combinem rigor, clareza e aprofundamento matemático.

No final de cada post, você encontrará uma seção de referências com todas as fontes utilizadas, para que possa consultar o material original e explorar exemplos adicionais. Os livros que servirão de base para esta série são principalmente:

- Learning from Data — Yaser S. Abu-Mostafa, Malik Magdon-Ismail e Hsuan-Tien Lin

- Pattern Recognition and Machine Learning — Christopher M. Bishop

- Applied Multivariate Statistical Analysis — Wolfgang Härdle e Leopold Simar

O objetivo é que esta série sirva como um guia para quem deseja compreender não apenas “como usar” técnicas de Machine Learning, mas por que elas funcionam.

## O Problema da Aprendizagem 

Considere o seguinte cenário: é sábado à noite e cada pessoa da sua casa quer algo diferente. Você quer pizza, seu irmão quer um açaí gigante e sua mãe quer comida japonesa.

Agora imagine conectar cada usuário ao restaurante ideal — mesmo quando são cozinhas diferentes, com preços diferentes, tempos de entrega diferentes e preferências que mudam o tempo todo. Quanto tempo o pedido deve levar? Qual restaurante é a melhor opção para cada pessoa? Esses são exatamente os desafios que a maior foodtech da América Latina, o iFood, resolve todos os dias.

Como mencionei no início, usuários e restaurantes têm características únicas — e ambas mudam constantemente. O “melhor” restaurante não é apenas o que serve a melhor comida. Ele também precisa ter o preço certo, o tempo de entrega adequado e, quem sabe, até uma promoção especial naquele momento.

O grande desafio é identificar, a cada instante, qual é a melhor oferta para aquele cliente, naquele contexto específico.
Para isso, o sistema de recomendação do iFood é estruturado em quatro pilares principais:

- Perfil do cliente: quanto ela pode pagar? Quão ativa é essa usuária? Que culinárias ela mais pede?
- Contexto: como o comportamento muda aos fins de semana? Em datas especiais? Em uma determinada localização?
- Jornada do cliente: se um usuário pediu pizza ontem, o que faz sentido recomendar hoje?
- Perfil dos pratos: quais tags definem cada prato? Prazer, saudabilidade, rapidez, valor calórico, risco de alergias, etc.

E é justamente aqui que entra o poder do learning from data. Em vez de programar manualmente todas essas regras — o que seria impossível — o sistema aprende sozinho, a partir dos dados de pedidos anteriores. Ele descobre padrões complexos que conectam cada cliente, cada prato e cada contexto, fazendo uma “engenharia reversa” das preferências reais das pessoas. Caso você queria saber mais sobre o sistema de recomendação do ifood, leia o post [[^ifood]] que foi escrito pela Julia Tessler no Medium.

### Componentes da Aprendizagem

O sistema de recomendação do iFood ilustra de forma clara o que significa aprender a partir de dados — assim como muitas outras aplicações em domínios totalmente distintos. Para compreender melhor os elementos comuns presentes em qualquer problema de aprendizado, utilizarei esse cenário ao longo da explicação. Ele servirá como metáfora para apresentar, de maneira intuitiva, cada componente essencial do processo de learning from data.

No livro Learning from Data, o professor Yaser Abu-Mostafa adota o exemplo da análise de solicitação de cartão de crédito para desempenhar esse mesmo papel. Caso você deseje conferir como ele usa essa metáfora, basta consultar a subseção 1.1.1 Components of Learning [[^Yaser]].

Antes de prosseguir, vale destacar as condições fundamentais que caracterizam um problema de aprendizado de máquina. Em linhas gerais, elas podem ser resumidas em três pontos:

- **Existencia um padrão**;

- Esse padrão não é diretamente acessível — **não há uma fórmula ou método matemático** fechado capaz de resolvê-lo;

- **Existem dados** a partir dos quais podemos tentar inferir esse padrão.

Se o problema que você está estudando não satisfaz essas três condições, então ele simplesmente não se enquadra como um problema de aprendizado de máquina

Bom, vamos agora formalizar matematicamente os componentes principais de um problema de aprendizado.
Primeiro, temos a **entrada** $x$ , que representa as informações disponíveis sobre um usuário naquele momento (por exemplo: horário do dia, histórico de pedidos, localização, categoria preferida etc.).
Existe também uma **função alvo** (*target function*) $f: X \rightarrow Y$. No contexto da metáfora do iFood, $f$ representa a **escolha ideal do usuário** — isto é, a função que saberia exatamente qual restaurante o usuário escolheria em cada situação.
O conjunto $X$ é o **espaço de entrada** (*input space*), contendo todas as possíveis combinações de características $X$.

O conjunto $Y$ é o **espaço de saída** (*output space*), que representa todas as recomendações possíveis (por exemplo, um restaurante ou um conjunto ranqueado de restaurantes). Também temos o **conjunto de dados** $D = {(x_n, y_n)}_{n=1}^N$, composto por pares entrada–saída, onde cada $y_n = f(x_n)$ é a escolha que o usuário realmente fez naquele contexto.
Por fim, temos o **algoritmo de aprendizado** (learning algorithm), cujo objetivo é encontrar uma função $g$ tal que $g : X \rightarrow Y$ seja uma boa aproximação de $f$.
O algoritmo não pode escolher *qualquer* função possível — ele deve escolher dentro de um conjunto limitado de candidatos, chamado **conjunto de hipóteses**, $H$. Assim como o iFood não mostra **todos** os restaurantes do mundo, mas apenas um subconjunto disponível e relevante, o algoritmo também escolhe sua hipótese apenas dentro do “cardápio” oferecido por $H$. A figura abaixo ilustra os compontes de aprendizagem.

<img src="/img/machine-learning/componentes de aprendizagem.png" class="center" />

# Referências
[^ifood]: [Recommendation at Ifood](https://medium.com/ifood-engineering/recommendation-ifood-88d60fa8bc6a)
[^Yaser]: [Learning From Data](https://amlbook.com/)





