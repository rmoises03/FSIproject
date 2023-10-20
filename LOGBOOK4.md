**Task 1: **
- Utilizamos o printenv e o env para dar “print” às variáveis de ambiente. Utilizamos também o  export e o unset  para dar “set” e “unset” nas mesmas variáveis de ambiente.

**Task 2: **
- Comparando as diferenças de ambos os files, usando o comando “diff”, percebemos que não há diferenças. 

![image 1](docs/images/Imagem1.png)

**Task 3: **
- Executando essa alteração, observamos que o programa executado terá acesso às variáveis de ambiente do programa original. Concluímos assim, que as variáveis de ambiente não são passadas automaticamente usando “execve()”, tendo estas de serem passadas por argumentos.

**Task 4: **
- Usando o “system()” ao invés do “execve()”, temos acesso às variáveis de ambiente do sistema completo, em vez de ser só as de utilizador.

**Task 5: **
- Ao contrário do espectável, apenas as variáveis PATH e ANY_NAME estavam presentes, faltando assim a LD_LIBRARY_PATH, que não estava disponível por questões de segurança.

**Task 6: **
- Foi possível alterar o “PATH” para o programa, de forma a que ao executar o “a.out”, este executasse o nosso ficheiro “ls” que dava “print” em “You just got hacked!”, em vez de executar o “system(“ls”). Desta forma o códico malicioso está a correr com “root privilegie”. Percebemos que quando corremos outro terminal sem as mesmas funcionalidades de segurança, o programa corre com privilégios de "root".

![image 2](docs/images/1af0b6f8-cedf-4f9b-938f-ac3c58a0d097.jpg)
