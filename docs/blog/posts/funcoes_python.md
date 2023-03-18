---
title: Funções em Python
description: Um pouco sobre as operacionalidade das funções em python e como utilizar parâmetros, *args, parâmetros defaults, *kwargs e a importância de sua sequência.

author: Abel Salim
tags:
  - funções
  - python

hide:
  - footer
---

# Funções em Python

Num âmbito geral, as funções trouxeram para as linguagens de programação a
possibilidade da reutilização de trechos de código em qualquer lugar do
arquivo. Após a adoção da parametrização o resultado são códigos limpos e de
fácil leitura.

Em Python uma função é definida pela palavra reservada `def` seguido do nome da
função e entre parênteses os parâmetros a serem atribuídos caso a função
necessite dos mesmos.

```{.py3 hl_lines='' linenums="1" title=""}
def funcao(parametros):
    print('Corpo da função')
```

> def = Palavra especial;
>
> funcao = Nome da função;
>
> parametros = Parâmetros da função;
> 
> Tudo que estiver com a edentação de quatro espaços após os dois pontos é
> considerado como corpo da função, portanto será executado quando o código
> chamá-la.

```{.py3 hl_lines='' linenums="1" title=""}
def adicao(x, y):
    """Função simples para soma de dois parâmetros"""
    return x + y


print(soma(1, 2))                      # Resultado = 03
```

O exemplo acima ilustra uma função que recebe dois parâmetros e retorna a soma
dos mesmos ao ser executado.

__Em funções Python podemos passar quatro tipos de parâmetros: normais , *args,
default, e **kwargs.__

___

## Parâmetros normais

Assim como foi ilustrado anteriormente na função `adicao`, os parâmetros normais
são aqueles que simplesmente são chamados tornando a função dependente da
entrada e ao mesmo tempo sujeita a um `TypeError` com a ausência da mesma.

```{.py3 hl_lines='' linenums="1" title=""}
def adicao(x, y):
    """Função simples para soma de dois parâmetros"""
    return x + y


print(soma(1, 2))                      # Resultado = 03
```

O exemplo abaixo ilustra a função `adicao` que deve receber dois parâmetros de
entrada, porém retornou um `TypeError` ao ser executada com apenas um parâmetro.

```{.py3 hl_lines='' linenums="1" title=""}
def adicao(x, y):
    """Função simples para soma de dois parâmetros"""
    return x + y


print(soma(2))

"""
Resultado:
    "TypeError: soma() missing 1 required positional argument: 'y'"
"""
```

___

## Parâmetros *args

Args é um parâmetro ‘especial’ que recebe os argumentos e os insere em uma
tupla, e assim como as listas, tuplas podem ser iteradas tornando possível
fazer múltiplas entradas sem necessariamente setar parâmetro por parâmetro na
para a função.

Antes de darmos início aos exemplos vamos deixar claro que a palavra args é
apenas uma convenção, ou seja, contanto que o asterisco venha primeiro, qualquer
nome ou caractere será aceito.

### Exemplo 1

Um exemplo simples e prático seria atribuirmos o args a função adicao para
passarmos entradas múltiplas de argumentos e em seguida retornarmos a soma de
todas as entradas.

```{.py3 hl_lines='' linenums="1" title=""}
def soma(*args):
    """Função simples para soma de parâmetros com *args"""
    soma = sum(args)
    return soma


print(soma(1))                         # Resultado = 01
print(soma(2, 3))                      # Resultado = 05
print(soma(3, 4, 5))                   # Resultado = 12
print(soma(4, 5, 6, 7))                # Resultado = 22
print(soma(5, 6, 7, 8, 9))             # Resultado = 35
```

Note que na função adicao foi realizada a soma e independente da quantidade de
argumentos (1, 2, 3, 4, …, 1000), irá ser realizado a soma dos valores ali
inseridos.

> A função `sum()` tem a finalidade de retornar a soma de quaisquer valores que
> sejam inteiros ou float.

### Exemplo 2

Para o nosso segundo exemplo, vamos imaginar que em uma loja de equipamentos
eletrônicos precisamos somar todos os valores recebidos a cada venda.

Embora a função seja relativamente simples, vamos realizar esse recebimento de
duas maneiras: soma em tempo real de execução com a utilização de um for e
utilizar a função `sum()`.

Utilizando o for:

```{.py3 hl_lines='' linenums="1" title=""}
recebimentos = [1.2, 32.3, 40, 17.9, 137.37, 98, 47, 5, 2.5, 66]


def caixa(*args):
    """Função simples para descompactar a lista recebimentos com *args"""
    soma = 0
    for x in args:
        soma += x
    return soma


print(caixa(*recebimentos))             # Resultado = 447.27
```

Utilizando `sum()`:

```{.py3 hl_lines='' linenums="1" title=""}
recebimentos = [1.2, 32.3, 40, 17.9, 137.37, 98, 47, 5, 2.5, 66]


def caixa(*args):
    """Função simples para descompactar a lista recebimentos com *args"""
    soma = sum(args)
    return soma


print(caixa(*recebimentos))            # Resultado = 447.27
```

Note que com a função sum para realizar a soma dos valores o código se torna
mais ‘enxuto’, o que facilita uma futura leitura ou manutenção no código.

___

## Parâmetros default

Os parâmetros com valor default servem para deixar opcional a sua entrada,
tornando assim o código mais maleável.

Voltando ao nosso exemplo da função adicao, podemos definir o valor padrão para
y e optar em passar ou não um argumento.

```{.py3 hl_lines='' linenums="1" title=""}
def adicao(x, y=10):
    """Função simples para soma de dois parâmetros"""
    return x + y


print(soma(1))                         # Resultado = 11
print(soma(1, 2))                      # Resultado = 03
```

Nessa situação, a função recebe como valor default para y o inteiro 10, ou seja,
caso a entrada atribua um valor à y, a função irá receber as suas entradas e
fazer a soma com os respectivos valores para x e y, mas se esse não for o caso
e apenas um valor tenha sido atribuído, o parâmetro y receberá valor default 10
e a função não retornará um `TypeError`.

___

## Parâmetros **kwargs

Kwargs é um parâmetro ‘especial’ que recebe os argumentos como dicionário e
assim como as listas ou tuplas, dicionários podem ser iterados tornando
possível fazer múltiplas entradas sem necessariamente setar uma a uma na função.

Antes de darmos início aos exemplos vamos deixar claro que a palavra kwargs é
apenas uma convenção, ou seja, contanto que os dois asteriscos venham primeiro,
qualquer nome ou caractere será aceito.

### Exemplo 1

Para um exemplo simples e prático vamos entrar com um kwargs que informa a
cor favorita de alguém:

```{.py3 hl_lines='' linenums="1" title=""}
def cor_favorita(**kwargs):
    for nome, cor in kwargs.items():
        print(f'A cor favorita de {nome.title()} é {cor}')


cor_favorita(salim='preto')

"""
Resultado:
    A cor favorita de Salim é preto
"""
```

Note que foi atribuído a chave salim (nome do indivíduo) e o valor preto (cor
favorita).

> Para essa função foi utilizada a função items que possibilita iterar em um
> dicionário através de um for explorando tanto chave quanto valores

### Exemplo 2

Para ilustrarmos o exemplo vamos fazer uma função que receba um dicionário
contendo como chave o nome de uma pessoa e como valor sua cor favorita, e
imprima uma frase informando a quem pertence a cor favorita.

```{.py3 hl_lines='' linenums="1" title=""}
dicionario = {
    'salim': 'preto',
    'gigi': 'branco',
    'edson': 'vermelho',
    'rosy': 'rosa',
    'lucas': 'azul',
    'samuel': 'verde'
}


def cor_favorita(**kwargs):
    for nome, cor in kwargs.items():
        print(f'A cor favorita de {nome.title()} é {cor}')


cor_favorita(**dicionario)

"""
Resultado:
    A cor favorita de Salim é preto
    A cor favorita de Gigi é branco
    A cor favorita de Edson é vermelho
    A cor favorita de Rosy é rosa
    A cor favorita de Lucas é azul
    A cor favorita de Samuel é verde
"""
```

> Para essa função foi utilizada a função title que transforma a primeira de
> cada palavra de uma string em maiúscula.

___

## Ordem

Em Python os parâmetros de função seguem a ordem ‘normais, default, *args e
**kwargs’, caso anexe um tipo de parâmetro antes ou após da sua ordem definida
o resultado vai ocorrer, mas não como esperado!

Um exemplo para entendermos a importância da ordem dos parâmetros em uma função
pode ser exemplificada a seguir:

```{.py3 hl_lines='' linenums="1" title=""}
lista_args = [1, 2, 4]


def exemplo(a, b, *args, linguagem='Python', **kwargs):
    return [a, b, args, linguagem, kwargs]


print(exemplo(1, 2, *lista_args))
print(exemplo(1, 2, linguagem='JavaScript', nome='Abel'))
print(exemplo(1, 2, *lista_args, nome='Abel', sobrenome='Salim'))

"""
Resultados
    [1, 2, (1, 2, 4), 'Python', {}]
    [1, 2, (), 'JavaScript', {'nome': 'Abel'}]
    [1, 2, (1, 2, 4), 'Python', {'nome': 'Abel', 'sobrenome': 'Salim'}]
"""
```

Na função `exemplo` demos entradas com dois parâmetros normais (a, b), args
(desempacotando a lista ‘lista_args’), default (linguagem=’Python’) e Kwargs.

Nesse tipo de função temos que passar com obrigação apenas os parâmetros normas
(‘a’ e ‘b’ que receberam respectivamente os valores de 1 e 2) e os demais são
opcionais, onde args e kwargs retornam respectivamente uma tupla e dicionário
vazias.

Caso a ordem (normal, args, default, kwargs) não seja respeitada, um dos
problemas que podemos ter será o exemplificado a seguir:

```{.py3 hl_lines='' linenums="1" title=""}
lista_args = [1, 2, 4]


def exemplo(a, b, linguagem='Python', *args, **kwargs):
    return [a, b, linguagem, args, kwargs]


print(exemplo(1, 2, linguagem='JavaScript', nome='Abel'))
print(exemplo(1, 2, *lista_args, nome='Abel', sobrenome='Salim'))

"""
Resultados
    [1, 2, 'JavaScript', (), {'nome': 'Abel'}]
    [1, 2, 1, (2, 4), {'nome': 'Abel', 'sobrenome': 'Salim'}]
"""
```

Ao executarmos essa função que solicita os parâmetros fora de ordem temos sim
um retorno, mas totalmente diferente do que esperamos.

No primeiro print foi atribuído os seguintes valores:

```
a = 1
b = 2
args = ()
default = ‘JavaScript’
kwargs = “ nome=’Abel’ ”
```

E esperado o seguinte retorno:

```
[1, 2, (), ‘JavaScript’, {‘nome’: ‘Abel’}]
```

Mas com a alteração da ordem dos parâmetros tivemos o seguinte retorno:

```
a = 1
b = 2
args = ‘JavaScript’
default = ()
kwargs = “ nome=’Abel’ ”
```

Ou seja, ao alterarmos a sequencia dos parâmetros em python não temos um
retorno de algum erro da linguagem, mas como as saídas de args e default foram
alteradas, essa modificação retorna uma saída inválida ao que a função foi
destinada a realizar.

___

## Conclusão

Essa foi uma de várias explicações sobre funções que você vai achar na
internet, mas sinceramente, espero que tenha ajudado!