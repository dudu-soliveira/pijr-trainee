# Preenchimento de Campos de Texto

Para preencher campos de texto, como por exemplo usuário, senha ou formulários em geral, podemos usar o método `self.type(selector, texto)`. Este método encontra o campo específico pelo seletor fornecido e digita o texto desejado, como se fosse um usuário real interagindo.

## Parâmetros do método type()

- **selector**: Geralmente é um seletor CSS (por exemplo, `#username` para um elemento com ID "username").
- **text**: A string que você deseja inserir naquele campo.

## Limpando campos antes de digitar

Caso queira limpar o campo antes de digitar (quando o campo já vem preenchido com algum valor ou placeholder), você tem algumas opções:

- Usar `self.clear(selector)` antes de `self.type(selector, text)`, para limpar o conteúdo do campo.
- Ou usar `self.update_text(selector, text)`, que limpa e digita automaticamente.

## Exemplo de preenchimento de formulário

```python
from seleniumbase import BaseCase
import time

class TestFormulario(BaseCase):
    def test_preencher_form(self):
        # Abrir a página com o formulário
        self.open("https://the-internet.herokuapp.com/login")
        time.sleep(3)
        
        # Preencher os campos do formulário de login
        self.type("#username", "tomsmith")
        time.sleep(3)
        
        self.type("#password", "SuperSecretPassword!")
        time.sleep(3)
        
        # Clicar no botão de login
        self.click("button[type='submit']")
        time.sleep(3)
        
        # Verificar se o login foi bem-sucedido
        self.assert_text("You logged into a secure area!", "#flash")
        time.sleep(3)
        
        # Ir para página de inputs
        self.open("https://the-internet.herokuapp.com/inputs")
        time.sleep(3)
        
        # Preencher campo numérico
        self.type("input[type='number']", "1234")
        time.sleep(3)
        
        # Limpar e preencher novamente
        self.clear("input[type='number']")
        time.sleep(3)
        
        self.type("input[type='number']", "5678")
        time.sleep(3)
        
        # Ir para página de dropdown
        self.open("https://the-internet.herokuapp.com/dropdown")
        time.sleep(3)
        
        # Selecionar opção do dropdown
        self.select_option_by_text("#dropdown", "Option 2")
        time.sleep(3)
```

## Trabalhando com diferentes tipos de campos

### Campos de texto simples

```python
self.type("#nome", "João Silva")
```

### Campos de senha

```python
self.type("#senha", "minhasenha123")
```

### Áreas de texto (textareas)

```python
self.type("textarea#comentario", "Este é um comentário longo que está sendo digitado em uma área de texto maior.")
```

### Campos com máscara ou formatação especial

Para campos com máscaras (como telefone, CPF, data), o SeleniumBase geralmente consegue inserir o texto normalmente:

```python
self.type("#cpf", "123.456.789-00")
self.type("#data", "01/01/2023")
```

### Dicas para formulários complexos

1. **Identificando o seletor correto**: Use as ferramentas de desenvolvedor do navegador (F12) para identificar o seletor correto de cada campo.

2. **Campos dinâmicos**: Alguns campos podem aparecer ou desaparecer dependendo de outras seleções. Use `wait_for_element()` antes de tentar preencher.

3. **Validação em tempo real**: Alguns formulários validam os campos enquanto você digita. Você pode verificar mensagens de erro:

   ```python
   self.type("#email", "email_invalido")
   self.assert_element_visible(".erro-email")
   ```

4. **Formulários em múltiplas etapas**: Para formulários com várias páginas ou etapas:

   ```python
   # Etapa 1
   self.type("#nome", "João Silva")
   self.click("#proximo")

   # Esperar carregar a etapa 2
   self.wait_for_element("#endereco")

   # Etapa 2
   self.type("#endereco", "Rua das Flores, 123")
   ```
