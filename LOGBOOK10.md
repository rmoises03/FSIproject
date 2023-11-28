# SEED Labs - Secret Key Encryption

## TASK 1

- Começamos por executar o ficheiro freq.py de forma a percebermos a frequência das letras/conjuntos de letras. De seguida vimos as palavras de três letras mais utilizadas na língua inglesa e subsituímos a palavra com mais frequência no freq.py (ytn) pela palavra de três letras mais utilizada na língua inglesa(the), através do código `` tr 'ytn' 'THE' < ciphertext.txt > out.txt ``.
- Fomos efetuando mais trocas até consguirmos perceber o texto.
- A últims troca que realizámos até ser completamente possível perceber o texto foi ```tr 'ytnvxupmhqidzcbarflges' 'THEAONDIRSLYUMFCGVWBPK' < ciphertext.txt > out.txt` ```.
- Este foi o primeiro parágrafo do texto "THE OSCARS TURN  ON SUNDAY WHICH SEEMS ABOUT RIGHT AFTER THIS LONG STRANGE
AWARDS TRIP THE BAGGER FEELS LIKE A NONAGENARIAN TOO"

## TASK 2

- Começamos por executar o comando ```man enc``` para percebermos melhor os diferentes tipose de criptografia.
- Experimentamos encriptar um fichiro de texto com os seguintes tipos de criptografia -aes-128-cbc, -bf-cbc,
-aes-128-cfb.

## TASK 3

- Começamos por separar a imagem em header e body.
- De seguida, encriptamos apenas o body pelo modo ECB com o comando ```openssl enc -aes-128-ecb -e -in body -out body2 -K  00112233445566778889aabbccddeeff```.
- Experimentamos visualizar a imagem depois de juntarmos o body com o header atraves do comando ```eog new.bmp```. Percebemos que esta encriptação permite continuarmos a percerber a imagem.
- Por fim encriptamos segundo o modo CBC com o comando ```openssl enc -aes-128-cbc -e -in body -out body2 -K 00112233445566778889aabbccddeeff -iv 0102030405060708```. Visuzalizando esta imagem, não era possível perceber o seu conteúdo.
- Concluímos que a segunda encriptação foi mais forte.


#CTF
