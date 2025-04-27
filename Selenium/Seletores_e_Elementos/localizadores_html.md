# Localizadores e Estrutura HTML

Localizadores são maneiras de identificar elementos dentro de uma página web. Eles são fundamentais para o web scraping, pois permitem que você selecione precisamente os elementos que deseja extrair ou com os quais deseja interagir.

## Estrutura HTML básica

Antes de entrar em detalhes sobre cada um dos localizadores, vamos entender um pouco sobre a estrutura do HTML, que é uma linguagem de instruções responsável pela estruturação de uma página web.

> **Dica prática**: Para ver o código HTML de uma página web, você pode clicar com o botão direito do mouse em qualquer lugar da página e, depois, clicar em "Inspecionar" ou "Inspect Element".

### Tags HTML

As instruções em HTML vêm em "tags", que são envolvidas por `<>`, e a maioria das tags vem acompanhada por uma tag de fechamento. Por exemplo:

- A tag que indica um parágrafo é `<p>`
- A tag que indica um link é `<a>`

Exemplo de um parágrafo:

```html
<p>Isso é um parágrafo.</p>
```

Neste exemplo, `<p>` representa a tag de abertura e `</p>` representa a tag de fechamento.

### Atributos HTML

Dentro das tags, pode haver mais conteúdo, chamado de atributos, que é o que nos interessa para o web scraping. Atributos comuns são "id" e "class". Por exemplo:

```html
<div id="menu-principal" class="navegacao">
  <a href="https://www.exemplo.com">Link de exemplo</a>
</div>
```

Neste código HTML:

- `div` é a tag
- `id="menu-principal"` é um atributo id
- `class="navegacao"` é um atributo class
- A tag `div` contém uma tag `a` (link) que tem um atributo `href`

> **Observação**: A tag `<div>`, que aparece com muita frequência nos códigos HTML, não faz nada de mais, funcionando apenas como uma "caixa" que agrupa e organiza outros elementos da página web.

## Tipos de Localizadores

### 1. ID

O ID é um atributo único que deve identificar apenas um elemento na página. É o localizador mais confiável e rápido.

```html
<button id="botao-enviar">Enviar</button>
```

Para selecionar este elemento adicione uma hashtag antes do id:

```python
self.find_element("#botao-enviar")
```

### 2. Name

O atributo "name" é comumente usado em elementos de formulário.

```html
<input type="email" name="usuario-email" />
```

Para selecionar este elemento adicione `name=` antes do name:

```python
self.find_element("name=usuario-email")
```

### 3. Class Name

Classes podem ser aplicadas a múltiplos elementos, então este localizador pode retornar mais de um elemento.

```html
<div class="produto-card">Produto 1</div>
<div class="produto-card">Produto 2</div>
```

Para selecionar estes elementos adicione um ponto antes do nome da classe:

```python
# Retorna o primeiro elemento com a classe "produto-card"
self.find_element(".produto-card")

# Retorna todos os elementos com a classe "produto-card"
self.find_elements(".produto-card")
```

### 4. CSS Selector

Seletores CSS são poderosos e flexíveis, permitindo selecionar elementos com base em diversos critérios.

```html
<div id="id-container" class="container">
  <ul class="lista-produtos">
    <li class="item">Produto A</li>
    <li class="item destaque">Produto B</li>
  </ul>
</div>
<input type="email" />
```

Exemplos de seletores CSS:

```python
# Seleciona por ID
self.find_element("#id-container")

# Seleciona por classe
self.find_element(".item")

# Seleciona elementos com múltiplas classes
self.find_element(".item.destaque")

# Seleciona elementos filho
self.find_element(".lista-produtos > li")

# Seleciona por atributo
self.find_element("input[type='email']")
```

### 5. XPath

XPath é uma linguagem para navegar na estrutura XML/HTML. É extremamente poderoso, mas pode ser mais complexo e ligeiramente mais lento.

```html
<div class="container">
  <section>
    <article>
      <h2>Título do Artigo</h2>
      <p>Conteúdo do artigo...</p>
    </article>
  </section>
</div>
```

Exemplos de XPath:

```python
# Seleciona um elemento por sua tag
self.find_element("//h2")

# Seleciona um elemento por atributo
self.find_element("//div[@class='container']")

# Seleciona usando relacionamentos (pai, filho, irmão)
self.find_element("//section/article/h2")

# Seleciona por texto contido no elemento
self.find_element("//h2[contains(text(), 'Título')]")

# Seleciona elementos usando funções mais complexas
self.find_element("//p[string-length(text()) > 10]")
```

## Como encontrar os valores dos localizadores

### Para ID, Name e Class Name

Inspecione o elemento na página e procure pelo atributo correspondente no código HTML.

### Para CSS Selector e XPath

1. Clique com o botão direito no elemento desejado
2. Selecione "Inspecionar" ou "Inspect Element"
3. No painel de desenvolvimento, clique com o botão direito no elemento destacado
4. Para CSS Selector: selecione "Copy" → "Copy selector"
5. Para XPath: selecione "Copy" → "Copy XPath"

## Funções para selecionar elementos

No SeleniumBase (e Selenium), existem duas funções principais para selecionar elementos:

### `find_element(value, by="selector")`

Retorna o primeiro elemento que corresponde ao localizador especificado.

```python
# Encontra o primeiro elemento com a classe "produto"
elemento = self.find_element("produto", by="class name")
```

### `find_elements(value, by="selector")`

Retorna uma lista com todos os elementos que correspondem ao localizador.

```python
# Encontra todos os elementos com a tag "a" (links)
links = self.find_elements("a", by="tag name")

# Você pode iterar sobre os elementos encontrados
for link in links:
    print(link.get_attribute("href"))
```
