# Trabalhando com Componentes de Formulários

Além dos campos de texto simples, os formulários web podem conter vários outros tipos de componentes, como checkboxes, radio buttons, dropdowns e muito mais. Vamos ver como trabalhar com esses elementos usando o SeleniumBase.

## Seleção de Checkboxes

Os checkboxes são elementos de formulário que permitem ao usuário selecionar múltiplas opções ao mesmo tempo. Para interagir com checkboxes, podemos usar o método `self.click()` passando o seletor do checkbox.

```python
from seleniumbase import BaseCase
import time

class TestCheckboxes(BaseCase):
    def test_selecionar_checkbox(self):
        # Abrir a página com checkboxes
        self.open("https://the-internet.herokuapp.com/checkboxes")
        time.sleep(2)

        # Verificar estado inicial dos checkboxes
        # Checkbox 1 começa desmarcado
        # Checkbox 2 começa marcado
        self.assert_false(self.is_selected("input[type='checkbox']:nth-of-type(1)"))
        self.assert_true(self.is_selected("input[type='checkbox']:nth-of-type(2)"))

        # Selecionar o primeiro checkbox (que está desmarcado)
        self.click("input[type='checkbox']:nth-of-type(1)")
        time.sleep(2)

        # Verificar se o primeiro checkbox foi selecionado
        self.assert_true(self.is_selected("input[type='checkbox']:nth-of-type(1)"))

        # Desmarcar o segundo checkbox (que está marcado)
        self.click("input[type='checkbox']:nth-of-type(2)")
        time.sleep(2)

        # Verificar se o segundo checkbox foi desmarcado
        self.assert_false(self.is_selected("input[type='checkbox']:nth-of-type(2)"))

        # Alternar novamente os checkboxes
        self.click("input[type='checkbox']:nth-of-type(1)")  # Desmarca
        time.sleep(2)
        self.click("input[type='checkbox']:nth-of-type(2)")  # Marca
        time.sleep(2)

        # Verificar estado final dos checkboxes
        self.assert_false(self.is_selected("input[type='checkbox']:nth-of-type(1)"))
        self.assert_true(self.is_selected("input[type='checkbox']:nth-of-type(2)"))
```

## Radio Buttons

Radio buttons são semelhantes aos checkboxes, mas permitem selecionar apenas uma opção em um grupo. Eles são usados para escolhas mutuamente exclusivas.

```python
def test_selecionar_radio(self):
    # Abrir a página com radio buttons
    self.open("https://demoqa.com/radio-button")
    time.sleep(2)

    # Selecionar um radio button
    self.click("label[for='yesRadio']")
    time.sleep(2)

    # Verificar se o radio button foi selecionado
    self.assert_true(self.is_selected("#yesRadio"))

    # Selecionar outro radio button do mesmo grupo (apenas um será selecionado)
    self.click("label[for='impressiveRadio']")
    time.sleep(2)

    # Verificar a troca de seleção
    self.assert_false(self.is_selected("#yesRadio"))
    self.assert_true(self.is_selected("#impressiveRadio"))
```

## Dropdowns (Select)

Dropdowns permitem que o usuário selecione uma opção de uma lista suspensa. O SeleniumBase oferece métodos específicos para trabalhar com esses elementos.

```python
def test_selecionar_dropdown(self):
    # Abrir a página com dropdowns
    self.open("https://the-internet.herokuapp.com/dropdown")
    time.sleep(2)

    # Selecionar uma opção pelo texto visível
    self.select_option_by_text("#dropdown", "Option 1")
    time.sleep(2)

    # Verificar a opção selecionada usando get_value
    dropdown_value = self.get_value("#dropdown")
    self.assert_equal(dropdown_value, "1")
    
    # Selecionar uma opção pelo valor
    self.select_option_by_value("#dropdown", "2")
    time.sleep(2)

    # Verificar a nova opção selecionada
    dropdown_value = self.get_value("#dropdown")
    self.assert_equal(dropdown_value, "2")
    
    # Selecionar uma opção pelo índice (começando de 0)
    self.select_option_by_index("#dropdown", 0)
    time.sleep(2)
    
    # Verificar se voltou para a opção inicial
    dropdown_value = self.get_value("#dropdown")
    self.assert_equal(dropdown_value, "1")
```

## Datepickers (Seletores de Data)

Muitos sites usam widgets de calendário para seleção de datas. Existem duas abordagens comuns:

```python
def test_datepicker(self):
    # Abrir a página com datepicker
    self.open("https://demoqa.com/date-picker")
    time.sleep(2)

    # Abordagem 1: Digitar diretamente no campo (se o site permitir)
    # Primeiro limpa o campo
    self.clear("#datePickerMonthYearInput")
    time.sleep(2)
    self.type("#datePickerMonthYearInput", "12/25/2023")
    time.sleep(2)
    self.press_keys("#datePickerMonthYearInput", "Enter")
    time.sleep(2)

    # Abordagem 2: Interagir com o widget de calendário
    self.click("#datePickerMonthYearInput")  # Abre o calendário
    time.sleep(2)
    self.click(".react-datepicker__day--015:not(.react-datepicker__day--outside-month)")  # Seleciona o dia 15
    time.sleep(2)
```

## Upload de Arquivos

Para fazer upload de arquivos, você pode simplesmente fornecer o caminho do arquivo ao campo de upload:

```python
def test_upload_arquivo(self):
    # Abrir a página com upload
    self.open("https://the-internet.herokuapp.com/upload")
    time.sleep(2)

    # Selecionar um arquivo para upload (forneça o caminho completo)
    # Nota: Ajuste o caminho para um arquivo real em seu sistema
    caminho_arquivo = "/caminho/completo/para/arquivo.pdf"
    self.choose_file("#file-upload", caminho_arquivo)
    time.sleep(2)

    # Clicar no botão para enviar o arquivo
    self.click("#file-submit")
    time.sleep(3)

    # Verificar mensagem de sucesso após o upload
    self.assert_text("File Uploaded!", "h3")
```

## Sliders (Controles Deslizantes)

Para controles deslizantes, podemos usar a funcionalidade de drag_and_drop:

```python
def test_slider(self):
    # Abrir a página com slider
    self.open("https://the-internet.herokuapp.com/horizontal_slider")
    time.sleep(2)

    # Mover o slider de forma relativa (x, y) partindo da posição atual
    self.drag_and_drop_with_offset("input[type='range']", 50, 0)  # Move 50px para a direita
    time.sleep(2)
```

## iFrames

Se os elementos do formulário estiverem dentro de um iFrame, você precisa mudar para o iFrame antes de interagir com eles:

```python
def test_iframe_form(self):
    # Abrir a página com iframe
    self.open("https://the-internet.herokuapp.com/iframe")
    time.sleep(2)

    # Mudar para o iframe
    self.switch_to_frame("#mce_0_ifr")
    time.sleep(2)

    # Interagir com elementos dentro do iframe
    self.clear("#tinymce")
    time.sleep(2)
    self.type("#tinymce", "Texto dentro do iframe")
    time.sleep(2)

    # Voltar para o contexto principal
    self.switch_to_default_content()
```

## Alertas e Diálogos

Para lidar com alertas JavaScript:

```python
def test_alertas(self):
    # Abrir a página com alerta
    self.open("https://the-internet.herokuapp.com/javascript_alerts")
    time.sleep(2)

    # Clicar em um botão que dispara um alerta JavaScript
    self.click("button[onclick='jsAlert()']")
    time.sleep(2)

    # Aceitar o alerta (clicar em OK)
    self.accept_alert()
    time.sleep(2)

    # Verificar mensagem de resultado
    self.assert_text("You successfully clicked an alert", "#result")

    # Clicar em botão que abre um diálogo de confirmação
    self.click("button[onclick='jsConfirm()']")
    time.sleep(2)

    # Recusar o diálogo (clicar em Cancelar)
    self.dismiss_alert()
    time.sleep(2)

    # Verificar mensagem de resultado
    self.assert_text("You clicked: Cancel", "#result")

    # Clicar em botão que abre um prompt
    self.click("button[onclick='jsPrompt()']")
    time.sleep(2)

    # Inserir texto no prompt e aceitar
    self.send_keys_to_alert("Minha resposta")
    self.accept_alert()
    time.sleep(2)

    # Verificar mensagem de resultado
    self.assert_text("You entered: Minha resposta", "#result")
```

## Dicas Gerais para Formulários

1. **Valide os dados após submissão**: Sempre verifique se as mensagens de erro ou sucesso aparecem corretamente.

2. **Considere validações em tempo real**: Muitos formulários modernos validam os campos à medida que você digita.

3. **Lide com autofill**: Navegadores podem preencher automaticamente alguns campos; esteja preparado para isso.

4. **Trate formulários dinâmicos**: Alguns campos podem aparecer ou desaparecer com base em outras seleções.

5. **Teste casos de borda**: Inclua testes com dados inválidos, valores limite, etc.

```python
def test_validacao_formulario(self):
    # Abrir a página com formulário
    self.open("https://demoqa.com/automation-practice-form")
    time.sleep(2)

    # Testar com email inválido
    self.type("#userEmail", "email_invalido")
    time.sleep(2)
    self.type("#userNumber", "123")  # Campo telefone com menos dígitos que o esperado
    time.sleep(2)
    self.click("#submit")
    time.sleep(2)

    # Os campos mudarão de cor/borda indicando erro
    # Corrigir o email e o telefone
    self.update_text("#userEmail", "valido@exemplo.com")
    time.sleep(2)
    self.update_text("#userNumber", "1234567890")
    time.sleep(2)
    
    # Preencher campos obrigatórios
    self.type("#firstName", "João")
    time.sleep(2)
    self.type("#lastName", "Silva")
    time.sleep(2)
    self.click("label[for='gender-radio-1']")  # Seleciona gênero
    time.sleep(2)
    
    # Enviar novamente
    self.click("#submit")
    time.sleep(3)
    
    # Verificar se o formulário foi enviado (um modal aparece)
    self.wait_for_element_visible("#example-modal-sizes-title-lg")
    self.assert_text("Thanks for submitting the form", "#example-modal-sizes-title-lg")
```
