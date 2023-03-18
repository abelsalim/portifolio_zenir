---
hide:
  - footer
---

# Check Comunicador

Script python para checagem e verificação de atividade para o Software monitor e
comunicador da SEFAZ
___

## O problema

Inicialmente tivemos um problema relacionado aos drives do MFE (Módulo Fiscal
Eletrônico) onde o mesmo ficava constantemente offline devido a queda dos
serviços do comunicador e monitor, porém, os mesmos ficavam up com a execução
manual via shell.

O script do comunicador é responsável por estabelecer a
comunicação do o MFE e o script monitor é responsável por retornar um status
visual do estado do módulo, ou seja, se encontra-se conectado ou não ao
computador.
___

## Solução

Como solução, trouxe um script em python onde o mesmo teria duas versões
visando a falta de padronização nos sistemas linux (como ubuntu 16.04 e 18.04,
Zorin 14.04 e 16.04).

> Nesse artigo trataremos do script adaptado como __Solução para o python3.6__.

A solução abordada foi o script `check_comunicador.py`, que basicamente utiliza
bibliotecas como threading, subprocess e time:

* __Threading:__ Utilizar threads possibilitou o a criação de treads para então
  realizarmos a checagem de modo concorrente ao inserirmos na pool.

* __Subprocess:__ A biblioteca subprocess é utilizada para realizar processos
  UNIX no python (como o up dos scripts que são feitos em shell).

* __Time:__ Utilizada apenas para definir o time de rechecagem.

```{.py3 hl_lines='1 2 3' linenums="3" title="check_comunicador.py"}
import threading as t
from subprocess import run, PIPE
from time import sleep 
```

Essa abordagem utiliza threads para execução em paralelo visando a limitação do
consumo de memória ao impedir a criação de processos órfãos, visto que o mesmo
seria executado via loop a cada dez segundos.
___

## Análise por trecho de código

De início temos a função `check_up()` que como o nome sugere, verifica se o
script está up e retorna ou não o seu PID (Número do processo UNIX), caso o
serviço não esteja up o retorno é uma string vazia.

Outro ponto é que essa função toma como parâmetro apenas o serviço `Comunicador`
para verificação de atividade. Essa medida foi tomada pois, as distros antigas
estão em processo de atualização, e também os erros dificilmente ocorrem com o
serviço do monitor (Ambas as verificações são abordadas e realizadas no script
`check_async.py`).

```{.py3 hl_lines='' linenums="8" title="check_comunicador.py"}
def check_up():
    """
    Função simples para coletar e retornar o PID do programa Comunicador.
    """

    command = run(['ps -C Comunicador -o pid|grep -v PID'],
                  shell=True,
                  stdout=PIPE,
                  universal_newlines=True)

    return command.stdout.strip()
```

Como sequência temos a função `up` que de fato irá realizar o start dos
programas repassados.

De início temos duas constantes que irão armazenar a localização dos scripts:
`COM` E `MON`.

* __COM:__ Variável com localização do script de start para o comunicador.
* __MON:__ Variável com localização do script de start para o Monitor.

```{.py3 hl_lines='1 2' linenums="28" title="check_comunicador.py"}
COM = ['/usr/bin/nohup /opt/sefaz/cco/bin/runcco-ser.sh > /dev/null 2>&1 &']
MON = ['/usr/bin/nohup /opt/sefaz/cco/bin/runcco-mon.sh > /dev/null 2>&1 &']
```

Em seguida veremos duas list comprehensions, onde a primeira fará um for em cada
objeto threading instanciado na variável `threads` realizando assim um up na
thread e por sua vez disponibilizando para a pool de processos e a segunda irá
realizar um join em cada objeto threading, com isso o processo só irá avançar
quando ambas as threads forem finalizadas. 


```{.py3 hl_lines='' linenums="21" title="check_comunicador.py"}
def up():
    """
    Função que executa o script que chama o Comunicador em background e
    redireciona sua saídas para /dev/null quando executada através de uma thread
    para limitar o consumo de memória.
    """

    COM = ['/usr/bin/nohup /opt/sefaz/cco/bin/runcco-ser.sh > /dev/null 2>&1 &']
    MON = ['/usr/bin/nohup /opt/sefaz/cco/bin/runcco-mon.sh > /dev/null 2>&1 &']


    threads = [
        t.Thread(target=run(COM, shell=True), name='up'),
        t.Thread(target=run(MON, shell=True), name='up')
    ]

    [th.start() for th in threads] 
    [th.join() for th in threads]
```

No `if __name__ == '__main__'` é inserido um loop onde caso o serviço não esteja
rodando, a função `up()` será chamada. O siclo se reinicia a cada 10 segundos
(Isso ocorre visando a queda do serviço na hora da emissão, ou seja, dez
segundos é tempo suficiente para cair e subir o serviço antes do envio do cupom
para a SEFAZ). 

```{.py3 hl_lines='' linenums="41" title="check_comunicador.py"}
if __name__ == '__main__':

    # O stdout do check_up(), ou seja, o output do run executado sempre será
    # em string, se o comunicador não estiver em execução o run() retornará
    # um string vazia, caso contrário o retorno será uma string com o PID do
    # processo.

    # Loop que verifica se o Comunicador está UP
    while True:
        if not check_up():
            up()

        sleep(10)
```

___

## Código completo

```{.py3 hl_lines='' linenums="1" title="check_comunicador.py"}
#!/usr/bin/python3

import threading as t
from subprocess import run, PIPE
from time import sleep 


def check_up():
    """
    Função simples para coletar e retornar o PID do programa Comunicador.
    """

    command = run(['ps -C Comunicador -o pid|grep -v PID'],
                  shell=True,
                  stdout=PIPE,
                  universal_newlines=True)

    return command.stdout.strip()


def up():
    """
    Função que executa o script que chama o Comunicador em background e
    redireciona sua saídas para /dev/null quando executada através de uma thread
    para limitar o consumo de memória.
    """

    COM = ['/usr/bin/nohup /opt/sefaz/cco/bin/runcco-ser.sh > /dev/null 2>&1 &']
    MON = ['/usr/bin/nohup /opt/sefaz/cco/bin/runcco-mon.sh > /dev/null 2>&1 &']


    threads = [
        t.Thread(target=run(COM, shell=True), name='up'),
        t.Thread(target=run(MON, shell=True), name='up')
    ]

    [th.start() for th in threads] 
    [th.join() for th in threads]



if __name__ == '__main__':

    # O stdout do check_up(), ou seja, o output do run executado sempre será
    # em string, se o comunicador não estiver em execução o run() retornará
    # um string vazia, caso contrário o retorno será uma string com o PID do
    # processo.

    # Loop que verifica se o Comunicador está UP
    while True:
        if not check_up():
            up()

        sleep(10)
```
