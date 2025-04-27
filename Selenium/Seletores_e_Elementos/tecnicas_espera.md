# Técnicas de Espera (Waits)

Para tornar os testes e as automações mais robustos, precisamos lidar com diferentes situações de carregamento de página, elementos que aparecem ou somem dinamicamente e até redirecionamentos de URL. Em SeleniumBase, temos métodos que nos ajudam com isso de forma mais inteligente do que usar `sleep()` (pausa fixa).

## Por que usar esperas inteligentes?

Usar uma espera fixa (como `self.sleep(5)`) pode:

1. Tornar seu teste desnecessariamente lento (se o elemento aparecer antes)
2. Fazer seu teste falhar (se o elemento demorar mais que o esperado)

As esperas inteligentes (ou "waits") aguardam apenas o tempo necessário, tornando os testes mais rápidos e confiáveis.

## Principais métodos de espera

### `self.wait_for_element(selector, timeout=10)`

Espera até que um elemento esteja visível, com tempo limite definido.

```python
# Espera até 10 segundos para que o elemento apareça
self.wait_for_element(".conteudo-principal")
```

### `self.wait_for_element_present(selector, timeout=10)`

Espera que um elemento esteja presente no DOM (pode estar visível ou não).

```python
# Espera até 10 segundos para que o elemento exista no DOM
self.wait_for_element_present("#elemento-oculto")
```

### `self.wait_for_element_visible(selector, timeout=10)`

Similar ao `wait_for_element`, espera até que um elemento esteja visível.

```python
# Espera até que o pop-up fique visível
self.wait_for_element_visible(".modal-popup")
```

### `self.wait_for_element_absent(selector, timeout=10)`

Espera até que um elemento desapareça da página.

```python
# Espera até que o indicador de carregamento desapareça
self.wait_for_element_absent(".loading-spinner")
```

### `self.wait_for_element_not_visible(selector, timeout=10)`

Espera até que um elemento exista, mas não esteja visível.

```python
# Espera até que o menu suspenso fique oculto
self.wait_for_element_not_visible(".dropdown-menu")
```

### `self.wait_for_text(text, selector="html", timeout=10)`

Espera até que um texto específico apareça dentro de um elemento.

```python
# Espera até que a mensagem de sucesso apareça
self.wait_for_text("Operação concluída com sucesso", ".message")
```

### `self.wait_for_attribute(selector, attribute, value, timeout=10)`

Espera até que um elemento tenha um atributo específico com um valor específico.

```python
# Espera até que o botão se torne habilitado
self.wait_for_attribute("button#enviar", "disabled", "false")
```

### `self.wait_for_url(url, timeout=10)`

Espera até a URL mudar para o valor indicado.

```python
# Espera até ser redirecionado para a página de dashboard
self.wait_for_url("https://exemplo.com/dashboard")
```

### `self.wait_for_url_to_contain(substring, timeout=10)`

Espera até que a URL contenha uma substring específica.

```python
# Espera até que a URL contenha a palavra "resultado"
self.wait_for_url_to_contain("resultado")
```

## Exemplo prático

```python
from seleniumbase import BaseCase

class TestEsperasInteligentes(BaseCase):
    def test_formulario_dinamico(self):
        # Abre a página com formulário
        self.open("https://exemplo.com/formulario")

        # Espera até que o formulário seja carregado completamente
        self.wait_for_element("#form-container")

        # Preenche o primeiro campo
        self.type("#email", "usuario@teste.com")

        # Clica em um botão que faz aparecer campos adicionais
        self.click("#mostrar-mais-campos")

        # Espera até que os campos adicionais apareçam
        self.wait_for_element_visible("#campos-adicionais")

        # Preenche campo adicional
        self.type("#telefone", "98765-4321")

        # Envia o formulário
        self.click("#enviar")

        # Espera aparecer um spinner de processamento
        self.wait_for_element_visible(".processando")

        # Espera o spinner desaparecer
        self.wait_for_element_absent(".processando")

        # Verifica se apareceu mensagem de sucesso
        self.wait_for_text("Formulário enviado com sucesso!", ".mensagem")
```

## Quando usar cada tipo de espera

1. **Para páginas sendo carregadas**: Use `wait_for_element()` para aguardar um elemento que indique que a página foi carregada.

2. **Para conteúdo dinâmico**: Use `wait_for_text()` para aguardar texto que aparece dinamicamente.

3. **Para carregamentos e processamentos**: Use `wait_for_element_absent()` para aguardar que indicadores de carregamento desapareçam.

4. **Para redirecionamentos**: Use `wait_for_url()` ou `wait_for_url_to_contain()` para aguardar redirecionamentos.

5. **Para verificar estados de elementos**: Use `wait_for_attribute()` para aguardar mudanças de estado (como um botão ser habilitado).

As técnicas de espera inteligente são essenciais para criar testes robustos que funcionam consistentemente, mesmo com variações no tempo de carregamento ou com elementos que mudam dinamicamente.
