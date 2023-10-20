**Task 1:**
- Desligar as medidas de segurança para que as variáveis possam ser as mesmas.
- Ligar a shell ao /bin/sh para que o programa seja executado.
- Compilar e perceber que ambas as versões nos deram acesso à shell.

![image 1](docs/images/Screenshot_from_2023-10-20_11-29-16.png)


**Task 2:**
- Compilar o programa de forma a correr os comandos para tornar o programa Set-UID,

![image 2](docs/images/Screenshot_from_2023-10-20_11-39-09.png)

- Usando ls -l viu-se as permissões do programa confirmando o Set-UID.

![image 3](docs/images/Screenshot_from_2023-10-20_11-39-33.png)


**Task 3:**
- Inicialmente, corremos o programa no modo debug para percebermos a localização das variáveis na stack. Colocamos um breakpoint.
- Vimos de seguida, $ebp e o &buffer.

![image 4](docs/images/Screenshot_from_2023-10-20_12-03-45.png))
![image 5](docs/images/Screenshot_from_2023-10-20_14-02-37.png))

- Depois alteramos as seguintes varáveis no script de python:
- - Colocamos o start a 490 para o início do shellcode no buffer ser igual à posição para onde o programa salta.
- - Colocamos o offset a 112, que é a distância entre o início do buffer e a address do return.
- - Colocamos o ret a 0xffffcb48 + 4 + 200 para ser a address para onde queremos que o programa salte.

![image 6](docs/images/Screenshot_from_2023-10-20_11-39-33.png))

- Corremos o script e depois o programa e obtivemos uma shell com previlégios de root.


**Task 4:**


![image 7](docs/images/Screenshot_from_2023-10-20_14-15-00.png))









