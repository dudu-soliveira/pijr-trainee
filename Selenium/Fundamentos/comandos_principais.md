# Comandos Principais do SeleniumBase

Os comandos básicos serão os mesmos, independentemente da classe que você esteja usando. A única diferença é o método que usamos para chamar:

- Na classe `BaseCase`, usamos `self.` antes do comando
- Na classe `SB`, usamos `sb.` (ou o nome que você deu à instância)

A seguir, os comandos serão mostrados considerando a classe `BaseCase` (com `self.`).

## Navegação

### `self.open(url)`

Abre uma URL no navegador.

```python
self.open("https://google.com")
```

### `self.refresh()`

Atualiza a página atual (como apertar F5).

```python
self.refresh()
```

Bom para testes que precisam simular atualização ou mudanças de conteúdo em tempo real.

## Interação com Elementos

### `self.click(selector)`

Clica em um botão ou link referenciado por seu seletor HTML.

```python
self.click("button[type='submit']")
```

### `self.type(selector, text)`

Digita algo em um campo de texto. É preciso especificar o elemento em que será feita a digitação.

```python
self.type("#username", "meu_usuario")
self.type("#senha", "minha_senha")
```

### `self.hover(selector)`

Simula o movimento do mouse sobre um elemento.

```python
self.hover(".menu-opcao")
```

Útil para revelar menus dropdown ou tooltips escondidos.

## Verificação de Elementos

### `self.assert_element(selector)`

Verifica se o elemento está presente na página.

```python
self.assert_element("input[name='email']")
```

### `self.assert_text(text, selector)`

Confirma que um determinado texto está visível no elemento indicado.

```python
self.assert_text("Login realizado com sucesso!", ".mensagem")
```

Além do `assert_text`, existem outros vários métodos de verificação para os mais diversificados elementos, como por exemplo o `assert_link`, que verifica links, dentre outros.

## Extração de Dados

### `self.get_text(selector)`

Extrai e retorna o texto de um elemento.

```python
nome = self.get_text(".produto-nome")
print(f"Produto encontrado: {nome}")
```

Muito útil para validações dinâmicas ou automações de scraping.

## Esperas (Waits)

### `self.sleep(segundos)`

Pausa o script por um número de segundos (útil em testes visuais ou debugging).

```python
self.sleep(2)  # pausa por 2 segundos
```

Evite usar em produção — prefira os métodos `wait_for_*`, que são mais inteligentes.

### `self.wait_for_element(selector, timeout=10)`

Espera até que um elemento esteja visível, com tempo limite.

```python
self.wait_for_element(".loader-fim")
```

Excelente para lidar com elementos carregados via JavaScript.

### `self.wait_for_element_absent(selector, timeout=10)`

Espera até que um elemento desapareça da página.

```python
self.wait_for_element_absent(".spinner")
```

Usado para aguardar fim de carregamentos ou transições.

### `self.wait_for_url(url)`

Espera até a URL mudar para o valor indicado.

```python
self.wait_for_url("https://exemplo.com/dashboard")
```

Perfeito após cliques ou formulários que redirecionam o usuário.

## Captura de Tela

### `self.save_screenshot("nome_arquivo.png")`

Tira um print da página atual.

```python
self.save_screenshot("tela_login.png")
```

Ideal para relatórios ou verificar visualmente o que está acontecendo.

## Importante

Por mais que tenham sido mostrados nos exemplos anteriores, devemos lembrar que os elementos que estamos manipulando variam de acordo com a formatação HTML do site e do propósito do projeto, portanto, os seletores nos comandos podem variar.
