# Estrutura Básica de um Script com SeleniumBase

Em SeleniumBase, todo script de teste é estruturado como uma classe Python que herda de uma das **classes principais** da biblioteca. Existem duas classes principais que você pode utilizar: `BaseCase` e `SB`.

## 1. Classe BaseCase

Essa é a **classe base mais comum**, ideal para testes automatizados. Ela herda funcionalidades do `unittest.TestCase` e é usada com pytest.

```python
from seleniumbase import BaseCase

class MeuTeste(BaseCase):
    def test_exemplo(self):
        # Abrir uma página
        self.open("https://www.exemplo.com")

        # Interagir com elementos
        self.type("#campo-busca", "selenium")
        self.click("#botao-buscar")

        # Verificar resultados
        self.assert_element(".resultados")
        self.assert_text("Resultados para: selenium", "h1")
```

## 2. Classe SB

Essa é uma classe mais **flexível**, ideal para **scripts interativos ou não estruturados**. Muito útil em **Jupyter Notebooks** ou para automações pontuais.

```python
from seleniumbase import SB

# Criar uma instância do navegador
with SB(browser="chrome") as sb:
    # Abrir uma página
    sb.open("https://www.exemplo.com")

    # Interagir com elementos
    sb.type("#campo-busca", "selenium")
    sb.click("#botao-buscar")

    # Verificar resultados
    sb.assert_element(".resultados")
    sb.assert_text("Resultados para: selenium", "h1")
```

## Diferenças entre BaseCase e SB

Ambas as classes são ótimas para utilizar no SeleniumBase, mas possuem diferenças no momento de utilização e no modo de execução dos testes:

1. **Método de execução**:

   - A classe `BaseCase` é executada através do pytest (que já vem junto com o SeleniumBase)
   - Os scripts com a classe `SB` podem ser executados diretamente pelo terminal, com o comando padrão de execução do Python

2. **Casos de uso**:

   - A classe `BaseCase` é mais utilizada para testes formais e estruturados
   - A classe `SB` é mais adequada para scripts rápidos e projetos de web scraping

3. **Sintaxe**:
   - Na classe `BaseCase`, os comandos são precedidos por `self.`
   - Na classe `SB`, os comandos são precedidos pelo nome da instância, como `sb.`

## Exemplo de execução

Para executar um teste usando a classe `BaseCase`:

```bash
pytest meu_teste.py
```

Para executar um script usando a classe `SB`:

```bash
python meu_script.py

# ou

uv run meu_script.py
```
