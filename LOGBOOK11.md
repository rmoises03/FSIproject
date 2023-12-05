# Task 1: Becoming a Certificate Authority (CA)

 O objetivo era emitir certificados digitais sem pagar a CAs.

## Passos Realizados

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
                Modulus: [omitido para brevidade]
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
