---
title: "Otimização de código usando fórmula de juro composto"
date: 2017-07-02T05:00:00Z
cover:
    image: "images/digital-marketing.jpg"
    alt: "Imagem de capa de um table com gráficos."
    relative: true
description: "Resolução de um problema proposto na plataforma repl.it que usa a fórmula de juros compostos ao invés de usar loop."
summary: "Neste artigo mostro como foi otimizado um algoritmo que usava loop para efetuar um cálculo com performance linear, para um algoritmo com performance constante usando a fórmula do juro composto."
categorias:
  - Programação
tags:
  - Python
math: true
---


## TL;DR

Foi aplicada a fórmula do juro composto ($V_f = V_p(1 + j) ^ n$) para resolver o problema sem usar laços de repetição, apenas matemática.

Problema:

> Você consegue correr `X` milhas por dia, você quer correr `Y` por dia. Em quantos dias você conseguirá correr `Y` milhas por dia sendo que seu desempenho melhor 10% ao dia?

Código anterior:

```python
x, y = int(input()), int(input())
num_days = 1

while x < y:
    x *= 1.1
    num_days += 1

print(num_days)
```

Novo código:

```python
from math import log, ceil

v_p, v_f = int(input()), int(input())
n = log(v_f/v_p, 1.1)

print(ceil(n) + 1)
```

---

## Introdução

Sempre que preciso enviar um código HTML, Javascript ou CSS de exemplo para alguém (seja para ajudar ou só trocar uma ideia mesmo) utilizo o [CodePen] ou o [JSFiddle], mas quando o assunto é python eu utilizava os [Gists] do GitHub. Há algumas semanas conheci o [Repl.it], um serviço cuja função se assemelha aos citados anteriormente, um interpretador online para várias linguagens ([várias mesmo](https://repl.it/languages)), sendo o Python 3 uma delas.

Até aí OK, porém semana passada descobri que o Repl.it tem uma plataforma de professor/aluno onde eu posso criar cursos com N exercícios onde eu crio testes para estes exercícios, e estes teste definem se a resposta do aluno está correta. Ainda não criei nenhum curso, e talvez não crie, mas vi que pode-se corrigir manualmente cada exercício dado aos alunos.

O fato é que, por mais que eu não assuma o papel de professor, decidi assumir o papel de aluno. Afinal a plataforma tem um seção "[Comunidade][replit.comunidade]" que apresenta todos os cursos criados pela comunidade e eu posso me inscrever em qualquer um deles.

Dito isso, me inscrevi no curso de Python 3 com um objetivo em mente, treinar minha lógica e boas práticas resolvendo os exercícios de maneira que eu use bem os recursos do Python 3. Exemplo: por mais que o exercício peça que eu use um `while` para fazer um fatorial, eu sei da existência da função [`math.factorial`][docs.factorial] e em um projeto real esta seria a melhor opção.

Um desses exercícios me chamou a atenção, e este é o motivo deste post. Vamos ao que interessa!


## O problema

_Problema **[6.4. While: Jogging][replit.exercicio]** do curso **[Auto-Graded Course with Solutions][replit.curso]**_

> Como um futuro atleta, você começou a praticar para um evento que está prestes a acontecer.
> No primeiro dia você correu `x` milhas, e no dia do evento você deverá correr `y` milhas.
> Calcule o número de dias para que você consiga correr a distância necessária para o evento, se você aumentar a distância percorrida 10% a mais que o dia anterior.
> Printe um inteiro representado o número de dias demorados para alcançar a distância desejada.

**Exemplo de entrada**:

```text
10
30
```

**Exemplo de sáida**:

```text
13
```

## Resposta proposta


Para todo o exercício deste curso existe uma resposta modelo, que serve apenas para mostrar uma maneira de resolver o problema que talvez você não tenha percebido.

A resposta proposta para este exercício é a seguinte:


```python
x, y = int(input()), int(input())
num_days = 1

while x < y:
    x *= 1.1
    num_days += 1

print(num_days)
```

Até aí tudo legal, o exercício se propõe a ensinar laço de repetição para o aluno.

Mas repare bem no nosso problema, temos um valor inicial, temos um período de tempo crescente, temos um percentual aplicado ao resultado do percentual anterior e aí temos um resultado final. Se trocarmos os nomes das variáveis fica mais claro que isso é um cálculo de juro composto!

Pensa comigo, o exemplo de entrada/saída do enunciado significa:

- Eu corro 10 milhas;
- Corro 10% a mais por dia;
- Quantos dias até eu conseguir correr 30 milhas ou mais?

Se eu alterar os nomes das váriaveis poderia escrever:

- Eu tenho uma dívida de 10 reais;
- Eu pago 10% de juros por dia;
- Quantos dias até minha dívida ser de 30 reais ou mais?

Viu como fica mais claro que é um problema de juro composto?

Beleza, então temos um problema que pode ser resolvido com um cálculo, sem a necessidade de fazer um loop. Isso torna nossa solução, que tinha complexidade computacional linear (_O(N)_) em uma solução de complexidade constante (_O(1)_). Ou seja, a solução _O(N)_ vai demorar mais para ser calculada quanto mais dias eu precisar para correr a `Y` milhas, já a solução _O(1)_ vai calcular nosso problema em tempo constante independente de quantos dias eu precisaria para correr as `Y` milhas.

Antes de entrar na fórmula vou falar um pouco sobre cálculo percentual, apenas uns *insights* caso você ainda não tenha pensado no assunto com mais carinho.



## Porcentagens

A própria palavra *porcentagem* vem do latim _per centum_ que significa **por cento** ou **a cada 100** (não me diga!).
O que precisamos lembrar é o que isso significa matematicamente. Por exemplo, `50%`:

$$$
50\\% = \frac{50}{100} = \frac{1}{2} = 0.5
$$$

Os números acima são equivalentes, quando fazemos cálculos com percentual usamos `0.5` (ou frações, dependo da preferência). Então se eu quiser saber quanto é 50% de `x`, é só calcular $x \times 0.5$.

Também sabemos que:

- qualquer número vezes zero é zero ($x \times 0 = 0$);
- qualquer número vezes 1 é ele mesmo ($x \times 1 = x$).

Mas podemos enxergar isso de outro jeito, podemos enxergar que o `0` e o `1` são os percentuais `0%` e `100%` respectivamente. Então ficaria:

- 0% de qualquer número é 0 ($x \times 0 = 0$);
- 100% de qualquer número é ele mesmo ($x \times 1 = x$).

Pra finalizar essa parte, se quisermos acrescentar 10% a um valor basta multiplicar por `1.1`, ou seja $x \times 1.1$, pois assim estaríamos pegando o próprio valor (1) mais os 10% dele (0.1).


## Descobrindo a fórmula do juro composto

O juro composto nada mais é do que um juro "retro-alimentado", pois ele é aplicado ao resultado do juro anterior e assim por diante.

Usando nosso exemplo, precisamos aplicar uma porcentagem em um valor inicial e depois ir aplicando este percentual no resultado do cálculo anterior. Veja, de maneira simples e ineficaz temos:

```python
valor_inicial = 10  # primeiro dia sem juros aplicados (lembre-se dessa linha)
valor_futuro = valor_inicial * 1.1  # 1 dia de juros
valor_futuro = valor_futuro * 1.1  # 2 dia de juros
valor_futuro = valor_futuro * 1.1  # 3 dia de juros
...
valor_futuro = valor_futuro * 1.1  # N dias de juros
```

Que também pode ser expressa da seguinte maneira:

```python
valor_inicial = 10
valor_futuro = (((valor_inicial * 1.1) * 1.1) * 1.1) ... # N vezes
```

Os parênteses no exemplo acima são desnecessários pois a multiplicação é uma operação com [distributividade][wiki.distributividade], ou seja, o resultado é o mesmo independente da ordem dos operandos ($2 \times 3 = 6 \iff 3 \times 2 = 6$). Removendo os parênteses temos:

```python
valor_inicial = 10
valor_futuro = valor_inicial * 1.1 * 1.1 * 1.1 ... # N vezes
```

Opa, o número `1.1` está sendo multiplicado repetidamente! Qualquer número `x` multiplicado `n` vezes por ele mesmo é uma exponenciação: 

$$$
x \times x \times x \times \mathellipsis = x ^n
$$$

Adaptando o nosso código:

```python
valor_inicial = 10
valor_futuro = valor_inicial * (1.1 ** n)
```

Pronto, chegamos à formula de juros compostos! Na [wikipedia][wiki.juros] a fórmula é mostrada como:

$$$
V_f = V_p(1 + j) ^ n
$$$

Sendo:

- $V_f$: Valor futuro
- $V_p$: Valor presente
- $j$: juro
- $n$: número de períodos

Que é exatamente o que encontramos, pois já calculamos o $i + j$ dos 10% de juros como `1.1`.


## Aplicando a fórmula

Ao substituir os valores do enunciado na fórmula temos:

$$$
30 = 10(1 + 0.1) ^ n
$$$


Nós já sabemos o valor futuro então precisamos isolar o `n` para podermos aplicar a fórmula:

{{< safe_html >}}
$$$
\begin{align}
  V_f &= V_p(1 + j) ^ n \\
  \frac{V_f}{V_p} &= (1 + j) ^ n \\
  (1 + j) ^ n &= \frac{V_f}{V_p} \\
  n &= \log_{(1 + j)} (\frac{V_f}{V_p})
\end{align}
$$$
{{< /safe_html >}}

O último passo é transformar a exponenciação do passo 3 ($(1 + j) ^ n$) em um logaritmo e no passo 4 temos a nossa fórmula final!

```python
from math import log

n = log(V_f / V_p, j)
```

Substituindo os valores do nosso problema na fórmula:

```python
from math import log

# V_f (valor futuro) = 30 milhas
# V_p (valor presente) = 10 milhas
# j (juros) = 10% a mais ao dia, ou seja, 1.1

n = log(30 / 10, 1.1)
n = log(3, 1.1)
n = 11.526704607247604
```

Como vai demorar aproximadamente onze dias e meio para chegarmos às 30 milhas e estamos trabalhando com dias inteiros, arredondamos o valor para 12. Isso significa que correremos 30 milhas apenas depois de melhorar nossa corrida 12 vezes.

Ué?! Mas a resposta não era pra ser 13 dias?
Sim! Lembra da linha que eu disse para não esquecer? São 12 dias aperfeiçoando a corrida (ou contando juros) para dar este resultado, porém temos que adicionar o 1º dia, pois ele não tem os "juros" aplicados ainda, ou seja, nosso cálculo aponta quantos dias precisamos aperfeiçoar nossa corrida e o 1º dia não conta como aperfeiçamento. Portanto é só adicionar `1` ao nosso cálculo e temos a resposta correta, que é `13`.

Então nossa resposta ficou:

```python
from math import log, ceil

v_p, v_f = int(input()), int(input())
n = log(v_p/v_f, 1.1)

print(ceil(n) + 1)
```

## Performance

Por fim, vamos demonstrar a diferença de performance das duas abordagens. Visto que a abordagem que utiliza `for` é _O(N)_, o tempo para executar o algoritmo deve aumentar linearmente de acordo com a quantidade de _loops_ executados. Já a segunda abordagem deve ser execudata em tempo constante, pois independente da quantidade de dias que for a resposta, o resultado é calculado diretamente usando o Log.

Para o teste de performance criei este [Repl.it][replit.performance] com o seguinte código:

```python
from math import ceil, log
import timeit


def calcular_for(dist_inicial, dist_meta, percentual):
    num_dias = 1

    while dist_inicial < dist_meta:
        dist_inicial *= percentual
        num_dias += 1
    
    return num_dias

def calcular_log(dist_inicial, dist_meta, percentual):
    num_dias = log(dist_meta/dist_inicial, percentual)
    return ceil(num_dias) + 1


# Argumentos para os testes no padrão
# (dist_inicial, dist_meta, percentual)
testes = [
    (10, 30, 1.1),           # 10% ao dia, resultado 13
    (10, 1_000_000, 1.01),   # 1% ao dia, resultado 1159
    (10, 1_000_000, 1.001),  # 0.1% ao dia, resultado 12_214
]

for args in testes:
    # garante que as respostas são as mesmas sempre
    assert calcular_for(*args) == calcular_log(*args)

    t = timeit.timeit("calcular_for(*args)", number=10_000, globals=globals())
    print(f"calcular_for{args!r}: {t: >07.4f} s")

    t = timeit.timeit("calcular_log(*args)", number=10_000, globals=globals())
    print(f"calcular_log{args!r}: {t: >07.4f} s")

```

Os resultados do replit foram:

```txt
calcular_for(10, 30, 1.1):  0.0335s
calcular_log(10, 30, 1.1):  0.0046s
calcular_for(10, 1000000, 1.01):  3.6109s
calcular_log(10, 1000000, 1.01):  0.0088s
calcular_for(10, 1000000, 1.001): 41.0780s
calcular_log(10, 1000000, 1.001):  0.0079s
```

Estes resultados podem variar de máquina para máquina, mas como vocês podem perceber mesmo para o nosso caso inicial que executa apenas 12 loops a diferença foi de 33.5ms para 4.6ms. E no pior caso do teste que precisaria de 12.214 loops para chegar numa resposta a diferença foi de mais de 40 segundos!!

## Conclusão

Espero que este artigo sirva como um exemplo de como podemos melhorar o desempenho de algoritmos comuns do nosso dia-a-dia aplicando fórmulas ao invés de laços de repetição e também de como analisar a performance do seu código usando o módulo [`timeit`][timeit]. Ou pelo menos que você tenha aprendido uma coisa nova. :)



[CodePen]: https://codepen.io/
[JSFiddle]: https://jsfiddle.net/
[Gists]: https://gist.github.com/

[Repl.it]: https://repl.it/ "Repl.it"
[replit.exercicio]: https://repl.it/student/submissions/1205564 "6.4. While: Jogging"
[replit.comunidade]: https://repl.it/community "Cursos disponíveis gratuitamente na plataforma"
[replit.estudante]: https://repl.it/student "Cursos incritos"
[replit.curso]: https://repl.it/student/classrooms/17929 "Auto-Graded Course with Solutions"
[replit.performance]: https://replit.com/@fernandosavio/blog-post-juros-compostos-performance#main.py "Repl.it com os resultados de performance"

[docs.factorial]: https://docs.python.org/3/library/math.html#math.factorial "math.factorial"
[wiki.juros]: https://pt.wikipedia.org/wiki/Juro#Juros_compostos "Juros compostos (Wikipedia)"
[wiki.distributividade]: https://pt.wikipedia.org/wiki/Distributividade "Distributividade (Wikipedia)"
[timeit]: https://docs.python.org/3/library/timeit.html "Documentação do módulo 'timeit'"

