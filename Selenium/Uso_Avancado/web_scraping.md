# Web Scraping com SeleniumBase

Uma das grandes vantagens do SeleniumBase (e do Selenium em geral) é a capacidade de "scraping" de dados, ou seja, extrair informações estruturadas de páginas web. Neste guia, veremos como utilizar o SeleniumBase para extrair dados de páginas web de forma eficiente.

## Conceitos Básicos de Web Scraping

Web scraping é a técnica de extrair dados de sites da web de forma automatizada. Com o SeleniumBase, podemos:

1. Navegar por páginas web
2. Localizar elementos específicos
3. Extrair dados desses elementos (texto, atributos, etc.)
4. Processar e armazenar esses dados

## Exemplo: Coleta de Nomes e Preços de Produtos

Imaginemos uma listagem de produtos em uma página. Cada produto tem o seguinte HTML simplificado:

```html
<div class="produto">
  <h2 class="produto-nome">Nome do Produto</h2>
  <span class="produto-preco">R$ 99,90</span>
</div>
```

Para extrair os nomes e preços de todos os produtos, podemos usar o seguinte código:

```python
from seleniumbase import BaseCase
import time

class ProdutosScraper(BaseCase):
    def test_coletar_produtos(self):
        # Abrir a página de produtos
        self.open("https://www.exemplo.com/produtos")

        # Esperar o carregamento da página
        time.sleep(2)  # Melhor usar wait_for_element para páginas reais

        # Encontrar todos os produtos
        produtos = self.find_elements(".produto")

        # Listas para armazenar os dados coletados
        nomes = []
        precos = []

        # Extrair os dados de cada produto
        for produto in produtos:
            # Obter o nome do produto
            nome_elemento = produto.find_element_by_css_selector(".produto-nome")
            nome = nome_elemento.text
            nomes.append(nome)

            # Obter o preço do produto
            preco_elemento = produto.find_element_by_css_selector(".produto-preco")
            preco = preco_elemento.text
            precos.append(preco)

        # Mostrar os resultados
        print(f"Foram encontrados {len(nomes)} produtos:")
        for i in range(len(nomes)):
            print(f"Produto: {nomes[i]} - Preço: {precos[i]}")

        # Opcional: Salvar os dados em um arquivo CSV
        self.salvar_csv(nomes, precos)

    def salvar_csv(self, nomes, precos):
        import csv

        with open('produtos.csv', 'w', newline='', encoding='utf-8') as arquivo:
            writer = csv.writer(arquivo)
            # Escrever cabeçalho
            writer.writerow(['Nome', 'Preço'])
            # Escrever dados
            for i in range(len(nomes)):
                writer.writerow([nomes[i], precos[i]])

        print("Dados salvos em produtos.csv")
```

## Técnicas Avançadas de Web Scraping

### 1. Navegação por Múltiplas Páginas (Paginação)

```python
def test_scraping_com_paginacao(self):
    self.open("https://www.exemplo.com/produtos")

    pagina_atual = 1
    max_paginas = 5  # Limite para não executar infinitamente

    todos_produtos = []

    while pagina_atual <= max_paginas:
        # Extrair produtos da página atual
        produtos = self.find_elements(".produto")

        for produto in produtos:
            nome = produto.find_element_by_css_selector(".produto-nome").text
            preco = produto.find_element_by_css_selector(".produto-preco").text
            todos_produtos.append({"nome": nome, "preco": preco})

        # Verificar se existe botão "Próxima página"
        if self.is_element_present("a.proxima-pagina"):
            self.click("a.proxima-pagina")
            pagina_atual += 1
            # Esperar a nova página carregar
            self.wait_for_element(".produto")
        else:
            # Não há mais páginas
            break

    print(f"Total de produtos coletados: {len(todos_produtos)}")
```

### 2. Extração de Dados de Tabelas

As tabelas HTML são estruturas comuns para exibir dados organizados:

```python
def test_extrair_tabela(self):
    self.open("https://www.exemplo.com/tabela")

    # Obter todas as linhas da tabela (exceto o cabeçalho)
    linhas = self.find_elements("table#dados tbody tr")

    dados_tabela = []

    for linha in linhas:
        # Obter as células da linha
        celulas = linha.find_elements_by_tag_name("td")

        # Extrair dados de cada célula
        id = celulas[0].text
        nome = celulas[1].text
        valor = celulas[2].text

        # Armazenar os dados
        dados_tabela.append({
            "id": id,
            "nome": nome,
            "valor": valor
        })

    # Processar os dados conforme necessário
    for item in dados_tabela:
        print(f"ID: {item['id']}, Nome: {item['nome']}, Valor: {item['valor']}")
```

### 3. Lidando com Conteúdo Dinâmico

Muitos sites modernos carregam conteúdo dinamicamente com JavaScript. Nesses casos, é importante usar esperas adequadas:

```python
def test_conteudo_dinamico(self):
    self.open("https://www.exemplo.com/dinamico")

    # Clicar em um botão que carrega conteúdo dinamicamente
    self.click("#carregar-mais")

    # Esperar pelo carregamento do conteúdo
    self.wait_for_element(".item-dinamico")

    # Agora podemos extrair os dados
    itens = self.find_elements(".item-dinamico")

    for item in itens:
        titulo = item.find_element_by_css_selector("h3").text
        descricao = item.find_element_by_css_selector("p").text
        print(f"Título: {titulo}")
        print(f"Descrição: {descricao}")
```

### 4. Extraindo Atributos de Elementos

Além do texto, muitas vezes precisamos extrair atributos como URLs de links ou imagens:

```python
def test_extrair_atributos(self):
    self.open("https://www.exemplo.com/galeria")

    # Encontrar todas as imagens
    imagens = self.find_elements("img.galeria-item")

    for img in imagens:
        # Extrair o URL da imagem
        url = img.get_attribute("src")
        # Extrair o texto alternativo
        alt = img.get_attribute("alt")
        print(f"Imagem: {alt}, URL: {url}")

    # Encontrar todos os links
    links = self.find_elements("a.produto-link")

    for link in links:
        # Extrair o URL do link
        href = link.get_attribute("href")
        texto = link.text
        print(f"Link: {texto}, URL: {href}")
```

## Boas Práticas para Web Scraping

1. **Respeite os Termos de Serviço** do site que você está acessando.

2. **Adicione Atrasos** para não sobrecarregar o servidor:

   ```python
   import time
   time.sleep(2)  # Espera 2 segundos entre ações
   ```

3. **Use Esperas Inteligentes** em vez de atrasos fixos sempre que possível:

   ```python
   self.wait_for_element("#resultados")
   ```

4. **Identifique-se** incluindo um User-Agent informativo:

   ```python
   options = {"user_agent": "MeuBot/1.0 (projeto-academico; contato@email.com)"}
   self.open("https://exemplo.com", options=options)
   ```

5. **Verifique a existência de uma API** pública que forneça os dados de que você precisa antes de fazer scraping.

6. **Implemente Tratamento de Erros** para lidar com mudanças na estrutura do site:

   ```python
   try:
       elemento = self.find_element(".produto-nome")
       nome = elemento.text
   except Exception as e:
       print(f"Erro ao extrair nome: {e}")
       nome = "Desconhecido"
   ```

7. **Armazene os Dados Extraídos** em formatos estruturados como CSV, JSON ou banco de dados.

O web scraping com SeleniumBase é uma ferramenta poderosa para automação de coleta de dados, permitindo extrair informações estruturadas de praticamente qualquer site, especialmente aqueles com conteúdo dinâmico que seria difícil de acessar com métodos mais simples de scraping.
