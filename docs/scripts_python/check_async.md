---
hide:
  - footer
---

# Check Async
Script python para checagem e verificação de atividade para o Software monitor e
comunicador da SEFAZ
___

## O problema

Inicialmente tivemos um problema relacionado aos drives do MFE (Módulo Fiscal
Eletrônico) onde o mesmo ficava constantemente offline devido a queda dos
serviços do comunicador e monitor, porém, os mesmos ficavam up com a execução
manual via shell.

O script do comunicador é responsável por estabelecer a comunicação do o MFE e o
script monitor é responsável por retornar um status visual do estado do módulo,
ou seja, se encontra-se conectado ou não ao computador.
___

## Solução

Como solução, trouxe um script em python onde o mesmo teria duas versões
visando a falta de padronização nos sistemas linux (como ubuntu 16.04 e 18.04,
Zorin 14.04 e 16.04).

> Nesse artigo trataremos do script adaptado como __Solução para o python3.9+__.

A solução abordada foi o script `check_async.py`, que basicamente utiliza
bibliotecas como `subprocess`, `asyncio` e `psutil`:

* __Subprocess:__ A biblioteca subprocess é utilizada para realizar processos
  UNIX no python (como o up dos scripts que são feitos em shell).

* __AsyncIO:__ É uma biblioteca utilizada para proporcionar o processamento
assíncrono, com threads e de forma concorrente. 

* __Psutil:__ Utilizada para verificar se o processo está rodando.

```{.py3 hl_lines='1 2 3' linenums="3" title="check_async.py"}
import subprocess
from asyncio import run, sleep, gather, to_thread
from psutil import process_iter
```

Essa abordagem utiliza uma programação assíncrona através da biblioteca asyncio
e com seus recursos tornou mais eficaz a utilização de threads distintas sendo
lançadas na pool de processos de forma concorrente e não sequencial.

___

## Análise por trecho de código - Arquivo de Constantes
O arquivo de constantes é composto por duas variáveis do tipo `dict`, onde a
chave é o nome do processo ao qual usaremos para rastrear a atividade do
mesmo e o valor é comando de execução utilizado para iniciar o processo.

```{.py3 hl_lines='' linenums="1" title="constant.py"}
COMUNICADOR = {
    'Comunicador': 
    '/usr/bin/nohup /opt/sefaz/cco/bin/runcco-ser.sh > /dev/null 2>&1 &'
    }

MONITOR = {
    'Monitor':
    '/usr/bin/nohup /opt/sefaz/cco/bin/runcco-mon.sh > /dev/null 2>&1 &'
    }
```

> A expressão `>` serve para direcionar o retorno do script executado para o
> destino `/dev/null 2>&1 &`, onde o mesmo será um destino nulo no sistema.

___

## Análise por trecho de código - Arquivo Check Async
De início temos a função `up_down()` que como o nome sugere, verifica se o
script está ativo ou não. Para simplificar, iremos iterar em todos os processos 
rodando no sistema através da biblioteca `psutil` e filtrar pelo nome dos 
processo através da seguinte expressão: `process_iter(['name'])`. Mas  antes 
disso vamos instanciar as constantes e desempacota-las nas variáveis `key` 
e `values` pela expressão `for key, value in process.items():`.

```{.py3 hl_lines="5" linenums="9" title="check_async.py"}
def up_down(process):
    """
    Função responsável por identificar se o processo em palta está em execução.
    """
    for key, value in process.items():
        for processo in process_iter(['name']):
            if processo.info['name'] == key:
                return True
        subprocess.run(value, shell=True)
```

Agora foi adicionado um segundo `for` para enfim podermos iterar em cada
processo com a biblioteca `psutil`. Para isso utilizamos a seguinte expressão
`for processo in process_iter(['name']):`.

```{.py3 hl_lines="6" linenums="9" title="check_async.py"}
def up_down(process):
    """
    Função responsável por identificar se o processo em palta está em execução.
    """
    for key, value in process.items():
        for processo in process_iter(['name']):
            if processo.info['name'] == key:
                return True
        subprocess.run(value, shell=True)
```

Após o desempacotamento das constantes e processos, é necessário apenas criar
uma condicional que se encarregue de verificar se ambos os nomes  de processos
(chave da constante e nome do processo extraído com `psutil`) são iguais.

```{.py3 hl_lines="7 8" linenums="9" title="check_async.py"}
def up_down(process):
    """
    Função responsável por identificar se o processo em palta está em execução.
    """
    for key, value in process.items():
        for processo in process_iter(['name']):
            if processo.info['name'] == key:
                return True
        subprocess.run(value, shell=True)
```

Caso passe na condicional, o script estará em execução,  mas se esse não for o 
caso, então o mesmo será iniciado pelo `subprocess` que irá utilizar como 
parâmetro a variável `value`para iniciar o processo em análise.

```{.py3 hl_lines="9" linenums="9" title="check_async.py"}
def up_down(process):
    """
    Função responsável por identificar se o processo em palta está em execução.
    """
    for key, value in process.items():
        for processo in process_iter(['name']):
            if processo.info['name'] == key:
                return True
        subprocess.run(value, shell=True)
```

Em seguida veremos a função `main()`, que tem como finalidade utilizar a 
biblioteca AsyncIO para executar a cada 10 segundos duas threads de forma 
concorrente e após uma análise individual realizar o start do script caso seja
necessário. 

```{.py3 hl_lines='6 7 8 9 10' linenums="3" title="check_async.py"}
async def main():
    """
    Função principal que executa as verificações/execuções como concorrências.
    """
    while True:
        await gather(
            to_thread(up_down, MONITOR),
            to_thread(up_down, COMUNICADOR)
            )
        await sleep(10)
```

> Definição dos termos abordados:
>
> - `async def`: expressão utilizada para definir uma função assíncrona.
> - `await`: expressão com sentido literal (aguarde). 
> - `gather`: método que executa tasks de forma concorrente.
> - `to_thread`: método que executa uma task em uma thread separada.

No `if __name__ == '__main__'` é inserido apenas a execução da função assíncrona
`main()` através do método `run()`.

> O siclo se reinicia a cada 10 segundos (Isso ocorre visando a queda do serviço 
> na hora da emissão, ou seja, dez segundos é tempo suficiente para cair e subir 
> o serviço antes do envio do cupom para a SEFAZ).

___

## Código completo

```{.py3 hl_lines='' linenums="3" title="check_async.py"}
#!/usr/bin/env python3.9

import subprocess
from asyncio import run, sleep, gather, to_thread
from psutil import process_iter
from constant import COMUNICADOR, MONITOR


def up_down(process):
    """
    Função responsável por identificar se o processo em palta está em execução.
    """
    for key, value in process.items():
        for processo in process_iter(['name']):
            if processo.info['name'] == key:
                return True
        subprocess.run(value, shell=True)


async def main():
    """
    Função principal que executa as verificações/execuções como concorrências.
    """
    while True:
        await gather(
            to_thread(up_down, MONITOR),
            to_thread(up_down, COMUNICADOR)
            )
        await sleep(10)


if __name__ == '__main__':
    run(main())
```