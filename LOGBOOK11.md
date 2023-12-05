# Public-Key Infrastructure (PKI) Lab

## Task 1: Becoming a Certificate Authority (CA)

 O objetivo era emitir certificados digitais sem pagar a CAs comerciais.


1. **Preparação do ficheiro de configuração:**
   - Copiamos o arquivo de configuração padrão do OpenSSL, `openssl.cnf`, do diretório `/usr/lib/ssl` para o nosso diretório atual.
   - Modificamos o arquivo para se adequar às necessidades da CA, a descomentar a linha `unique_subject = no` na seção `[ CA_default ]`.

2. **Geração do nosso CA:**
   - Executamos o comando OpenSSL para gerar um certificado autoassinado para o nosso CA:
     ```bash
     openssl req -x509 -newkey rsa:4096 -sha256 -days 3650 \
                 -keyout ca.key -out ca.crt \
                 -subj "/CN=www.modelCA.com/O=Model CA LTD./C=US" \
                 -passout pass:dees
     ```
   - Isso resultou na criação de duas chaves: `ca.key` (chave privada) e `ca.crt` (certificado de chave pública).

   - Para verificar o conteúdo do certificado X509 e da chave RSA, utilizamos os comandos:
     ```bash
     openssl x509 -in ca.crt -text -noout
     openssl rsa -in ca.key -text -noout
     ```

Identificamos que o certificado era de uma CA através do campo `Basic Constraints` com `CA:TRUE`. Confirmamos que era autoassinado pela igualdade nos campos `Issuer` e `Subject`. No RSA, observamos o expoente público (e), expoente privado (d), módulo (n) e os números primos (p, q).

~~~bash
Labsetup $ openssl x509 -in ca.crt -text -noout
openssl rsa -in ca.key -text -noout

Certificate:
    Data:
        Serial Number:
            4c:f0:b8:63:86:f8:11:97:8d:d7:f2:2c:8c:f3:f7:2c:c4:b6:be:5a
        Issuer: CN = www.modelCA.com, O = Model CA LTD., C = US
        Validity
            Not Before: Dec  5 13:23:11 2023 GMT
            Not After : Dec  2 13:23:11 2033 GMT
        Subject: CN = www.modelCA.com, O = Model CA LTD., C = US
        Subject Public Key Info:
            Public Key Algorithm: rsaEncryption
                Public-Key: (4096 bit)
                Modulus: [omitted for brevity]
                Exponent: 65537 (0x10001)
        X509v3 extensions:
            X509v3 Basic Constraints: critical
                CA:TRUE

Private-Key: (4096 bit, 2 primes)
modulus: [omitido para brevidade]
publicExponent: 65537 (0x10001)
privateExponent: [omitido para brevidade]
prime1: [omitido para brevidade]
prime2: [omitido para brevidade]
exponent1: [omitido para brevidade]
exponent2: [omitido para brevidade]
coefficient: [omitido para brevidade]
~~~

## Task 2: Generating a Certificate Request for Your Web Server

O objetivo desta tarefa é gerar uma CSR para o nosso servidor `www.l08g082023.com`, incluindo nomes alternativos no certificado.


1. **Geração da Chave Pública/Privada e CSR:**
   - Executamos o seguinte command para gerar um par de chaves pública/privada e a CSR:
     ```bash
     openssl req -newkey rsa:2048 -sha256 \
                 -keyout server.key -out server.csr \
                 -subj "/CN=www.l08g082023.com/O=SeedServer Inc./C=US" \
                 -passout pass:dees
     ```
   - Isso cria a (`server.key`) e a CSR (`server.csr`).

   - Para verificar o conteúdo decodificado da CSR e da chave privada, usamos:
     ```bash
     openssl req -in server.csr -text -noout
     openssl rsa -in server.key -text -noout
     ```

     screenshot

Para abranger vários nomes de host no certificado, utilizamos a extensão SAN.

1. **Incluindo SAN na CSR:**
   - Modificamos o comando para adicionar nomes alternativos (SAN) na CSR.
     ```bash
     openssl req -newkey rsa:2048 -sha256 \
                 -keyout server.key -out server.csr \
                 -subj "/CN=www.l08g082023.com/O=SeedServer Inc./C=US" \
                 -addext "subjectAltName = DNS:www.l08g082023.com, DNS:www.seed-server.com, DNS:www.zacarias2023.com, DNS:www.bank32.com" \
                 -passout pass:dees
     ```

    screenshot

## Task 3: Generating a Certificate for your server

Nesta tarefa, nosso objetivo é assinar a CSR do servidor `www.l08g082023.com` com nossa própria CA para formar um certificado.

### Assinando a CSR com a CA:

   - Utilizamos o arquivo de configuração `myopenssl.cnf` da Task 1.

   - Para permitir que a extensão SAN da CSR seja copiada para o certificado final descomentamos a linha `copy_extensions = copy`

   - Com a CSR (`server.csr`) e as chaves da CA (`ca.crt` e `ca.key`), geramos o certificado para o servidor:
     ```bash
     openssl ca -in server.csr -out server.crt -config myopenssl.cnf \
                 -extensions v3_req -extfile myopenssl.cnf \
                 -policy policy_anything
     ```

   - Para confirmar que o certificado foi gerado corretamente e inclui os nomes alternativos, executamos:
     ```bash
     openssl x509 -in server.crt -text -noout
     ```

    screenshot

> Movemos os ficheiros `ca.crt` e `ca.key` para novos diretorios dentro do demoCA. As linhas no `myopenssl.cnf` que apontavam para `./demoCA/private/cakey.pem` e `./demoCA/cacert.pem` foram atualizadas para o novo path.

Com esses passos, assinamos a CSR para `www.l08g082023.com` com nossa CA, gerando um certificado que inclui os nomes alternativos especificados. Tornando este certificado pronto para utilizar no nosso servidor.

