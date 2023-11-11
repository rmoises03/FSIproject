# LOGBOOK6

## CTF 6 - XSS + CSRF

### Sumário

Neste CTF tivemos de pedir a um **Admin** para nos dar a **Flag** através de um **form**. Or, encontrar e explorar o exploit.

---

### Páginas

#### Primeira Página (Página do Form)

Nesta página temos o id do pedido e um form com um input de text para submeter. Submetê-lo redireciona para outra página.

#### Segunda Página (Página de Submissão)

Nesta página podemos ver o estado do nosso pedido, o texto que submetemos e um link para a página do Administrador relacionada ao nosso pedido.

#### Terceira Página (Página do Administrador)

Nesta página existe um link que nos redireciona para a página anterior e dois butões desativados, um para aceitar o pedido e dar a **flag** e outro para rejeitá-lo.

### Construir o ataque

Inspecionando a página do admnin descubrimos o link do action que aprova o nosso pedido: ```request/{id}/approve```.

Depois de explorarmos um pouco mais a caixa de input da primeira página, observámos que não estava protegida contra XSS, então decidimos criar um form igual ao da página do admin para aparecer na página de submição um botão que podíamos clicar e nos dar a flag, usando o código html:

```html=!
<form method="POST" action="http://ctf-fsi.fe.up.pt:5005/request/{id}/approve" role="form">
    <div class="submit">
        <input type="submit" id="giveflag" value="Give the flag">
    </div>
</form>
```

Clicar o botão leva-nos para uma nova página, mas recebemos **403 Error - Forbidden**. Mas, reparámos que a página apareceu por um momento antes do erro aparecer.

Adicionando o atributo ```target = "_blank"``` ao **form** e o seguinte script depois do **form** consegue-nos a aprovação do pedido.

```javascript=
<script>document.forms[0].submit();</script>
```

Depois de colocarmos o código atualizado na caixa de entrada de texto e clicando no botão que aparece, chegamos outra vez à página com erro, no entanto, quando voltamos à página de submição a **flag** aparece no lugar a mensagem do estado do pedido.

---

## Como evitar ser atacado

Validar e sanitalizar todos os input de usúarios quer do lado do cliente quer do lado do servidor. Informação vinda dos usuários não é de confiança, incluindo campos dos forms e os parâmetros do URL.

Codificar as informações ao enviá-las para o navegador usando funções de codificação apropriadas com base no contexto, como por exemplo, usar htmlspecialchars() em PHP, escapeHTML() em JavaScript para evitar HTML indesejado, etc.

Ao utilizar cabeçalhos de CSP (Content Security Policy), controlar de onde os recursos podem ser carregados para evitar a execução de scripts inline. Dessa forma, o CSP ajuda a mitigar o impacto de ataques XSS especificando quais recursos podem ser carregados e executados.

Exemplo de header CSP:

```html!
Content-Security-Policy: default-src 'self'; script-src 'self' https://trusted-scripts.com;
```
