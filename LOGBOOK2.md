# Trabalho realizado nas Semanas #2 e #3

## Identificação

- item1
- item2
- item3
- item4

## Catalogação
- A vulnerabilidade existia no Linux Kernel desde a versão 2.6.22, lançada em 2007, até a versão 4.8.3 lançada em 2016 em que foi 'descoberta' por Phil Oester.
- O patch lançado em 2016 não se mostrou suficiente e foi necessário um [novo patch](https://threatpost.com/flaw-found-in-dirty-cow-patch/129064/) em 2017.
- Nível de severidade: Alta [(CVSS 7.8)](https://nvd.nist.gov/vuln/detail/cve-2016-5195)


## Exploit
- A vulnerabilidade funciona por criar uma race condition na forma como o subsistema de memória do linux kernel lida com a feature de Copy-on-Write.
> - Race Condition - Quando dois ou mais processos tentam aceder a um recurso compartilhado e o resultado da operação depende da ordem em que os processos são executados.
> - Copy-on-Write - Quando um processo tenta aceder/escrever em um ficheiro o kernel cria uma cópia do ficheiro em memória virtual e permite que o processo escreva na cópia.

1. É criado um private mmap (cópia privada read-only de um ficheiro em memória), garantindo assim que o processo não pode escrever no ficheiro original, assumamos que seja (**/etc/passwd**).

2. São usadas duas threads, uma pede ao kernel para write o private mmap, e a outra é usada para fazer um `madvise()` para que o kernel faça um **MADV_DONTNEED** (a thread aconselha ao kernel a descartar o private mmap).
[](https://player.slideplayer.com/102/17450358/slides/slide_17.jpg)

- Usando a race condition, se conseguirmos que o kernel faça o **MADV_DONTNEED** antes do `write()` então o kernel descarta o private mmap e o processo escreve no ficheiro original. Desta forma seria possível alterar o ficheiro **/etc/passwd** e adicionar um novo user com privilégios de root.

## Ataques

- item1
- item2
- item3
- item4
