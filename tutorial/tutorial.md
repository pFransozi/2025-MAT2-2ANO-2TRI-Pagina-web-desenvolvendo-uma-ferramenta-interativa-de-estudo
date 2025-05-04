# Tutorial Progressivo — Construindo um Site de Flashcards  

Cada etapa acrescenta algo novo. Siga na ordem para chegar ao resultado final.

---

## 0 – Preparação do Ambiente

### Quando estou começando o projeto pela primeira vez

1. Criar uma pasta no computador, onde os arquivos que serão programados devem ser salvos. Por exemplo:

```bash
Documentos
├── trabalho-flashcards
└──── index.html          
```

2. Abrir o VSCODE na pasta `trabalho-flashcards`, para que os arquivos possam ser carregados no VSCODE.


### Quando já guardei algo do meu projeto no github
---

## 1 – Primeira Página HTML Mínima (`index.html`)

Lembre-se que HTML é um código de marcação porque utiliza tags (etiquetas) para construir sites. HTML significa Linguagem de Marcação de Hipertexto, sendo utilizada para gerar a estrutura de um site. 

Existem mais de 100 tags, as quais podem ser consultadas neste [endereço](https://www.w3schools.com/tags/).

O código a seguir deverá estar no arquivo `index.html`. Lembre-se que o HTML utiliza ``tags`` para programação e geração da estrutura do site. Geralmente, essas tags estão em pares. Por exemplo:

  * `<html lang="pt-BR">` e `</html>`
  * `<head>` e `<head>`
  * `<section id="container">` e `</section>`

Além disso, é importante lembrar que o HTML segue uma hierarquia na sua estrutura. Por exemplo, a tag body (`<body>` e ``</body>``) deve estar entre `<html lang="pt-BR">` e `</html>`.



```html
<!-- código deve estar no index.html -->
<!DOCTYPE html>
<html lang="pt-BR">
  <head>
    <meta charset="utf-8">
    <title>Flashcards de Estudo</title>
  </head>
  <body>
    <main>
      <section id="container">
        <!-- a maior parte do código será colocado aqui -->
      </section>
    </main>
  </body>
</html>
```
*A página ainda está vazia: tudo pronto para receber estilo e cards.*

*`<!-- a maior parte do código será colocado aqui -->`* essa linha é um comentário. Podemos utilizar comentários para deixar mensagens que não aparecerão no site final, apenas pode ser vista por quem acessa o código.

---

## 2 – Variáveis e Reset no CSS (`style.css`)

```css
/* Paleta de cores reutilizável */
:root{
  --text-color:#DBE4EF;
  --card-front:#144480;
  --card-back:#00F4BF;
}

body{
  margin:0;
  font-family:"Bai Jamjuree",sans-serif;
}
```
**Por que variáveis?** Assim você troca o tema inteiro alterando só uma linha.

---

## 3 – Layout do Container e Cartão
```css
#container{
  display:flex;
  flex-wrap:wrap;
  gap:3rem;
  padding:4rem;
  justify-content:space-between;
}

.cartao{
  height:20rem;
  flex:1 0 calc(33% - 6rem); /* 3 por linha em telas grandes */
}
```
Salve e recarregue — ainda não há cartões, mas o layout está pronto.

---

## 4 – Estrutura de **um** Cartão Estático
Dentro de `<section id="container">` adicione:
```html
<article class="cartao">
  <div class="cartao__conteudo">
    <h3>Programação</h3>
    <div class="cartao__conteudo__pergunta">
      <p>O que é JavaScript?</p>
    </div>
    <div class="cartao__conteudo__resposta">
      <p>JavaScript é uma linguagem de programação</p>
    </div>
  </div>
</article>
```

Estilize o conteúdo:
```css
.cartao__conteudo{
  background:var(--card-front);
  text-align:center;
  height:100%;
  transform-style:preserve-3d;
  transition:transform .3s ease-in-out;
}

.cartao__conteudo h3{
  color:var(--text-color);
  border:1px solid var(--text-color);
  margin:.6rem;
  padding:.5rem;
  border-radius:.6rem;
  font-size:3vw;
  backface-visibility:hidden;
}

.cartao__conteudo__pergunta p{
  color:var(--text-color);
  margin-top:4rem;
  font-size:3.8vw;
}
```

---

## 5 – Efeito de Virar o Cartão
```css
.cartao__conteudo__resposta{
  position:absolute;
  top:0;left:0;
  height:100%;width:100%;
  background:rgba(0,244,191,.2);
  border:4px solid var(--card-back);
  transform:rotateY(180deg);
  backface-visibility:hidden;
  display:flex;align-items:center;justify-content:center;
  color:var(--card-back);
  font-weight:700;
}

.cartao.active .cartao__conteudo{
  transform:rotateY(180deg);
}
```
*`preserve-3d` mantém frente e verso; `backface-visibility:hidden` evita espelhamento.*

---

## 6 – JavaScript para Criar e Virar Cartões (`app.js`)
```js
function criaCartao(categoria, pergunta, resposta){
  const container = document.getElementById("container");
  const cartao = document.createElement("article");
  cartao.className = "cartao";
  cartao.innerHTML = `
    <div class="cartao__conteudo">
      <h3>${categoria}</h3>
      <div class="cartao__conteudo__pergunta"><p>${pergunta}</p></div>
      <div class="cartao__conteudo__resposta"><p>${resposta}</p></div>
    </div>`;
  cartao.addEventListener("click", ()=>cartao.classList.toggle("active"));
  container.appendChild(cartao);
}
```
Agora temos a função, mas ainda não a chamamos.

---

## 7 – Banco de Perguntas Separado (`perguntas.js`)
```js
criaCartao("Programação","O que é Python?","Python é uma linguagem de programação");
criaCartao("Geografia","Capital da França?","Paris");
criaCartao("Inglês","Como se diz oi?","Hi");
```

No final de **index.html**:
```html
<script src="app.js"></script>
<script src="perguntas.js"></script>
```
Recarregue: os flashcards aparecem e viram ao clique!

---

## 8 – Imagens de Fundo Responsivas
Coloque `bg-desktop.webp`, `bg-tablet.webp`, `bg-mobile.webp` em **img/** e no topo do CSS:
```css
body{
  background:url('img/bg-desktop.webp') center/cover no-repeat fixed;
}

/* tablet */
@media (max-width:1024px){
  body{ background:url('img/bg-tablet.webp') center/cover fixed; }
}
/* mobile */
@media (max-width:560px){
  body{ background:url('img/bg-mobile.webp') center/cover fixed; }
}
```

---

## 9 – Rodapé
Em **index.html** após `</main>`:
```html
<footer>
  <p>Projeto desenvolvido pela turma – sem fins lucrativos</p>
</footer>
```
CSS:
```css
footer{
  background:#000;color:#fff;
  position:fixed;bottom:0;left:0;width:100%;
  height:2rem;
  display:flex;align-items:center;justify-content:center;
  font-size:.75rem;
}
```

---

## 10 – Desafios Finais
1. Ajuste para telas muito pequenas.  
2. Crie input para o usuário adicionar novos flashcards.  
3. Salve progresso com `localStorage`.

---

### Parabéns! 🎉
Você construiu o site do zero e entendeu cada parte do código. Sinta‑se livre para expandir!
