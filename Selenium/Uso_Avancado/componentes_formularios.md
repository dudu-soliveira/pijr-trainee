# Trabalhando com Componentes de Formulários

Além dos campos de texto simples, os formulários web podem conter vários outros tipos de componentes, como checkboxes, radio buttons, dropdowns e muito mais. Vamos ver como trabalhar com esses elementos usando o SeleniumBase.

## Seleção de Checkboxes

Os checkboxes são elementos de formulário que permitem ao usuário selecionar múltiplas opções ao mesmo tempo. Para interagir com checkboxes, podemos usar o método `self.click()` passando o seletor do checkbox.

```python
from seleniumbase import BaseCase

class TestCheckboxes(BaseCase):
    def test_selecionar_checkbox(self):
        # Abrir a página com checkboxes
        self.open("https://exemplo.com/formulario")

        # Selecionar um checkbox
        self.click("input#termos")

        # Verificar se o checkbox foi selecionado
        self.assert_element_checked("input#termos")

        # Selecionar múltiplos checkboxes
        self.click("input#opcao1")
        self.click("input#opcao2")

        # Verificar se os checkboxes foram selecionados
        self.assert_element_checked("input#opcao1")
        self.assert_element_checked("input#opcao2")
```

## Radio Buttons

Radio buttons são semelhantes aos checkboxes, mas permitem selecionar apenas uma opção em um grupo. Eles são usados para escolhas mutuamente exclusivas.

```python
def test_selecionar_radio(self):
    # Abrir a página com radio buttons
    self.open("https://exemplo.com/formulario")

    # Selecionar um radio button
    self.click("input[name='genero'][value='masculino']")

    # Verificar se o radio button foi selecionado
    self.assert_element_checked("input[name='genero'][value='masculino']")

    # Selecionar outro radio button do mesmo grupo (apenas um será selecionado)
    self.click("input[name='genero'][value='feminino']")

    # Verificar a troca de seleção
    self.assert_element_not_checked("input[name='genero'][value='masculino']")
    self.assert_element_checked("input[name='genero'][value='feminino']")
```

## Dropdowns (Select)

Dropdowns permitem que o usuário selecione uma opção de uma lista suspensa. O SeleniumBase oferece métodos específicos para trabalhar com esses elementos.

```python
def test_selecionar_dropdown(self):
    # Abrir a página com dropdowns
    self.open("https://exemplo.com/formulario")

    # Selecionar uma opção pelo texto visível
    self.select_option_by_text("select#pais", "Brasil")

    # Selecionar uma opção pelo valor
    self.select_option_by_value("select#estado", "SP")

    # Selecionar uma opção pelo índice (começando de 0)
    self.select_option_by_index("select#cidade", 2)

    # Verificar a opção selecionada
    self.assert_option_selected("select#pais", "Brasil")
```

## Datepickers (Seletores de Data)

Muitos sites usam widgets de calendário para seleção de datas. Existem duas abordagens comuns:

```python
def test_datepicker(self):
    # Abrir a página com datepicker
    self.open("https://exemplo.com/formulario")

    # Abordagem 1: Digitar diretamente no campo (se o site permitir)
    self.type("#data-nascimento", "01/01/1990")

    # Abordagem 2: Interagir com o widget de calendário
    self.click("#data-viagem")  # Abre o calendário
    self.click(".ui-datepicker-next")  # Avança um mês
    self.click("a.ui-state-default[data-date='15']")  # Seleciona o dia 15
```

## Upload de Arquivos

Para fazer upload de arquivos, você pode simplesmente fornecer o caminho do arquivo ao campo de upload:

```python
def test_upload_arquivo(self):
    # Abrir a página com upload
    self.open("https://exemplo.com/upload")

    # Selecionar um arquivo para upload (forneça o caminho completo)
    caminho_arquivo = "/caminho/completo/para/arquivo.pdf"
    self.choose_file("input[type='file']", caminho_arquivo)

    # Clicar no botão para enviar o arquivo
    self.click("#botao-enviar")

    # Verificar mensagem de sucesso após o upload
    self.assert_text("Upload realizado com sucesso!", ".mensagem")
```

## Sliders (Controles Deslizantes)

Para controles deslizantes, podemos usar a funcionalidade de drag_and_drop:

```python
def test_slider(self):
    # Abrir a página com slider
    self.open("https://exemplo.com/slider")

    # Mover o slider de forma relativa (x, y) partindo da posição atual
    self.drag_and_drop_with_offset("input[type='range']", 50, 0)  # Move 50px para a direita
```

## iFrames

Se os elementos do formulário estiverem dentro de um iFrame, você precisa mudar para o iFrame antes de interagir com eles:

```python
def test_iframe_form(self):
    # Abrir a página com iframe
    self.open("https://exemplo.com/iframe")

    # Mudar para o iframe
    self.switch_to_frame("iframe#formulario")

    # Interagir com elementos dentro do iframe
    self.type("#nome", "João Silva")
    self.click("#enviar")

    # Voltar para o contexto principal
    self.switch_to_default_content()
```

## Alertas e Diálogos

Para lidar com alertas JavaScript:

```python
def test_alertas(self):
    # Abrir a página com alerta
    self.open("https://exemplo.com/alertas")

    # Clicar em um botão que dispara um alerta JavaScript
    self.click("#botao-alerta")

    # Aceitar o alerta (clicar em OK)
    self.accept_alert()

    # Clicar em botão que abre um diálogo de confirmação
    self.click("#botao-confirmar")

    # Recusar o diálogo (clicar em Cancelar)
    self.dismiss_alert()

    # Clicar em botão que abre um prompt
    self.click("#botao-prompt")

    # Inserir texto no prompt e aceitar
    self.send_keys_to_alert("Minha resposta")
    self.accept_alert()
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
    self.open("https://exemplo.com/formulario")

    # Testar com email inválido
    self.type("#email", "email_invalido")
    self.click("#enviar")

    # Verificar mensagem de erro
    self.assert_text("Email inválido", ".erro-email")

    # Corrigir o email e reenviar
    self.update_text("#email", "valido@exemplo.com")
    self.click("#enviar")

    # Verificar sucesso
    self.assert_text("Formulário enviado com sucesso!", ".mensagem-sucesso")
```
