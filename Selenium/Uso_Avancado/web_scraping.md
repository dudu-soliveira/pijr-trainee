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
        self.open("http://quotes.toscrape.com/")
        time.sleep(2)

        # Esperar o carregamento da página
        self.wait_for_element(".quote")
        time.sleep(2)

        # Encontrar todos os quotes (substituindo "produtos")
        quotes = self.find_elements(".quote")

        # Listas para armazenar os dados coletados
        textos = []
        autores = []

        # Extrair os dados de cada quote
        for i in range(len(quotes)):
            # Obter o texto do quote usando seletor específico
            texto_elemento = self.find_element(f".quote:nth-of-type({i+1}) .text")
            texto = texto_elemento.text
            textos.append(texto)

            # Obter o autor do quote
            autor_elemento = self.find_element(f".quote:nth-of-type({i+1}) .author")
            autor = autor_elemento.text
            autores.append(autor)

        # Mostrar os resultados
        print(f"Foram encontrados {len(textos)} quotes:")
        for i in range(len(textos)):
            print(f"Quote: {textos[i]} - Autor: {autores[i]}")

        # Opcional: Salvar os dados em um arquivo CSV
        self.salvar_csv(textos, autores)

    def salvar_csv(self, textos, autores):
        import csv

        with open('quotes.csv', 'w', newline='', encoding='utf-8') as arquivo:
            writer = csv.writer(arquivo)
            # Escrever cabeçalho
            writer.writerow(['Texto', 'Autor'])
            # Escrever dados
            for i in range(len(textos)):
                writer.writerow([textos[i], autores[i]])

        print("Dados salvos em quotes.csv")          
```

## Técnicas Avançadas de Web Scraping

### 1. Navegação por Múltiplas Páginas (Paginação)

```python
def test_scraping_com_paginacao(self):
    self.open("http://quotes.toscrape.com/")
    time.sleep(2)

    pagina_atual = 1
    max_paginas = 3  # Limite para não executar infinitamente

    todos_quotes = []

    while pagina_atual <= max_paginas:
        # Extrair quotes da página atual
        quotes = self.find_elements(".quote")

        for i in range(len(quotes)):
            texto = self.find_element(f".quote:nth-of-type({i+1}) .text").text
            autor = self.find_element(f".quote:nth-of-type({i+1}) .author").text
            todos_quotes.append({"texto": texto, "autor": autor})

        # Verificar se existe botão "Próxima página"
        if self.is_element_present("li.next a"):
            self.click("li.next a")
            pagina_atual += 1
            time.sleep(2)
            # Esperar a nova página carregar
            self.wait_for_element(".quote")
        else:
            # Não há mais páginas
            break

    print(f"Total de quotes coletados: {len(todos_quotes)}")
    
    # Salvar em CSV
    import csv
    with open('quotes_multiplas_paginas.csv', 'w', newline='', encoding='utf-8') as arquivo:
        writer = csv.DictWriter(arquivo, fieldnames=['texto', 'autor'])
        writer.writeheader()
        writer.writerows(todos_quotes)
    
    print("Dados salvos em quotes_multiplas_paginas.csv")
```

### 2. Extração de Dados de Tabelas

As tabelas HTML são estruturas comuns para exibir dados organizados:

```python
def test_extrair_tabela(self):
    self.open("https://the-internet.herokuapp.com/tables")
    time.sleep(2)

    # Obter todas as linhas da tabela (exceto o cabeçalho)
    linhas = self.find_elements("#table1 tbody tr")

    dados_tabela = []

    for i in range(len(linhas)):
        # Obter as células da linha específica
        sobrenome = self.find_element(f"#table1 tbody tr:nth-of-type({i+1}) td:nth-of-type(1)").text
        nome = self.find_element(f"#table1 tbody tr:nth-of-type({i+1}) td:nth-of-type(2)").text
        email = self.find_element(f"#table1 tbody tr:nth-of-type({i+1}) td:nth-of-type(3)").text
        valor = self.find_element(f"#table1 tbody tr:nth-of-type({i+1}) td:nth-of-type(4)").text
        website = self.find_element(f"#table1 tbody tr:nth-of-type({i+1}) td:nth-of-type(5)").text

        # Armazenar os dados
        dados_tabela.append({
            "sobrenome": sobrenome,
            "nome": nome,
            "email": email,
            "valor": valor,
            "website": website
        })

    # Processar os dados conforme necessário
    for item in dados_tabela:
        print(f"Nome: {item['nome']} {item['sobrenome']}, Email: {item['email']}, Valor: {item['valor']}")
    
    # Salvar em CSV
    import csv
    with open('dados_tabela.csv', 'w', newline='', encoding='utf-8') as arquivo:
        writer = csv.DictWriter(arquivo, fieldnames=['sobrenome', 'nome', 'email', 'valor', 'website'])
        writer.writeheader()
        writer.writerows(dados_tabela)
    
    print("Dados salvos em dados_tabela.csv")
```

### 3. Lidando com Conteúdo Dinâmico

Muitos sites modernos carregam conteúdo dinamicamente com JavaScript. Nesses casos, é importante usar esperas adequadas:

```python
def test_conteudo_dinamico(self):
    self.open("https://the-internet.herokuapp.com/dynamic_loading/2")
    time.sleep(2)

    # Clicar em um botão que carrega conteúdo dinamicamente
    self.click("#start button")
    time.sleep(2)

    # Esperar pelo carregamento do conteúdo
    self.wait_for_element_visible("#finish h4")
    time.sleep(2)

    # Agora podemos extrair os dados
    resultado = self.find_element("#finish h4")
    texto = resultado.text
    print(f"Texto carregado dinamicamente: {texto}")
    
    # Salvar resultado em CSV
    import csv
    with open('conteudo_dinamico.csv', 'w', newline='', encoding='utf-8') as arquivo:
        writer = csv.writer(arquivo)
        writer.writerow(['Texto Dinâmico'])
        writer.writerow([texto])
    
    print("Dados salvos em conteudo_dinamico.csv")
```

### 4. Extraindo Atributos de Elementos

Além do texto, muitas vezes precisamos extrair atributos como URLs de links ou imagens:

```python
def test_extrair_atributos(self):
    self.open("https://the-internet.herokuapp.com/broken_images")
    time.sleep(2)

    # Encontrar todas as imagens
    imagens = self.find_elements("img")
    dados_imagens = []

    for i in range(len(imagens)):
        # Extrair o URL da imagem específica
        img = self.find_element(f"img:nth-of-type({i+1})")
        url = img.get_attribute("src")
        print(f"URL da imagem {i+1}: {url}")
        dados_imagens.append({"numero": i+1, "url": url})

    # Exemplo com links
    self.open("https://the-internet.herokuapp.com/status_codes")
    time.sleep(2)

    # Encontrar todos os links
    links = self.find_elements("ul li a")
    dados_links = []

    for i in range(len(links)):
        # Extrair o URL do link específico
        link = self.find_element(f"ul li:nth-of-type({i+1}) a")
        href = link.get_attribute("href")
        texto = link.text
        if href and texto:  # Apenas links válidos
            print(f"Link: {texto}, URL: {href}")
            dados_links.append({"texto": texto, "url": href})
    
    # Salvar imagens em CSV
    import csv
    with open('imagens_extraidas.csv', 'w', newline='', encoding='utf-8') as arquivo:
        writer = csv.DictWriter(arquivo, fieldnames=['numero', 'url'])
        writer.writeheader()
        writer.writerows(dados_imagens)
    
    # Salvar links em CSV
    with open('links_extraidos.csv', 'w', newline='', encoding='utf-8') as arquivo:
        writer = csv.DictWriter(arquivo, fieldnames=['texto', 'url'])
        writer.writeheader()
        writer.writerows(dados_links)
    
    print("Dados salvos em imagens_extraidas.csv e links_extraidos.csv")
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
