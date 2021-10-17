---
# type: post
title: "Juro composto aplicado a um exemplo real"
date: 2017-07-02T05:00:00Z
# images:
#   - "/images/featured/digital-marketing.jpg"
description: "Resolução de um problema proposto na plataforma repl.it que usa a fórmula de juros compostos ao invés de usar loop."
categories: 
  - python
---


## TL;DR

Foi aplicada a fórmula do juro composto (`V_f = V_p(1 + j) ** n`) para resolver o problema sem usar laços de repetições, apenas matemática.

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
n = log(v_p/v_f, 1.1)

print(ceil(n) + 1)
```

---

## Introdução

Sempre que preciso enviar um código HTML, Javascript ou CSS de exemplo para alguém (seja para ajudar ou só trocar uma ideia mesmo) utilizo o [CodePen] ou o [JSFiddle], mas quando o assunto é python eu utilizava os [Gists] do GitHub. Há algumas semanas conheci o [Repl.it], um serviço cuja função se assemelha aos citados anteriormente, um interpretador online para várias linguagens ([várias mesmo](https://repl.it/languages)), sendo o Python 3 uma delas.

Até aí OK, porém semana passada descobri que o Repl.it tem uma plataforma de professor/aluno onde eu posso criar cursos com N exercícios onde eu crio testes para estes exercícios, e estes teste definem se a resposta do aluno está correta. Ainda não criei nenhum curso, e talvez não crie, mas vi que pode-se corrigir manualmente cada exercício dado aos alunos.

O fato é que, por mais que eu não assuma o papel de professor, decidi assumir o papel de aluno. Afinal a plataforma tem um seção "[Comunidade][replit.comunidade]" que apresenta todos os cursos criados pela comunidade e eu posso me inscrever em qualquer um deles.

Dito isso, me inscrevi no curso de Python 3 com um objetivo em mente, treinar minha lógica e boas práticas resolvendo os exercícios de maneira que eu use bem os recursos do Python 3. Exemplo: por mais que o exercício peça que eu use um `while` para fazer um fatorial, eu sei da existência da função `math.factorial` ([docs][docs.factorial]) e em um projeto real, esta seria a melhor opção.

Alguns desses exercícios me chamaram a atenção, e este é o motivo deste post. Vamos ao que interessa!


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


~~~python
x, y = int(input()), int(input())
num_days = 1

while x < y:
    x *= 1.1
    num_days += 1

print(num_days)
~~~

Até aí tudo legal, o exercício se propõe a ensinar laço de repetição para o aluno.

Mas repare bem nosso problema, temos um valor inicial, temos um período de tempo crescente, temos um percentual aplicado ao resultado do percentual anterior e um resultado final. Se trocarmos os nomes das variáveis fica mais claro que isso é um cálculo de juro composto!

Pensa comigo, o exemplo de entrada/saída do enunciado significa:

- Eu corro 10 milhas
- Corro 10% a mais por dia
- Quantos dias até eu conseguir correr 30 milhas ou mais?

Se eu alterar os nomes das váriaveis poderia escrever:

- Eu tenho uma dívida de 10 reais
- Eu pago 10% de juros por dia
- Quantos dias até minha dívida ser de 30 reais ou mais?

Beleza, então temos um problema que pode ser resolvido com um cálculo, sem a necessidade de fazer um loop. Isso torna nossa solução, que tinha complexidade computacional linear (*O(N)*) em uma solução de complexidade constante (*O(1)*).

Antes de entrar na fórmula vou falar um pouco sobre cálculo percentual, apenas uns *insights* caso você ainda não tenha pensado nisso.



## Porcentagens

A própria palavra *porcentagem* vem de *porcento* que significa **por cento** ou **a cada 100** (não me diga!).
O que precisamos lembrar é o que isso significa matematicamente. Por exemplo, `50%`:

~~~python
50% == 50/100 == 1/2 == 0.5
~~~

Os números acima são equivalentes, quando se faz cálculos com percentual usamos `0.5` (ou frações, dependo da preferência). Então se eu quiser saber quanto é 50% de `x`, é só calcular `x * 0.5`.

Também sabemos que:

- qualquer número vezes zero é zero (`x * 0 == 0`);
- qualquer número vezes 1 é ele mesmo (`x * 1 == x`).

Mas podemos enxergar isso de outro jeito, podemos enxergar que o `0` e o `1` são percentuais. Então ficaria:

- 0% de qualquer número é 0 (`x * 0 == 0`);
- 100% de qualquer número é ele mesmo (`x * 1 == x`).

Pra finalizar essa parte, se quisermos acrescentar 10% a um valor basta multiplicar por `1.1`, ou seja `x * 1.1`.


## Descobrindo a fórmula do juro composto

Precisamos aplicar uma porcentagem em um valor inicial e depois ir aplicando este percentual no resultado do cálculo anterior. De maneira simples e ineficaz:

~~~python
valor_inicial = 10  # Lembre-se desta linha, vou citá-la adiante
valor_futuro = valor_inicial * 1.1  # 1 dia de juros
valor_futuro = valor_futuro * 1.1  # 2 dia de juros
valor_futuro = valor_futuro * 1.1  # 3 dia de juros
...
valor_futuro = valor_futuro * 1.1  # N dias de juros
~~~

Que também pode ser expressa da seguinte maneira:

~~~python
valor_inicial = 10
valor_futuro = (((valor_inicial * 1.1) * 1.1) * 1.1) ... # N vezes
~~~

Como a multiplicação é uma operação com distributividade, ou seja, o resultado é o mesmo independente da ordem dos operandos (`2 * 3 == 6` e `3 * 2 == 6`), os parênteses do último exemplo são desnecessários.

~~~python
valor_inicial = 10
valor_futuro = valor_inicial * 1.1 * 1.1 * 1.1 ... # N vezes
~~~

Agora sim! Um número `x` multiplicado N vezes por ele mesmo é uma exponenciação `x * x * x * ... == x ** n`. Reescrevendo o código:

~~~python
valor_inicial = 10
valor_futuro = valor_inicial * (1.1 ** n)
~~~

Pronto, chegamos à formula de juros compostos! Na [wikipedia][wiki.juros] a fórmula é mostrada como:

~~~python
V_f = V_p(1 + j) ** n
~~~

Sendo:

- `V_f`: Valor futuro
- `V_p`: Valor presente
- `j`: juro
- `n`: número de períodos

Que é exatamente o que encontramos, pois já calculamos os 10 % de juros como `1.1`.


## Aplicando a fórmula

Ao substituir os valores do enunciado na fórmula temos:

~~~python
30 = 10(1 + 0.1)^n
~~~


Nós já sabemos o valor futuro então precisamos isolar o `n` para podermos aplicar a fórmula:

~~~python
V_f = V_p(1 + j) ** n
V_f / V_p = j ** n
j ** n = V_f / V_p
~~~

O último passo é transforma a exponenciação (`j ** n`) em um logaritmo e temos a nossa fórmula!

~~~python
from math import log

n = log(V_f / V_p, j)
~~~

Substituindo os valores:

~~~python
from math import log

n = log(30 / 10, 1.1)
n = log(3, 1.1)
n = 11.526704607247604
~~~

Como vai demorar aproximadamente onze dias e meio para chegarmos às 30 milhas e estamos trabalhando com dias inteiros, arredondamos o valor para 12.

Ué?! Mas a resposta não era pra ser 13 dias?
Sim! Lembra da linha que eu disse para não esquecer? São 12 dias aperfeiçoando a corrida (ou contando juros) para dar este resultado, porém temos que adicionar o 1º dia, pois ele não tem os "juros" aplicados ainda, ou seja, é nosso valor inicial. Portanto é só adicionar `1` ao nosso cálculo e temos a resposta correta, que é `13`.

Então nossa resposta ficou:

```python
from math import log, ceil

v_p, v_f = int(input()), int(input())
n = log(v_p/v_f, 1.1)

print(ceil(n) + 1)
```

Como é o primeiro artigo que escrevo, não sei se ficou muito longo, detalhado e chato, mas como queria guardar esse tipo de experiência, espero que sirva de alguma coisa para outras pessoas também.


[CodePen]: https://codepen.io/
[JSFiddle]: https://jsfiddle.net/
[Gists]: https://gist.github.com/

[Repl.it]: https://repl.it/ "Repl.it"
[replit.exercicio]: https://repl.it/student/submissions/1205564 "6.4. While: Jogging"
[replit.comunidade]: https://repl.it/community "Cursos disponíveis gratuitamente na plataforma"
[replit.estudante]: https://repl.it/student "Cursos incritos"
[replit.curso]: https://repl.it/student/classrooms/17929 "Auto-Graded Course with Solutions"

[docs.factorial]: https://docs.python.org/3/library/math.html#math.factorial "math.factorial"
[wiki.juros]: https://pt.wikipedia.org/wiki/Juro#Juros_compostos

