# Client-Side Concepts

- Conceitos essenciais para client-side hacking.

- OBS -> Não é/foi feito para ser organizado ou ser um arquivo ultimato sobre os assuntos. Se trata apenas de um arquivo `.md` com várias anotações de conceitos importantes para client-side hacking.

## CSR (Client-Side Rendering)

- Renderização que ocorre no lado do cliente (browser).

- É a prática de gerar o conteúdo do HTML usando JavaScript no navegador.

- O servidor envia um arquivo HTML que vai conter o(s) javascript bundles que serão utilizados pelo navegador para baixar o conteúdo da página.

```
1. Usuário faz a requisição ao site (o browser faz a requisição HTTP ao servidor)
2. O servidor envia um arquivo HTML inicial que vai conter o(s) link(s) para os arquivos JavaScript bundles
3. O navegador faz o download dos arquivos JS e CSS enquanto faz o parser do HTML
4. O navegador executa o código JavaScript e começa a renderizar a aplicação no DOM
5. O JavasCript pode fazer chamadas de API para buscar dados no servidor (usando fetch, axios e etc...)
6. O navegador utiliza os dados buscados para construir/atualizar o UI no DOM. Existem bibliotecas/frameworks JavaScript que constroem e atualizam o DOM de forma dinâmica, usando técnicas tipo o Virtual DOM do React.
```

## DOM

- O Document Object Model (DOM), é uma interface de programação para componentes web. Representa a estrutura de uma página web como uma árvore de objetos, em que cada objeto representa uma parte da página. 

- Cada nó/node da árvore do DOM pode ser acessado utilizando o objeto `window.document` ou `document`. Por exemplo, podemos acessar o elemento `<p id="demo"></p>` da seguinte forma: `document.getElementById("demo")`.

- Para entender vulnerabilidades DOM-based é necessário compreender o conceito de `source` e `sink`: 
	- `source`: São basicamente os lugares em que o usuário consegue enviar o seu input
	- `sink`: São os lugares no código em que o input do usuário é inserido e/ou executado. Se o input não for devidamente sanitizado, pode se tornar perigoso.

- O conceito de análise taint flow pode ser usada para encontrar vulnerabilidades baseadas no DOM.

```
- exemplos de sources comuns:
location.href
location.search
location.hash
document.URL
document.referrer
window.name
localStorage, sessionStorage, cookies

- exemplos de sinks comuns:
innerHTML
outerHTML
document.write()
eval()
setAttribute()
innerText / textContent
window.location

- exemplo bem simples de um fluxo perigoso. URL -> http://127.0.0.1:1337/?xss=<img/src=x onerror=alert(1)>:
<script>
	const input = location.search
	const qs = new URLSearchParams(input).get("xss")
	document.body.innerHTML = qs
</script>
```

## CSP

- Mecanismo de segurança do navegador que define quais recursos uma página pode carregar ou executar. Utilizado como uma camada adicional de segurança que facilita a detecção e mitigação de certos tipos de ataques. Mesmo que seja possível executar um código malicioso, o CSP pode mitigar a sua execução.

- Resumidamente, o servidor informa ao navegador quais scripts, imagens, estilos e conexões são confiáveis. Se algo fora da política estabelecida tentar executar, o navegador bloqueia. 

- O CSP pode ser enviado ao navegador via o cabeçalho HTTP `Content-Security-Policy` ou dentro do elemento `<meta>` no HTML.

- As diretivas do CSP são regras que dizem ao navegador quais tipos de recursos podem ser carregados e de onde eles podem vir. Cada diretiva controla um tipo específico de recurso dentro da página.

```
Exemplo real de uma política de CSP utilizada pelo YouTube:

content-security-policy
	script-src 'unsafe-eval' 'self' 'unsafe-inline' https://www.google.com https://apis.google.com https://ssl.gstatic.com https://www.gstatic.com https://www.googletagmanager.com https://www.google-analytics.com https://*.youtube.com https://*.google.com https://*.gstatic.com https://youtube.com https://www.youtube.com https://google.com https://*.doubleclick.net https://*.googleapis.com https://www.googleadservices.com https://tpc.googlesyndication.com https://www.youtubekids.com https://www.youtube-nocookie.com https://www.youtubeeducation.com https://www-onepick-opensocial.googleusercontent.com;report-uri https://csp.withgoogle.com/csp/youtube_main/allowlist, require-trusted-types-for 'script'
```

## SOP

## CORS

## postMessage

## devtools

### TODO
