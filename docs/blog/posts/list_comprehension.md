---
title: List Comprehension em Python
description: As compressões são formas ágeis e eficazes para gerar listas de maneira concisa, e embora a utilização de loop com ou sem operações condicionais façam a mesma coisa, um list comprehension pode proporcionar uma economia considerável no consumo de memória e linhas codadas.

author: Abel Salim
tags:
  - funções
  - list comprehension
  - python

hide:
  - footer
---

# List Comprehension em Python

As compressões são formas ágeis e eficazes para gerar listas de maneira
concisa, e embora a utilização de loop com ou sem operações condicionais façam
a mesma coisa, um list comprehension pode proporcionar uma economia
considerável no consumo de memória e linhas codadas.

## Estruturas de Repetição e List Comprehensions

Uma grande dúvida sobre compressões seria quando e porque usá-la, afinal as
estruturas de repetições são amplamente conhecidas e relativamente mais
simples. Então porque usá-la?

* Codagem: Compressões são geralmente mais compactas que funções ou loops,
proporcionando uma economia substancial em linhas codadas.

* Depuração: Se utilizada corretamente, a forma compacta das compressões
facilita a leitura de depuração do código.

___

## Sintaxe

Antes de qualquer coisa vamos entender sua sintaxe.

```python
[dado for dado in iterável]
```

Um list comprehension sempre será executado dentro de colchetes (assim como uma
lista) e o `return` será implícito.

Para ilustrarmos as diferenças entre as funcionalidades, vamos explanar sobre
loops e compressões individualmente.

Em nosso exemplo vamos receber um iterável e retornar seu quadrado.

___

### Utilizando estrutura de repetição For

```{.py3 hl_lines='' linenums="1" title=""}
numeros = [1, 2, 3, 4, 5, 6, 7, 8, 9, 10]
quadrado = []

for numero in numeros:
    quadrado.append(numero ** 2)

print(quadrado)         # Resultado = [1, 4, 9, 16, 25, 36, 49, 64, 81, 100]
```

Para esse código criamos uma lista com numerais para iterar no `for` e também
uma outra lista (quadrado) para inserirmos o quadrado de cada número da lista
`numeros`.

Em seguida um `for` se encarregou de iterar na lista `numeros` e a cada rodada
do `for` foi inserido o quadrado de cada número na lista `quadrado`.

___

### Utilizando List Comprehension

```{.py3 hl_lines='' linenums="1" title=""}
numeros = [1, 2, 3, 4, 5, 6, 7, 8, 9, 10]

quadrado = [numero ** 2 for numero in numeros]

print(quadrado)         # Resultado = [1, 4, 9, 16, 25, 36, 49, 64, 81, 100]
```

Para esse código criamos uma lista com numerais, e na lista quadrado contém um
comprehension onde um for irá iterar no iterável `numeros` através da variável 
numero atribuída no for, e retorna justamente o quadrado da variável da rodada
(variável numero), ou seja, o retorno será o quadrado de cada número iterado.

___

## Aplicando Lógica Condicional as Compressões

As estruturas condicionais (if e else) podem ser adicionadas às compressões com
pouco esforço, basta entender a sintaxe.

___

## Uma condicional

Nas compressões podemos utilizar uma condicional apenas, caso essa seja a
necessidade. A seguir veremos uma breve sintaxe/escopo da compressão.

```python
[dado for dado in iterável if condicao]
```

Agora veremos breves exemplos de sua utilização juntamente com loops para fins
comparacionais.

___

### Utilizando estrutura de repetição For

Separando os números ímpares de uma lista em uma nova lista.

```{.py3 hl_lines='' linenums="1" title=""}
numeros = [1, 2, 3, 4, 5, 6, 7, 8, 9, 10]
numeros_impares = []

for numero in numeros:
    if numero % 2:
        numeros_impares.append(x)

print(numeros_impares)  # Resultado = [1, 3, 5, 7, 9]
```

No exemplo acima criamos uma lista com os números de 1 a 10, e outra vazias
para inserirmos os números ímpares após fazermos o filtro com o `for`.

Para realizar a separação/descoberta dos números ímpares em um conjunto maior
temos que dividi-los por 2, sendo que se o resto da divisão for 1, significa
que o dividendo é ímpar, senão o dividendo será par.

Felizmente podemos utilizar o operador `%` que realiza a divisão dos números
repassados e retorna o resto dessa divisão.

Por fim, ao 'printarmos' as listas `numeros_impares` será retornado os números
ímpares contidos na lista `numeros`.
___

### Utilizando List Comprehension

Separando os números ímpares de uma lista em uma nova lista.

```{.py3 hl_lines='' linenums="1" title=""}
numeros = [1, 2, 3, 4, 5, 6, 7, 8, 9, 10]

numeros_impares = [numero for numero in numeros if numero % 2]

print(numeros_impares)  # Resultado = [1, 3, 5, 7, 9]
```

No exemplo acima criamos uma lista com os números de 1 a 10 e uma outra
(numeros_impares) que recebeu diretamente a list comprehension, pois as
compressões retornam o que for executado a cada `for`, ou seja, os número da
lista ao quadrado.

> Nesse caso o código com for levou cerca de 06 linhas para ser codada, já o
> código com list comprehension levou apenas 03 linhas.

___

## Dupla Condicional

Nas compressões podemos utilizar dupla condicionais como `if` e `else`. A seguir
veremos uma breve sintaxe/escopo da compressão.

```python
[x.append(dado) if codicao else y.append(dado) for dado in iterável]
```

Sabendo como seria sua utilização agora vamos colocar a mão na massa e realizar
alguns exemplos.

___

### Utilizando estrutura de repetição For

Separando os números ímpares e pares de uma lista em duas novas listas.

```{.py3 hl_lines='' linenums="1" title=""}
numeros = [1, 2, 3, 4, 5, 6, 7, 8, 9, 10]

numeros_impares = []
numeros_pares = []

for numero in numeros:
    if numero % 2:
        numeros_impares.append(x)
    else:
        numeros_pares.append(x)
        
print(numeros_impares)  # Resultado = [1, 3, 5, 7, 9]
print(numeros_pares)    # Resultado = [2, 4, 6, 8, 10]
```

No exemplo acima criamos uma lista com os números de 1 a 10, e duas listas
vazias para inserirmos os números ímpares e pares após fazermos o filtro com o
`for`.

Para realizar a separação/descoberta dos números ímpares em um conjunto maior,
temos que dividi-los por 2, sendo que se o resto da divisão for 1, significa
que o dividendo é ímpar, senão o dividendo será par.

Felizmente podemos utilizar o operador % que realiza a divisão dos números
repassados e retorna o resto dessa divisão.

Por fim, ao printarmos as listas `numeros_impares` e `numeros_pares` será
retornado respectivamente os números ímpares e pares contidos na lista
`numeros`.

___

### Utilizando List Comprehension

Separando os números ímpares e pares de uma lista em duas novas listas.

Nesse tipo de código podemos optar em usar apenas uma condicional e criarmos
ambas as listas recebendo o retorno das compressões ou criar duas listas
(numeros_impares e numeros_pares) previamente ou num só comprehension
direcionar os números para as listas.

#### Exemplo 1

```{.py3 hl_lines='' linenums="1" title=""}
numeros = [1, 2, 3, 4, 5, 6, 7, 8, 9, 10]

numeros_impares = [numero for numero in numeros if numero % 2]
numeros_pares = [numero for numero in numeros if not numero % 2]

print(numeros_impares)  # Resultado = [1, 3, 5, 7, 9]
print(numeros_pares)    # Resultado = [2, 4, 6, 8, 10]
```

No exemplo acima criamos uma lista com os números de 1 a 10. Já as listas
receberam diretamente as list comprehension, pois as compressões retornam o que
for executado a cada for, ou seja, os número ímpares ou pares.

>Nesse caso o código com for levou cerca de 10 linhas para ser codada, já o
>código com list comprehension levou apenas 05 linhas.

#### Exemplo 2

```{.py3 hl_lines='' linenums="1" title=""}
numeros = [1, 2, 3, 4, 5, 6, 7, 8, 9, 10]

numeros_impares = []
numeros_pares = []

[numeros_impares.append(num) if num % 2 else numeros_pares.append(num) for num in numeros]

print(numeros_impares)                # Resultado = [1, 3, 5, 7, 9]
print(numeros_pares)                  # Resultado = [2, 4, 6, 8, 10]
```

Nesse exemplo criamos as duas listas previamente e em seguida realizamos a
atribuição dos números pares a lista `numeros_pares` e impares a lista
`numeros_impares` com uma só compressão.

Nesse momento deve imaginar que o exemplo 2 é maior, mau organizado e talvez
até mais complexo, então porque usar o exemplo 2 e não o 1? A resposta é
simples, consumo de memória!

Isso mesmo, o segundo exemplo traz uma economia em espaço alocado em memória,
mas obviamente essa economia se torna perceptível em códigos grandes e quase
imperceptíveis em pequenos scripts, isso porque pequenos scripts são geralmente
feitos para executarem uma ação e finalizarem o processo.

Para analisarmos essa diferença de consumo vamos utilizar do pacote `getsizeof`
do módulo `sys` que tem como inteira e única finalidade retornar o consumo de um
objeto/função/comprehension/generator/etc em bytes.

Para importarmos o pacote `sys` faremos um `from sys import getsizeof` e para
termos o retorno do consumo, colocaremos o trecho a ser avaliado como parâmetro
da função `getsizeof()`.

##### Exemplo 1 - getsizeof

```{.py3 hl_lines='' linenums="1" title=""}
from sys import getsizeof

numeros = [1, 2, 3, 4, 5, 6, 7, 8, 9, 10]

print(getsizeof([numero for numero in numeros if numero % 2]))
print(getsizeof([numero for numero in numeros if not numero % 2]))

# Resultado do 01º print = 120
# Resultado do 02º print = 120
```

##### Exemplo 2 - getsizeof

```{.py3 hl_lines='' linenums="1" title=""}
from sys import getsizeof

numeros = [1, 2, 3, 4, 5, 6, 7, 8, 9, 10]

numeros_impares = []
numeros_pares = []

print(getsizeof([numeros_impares.append(num) if num % 2 else numeros_pares.append(num) for num in numeros]))

# Resultado = 184
```

Como podemos ver nas saídas retornadas, o primeiro código embora mais bem
estruturado ou de fácil interpretação (opinião do autor), cada compressão
consome cerca de 120 bytes, ou seja, um consumo final de 240 bytes, já com o
segundo exemplo consumiu apenas 184 bytes. Nesse momento você deve ter se
perguntado o que é 56 bytes para o nível de tecnologia atual que temos hoje,
bom, tudo vai depender da finalidade do seu código!

Para sistemas complexos de gestão empresarial, a lógica e a forma de como foi
aplicada é vital, pois nesse cenário, tempo de execução é literalmente dinheiro.

Em scripts de execução rápida que apenas verificam parâmetros, status ou
funções simples, eu iria com um código mais ‘bonito’ ou de fácil interpretação.
:smile: :thumbsup:

___

## Apenas Estruturas Condicionais

Nas compressões podemos utilizar dupla condicionais como `if` e `else` sem
utilizar o `for`. A seguir veremos uma breve sintaxe/escopo da compressão.

```python
[x.append(dado) if codicao else y.append(dado)]
```

Por não termos um for para iterar podemos utilizar um list comprehension para
validação de parâmetros em funções ou métodos de objetos.

Nesse tópico vamos criar uma lista chamada `numeros` que irá conter números
iniciados em 1, terminados em 1000, num passo de 3 dígitos, ou seja, a lista
terá a seguinte composição: 1, 4, 7, 10, 13, 16, 19, 22, 25, …, 1000.

Sabendo como será sua utilização agora vamos colocar a mão na massa e realizar
alguns exemplos.

___

### Utilizando estrutura de repetição For

```{.py3 hl_lines='' linenums="1" title=""}
numeros = list(range(1, 1001, 3))


def verifica(numero):
    if numero in numeros:
        print(f'{numero} está na lista numeros')
    else:
        print(f'{numero} não está na lista numeros')


verifica(int(input('Informe um número: ')))
```

___

### Utilizando List Comprehension

```{.py3 hl_lines='' linenums="1" title=""}
numeros = list(range(1, 1001, 3))


def verifica(numero):
    [print(f'{numero} está na lista numeros') if numero in numeros else print(f'{numero} não está na lista numeros')]
     

verifica(int(input('Informe um número: ')))
```

Embora escritos de formas diferentes, ambos os trechos têm a mesma finalidade!
Após a criação da lista com range, verificamos se dado número chamado com o
input contido na execução da função `verifica` se faz presente na lista
`numeros`.

## Conclusão
Nesse artigo vimos a importância das compressões no meio do código e o poder que
elas tem nos quesitos de funcionalidade, agilidade, código limpo e economia em
memória. Espero que tenha ajudado! 
