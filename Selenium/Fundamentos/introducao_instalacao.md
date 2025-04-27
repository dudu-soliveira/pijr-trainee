# Introdução ao Selenium e SeleniumBase

## O que é o SeleniumBase?

O SeleniumBase é uma biblioteca em Python que estende o Selenium com uma API mais amigável e poderosa, ideal para automação de testes de aplicações web. Ele combina o Selenium com ferramentas como pytest, pytest-html e outras bibliotecas úteis, oferecendo uma maneira prática de escrever testes legíveis e eficientes.

## Pré-requisitos

Antes de começarmos com a instalação, é preciso saber que o Selenium tem alguns pré-requisitos para que possa ser bem utilizado:

- Ambiente virtual configurado
- Python 3.7 ou superior

## Instalação do Selenium e do SeleniumBase

Para instalar o SeleniumBase e suas dependências (incluindo o Selenium), abra o terminal e execute:

```bash
uv add seleniumbase
```

## Verificação da instalação

Para verificar se a instalação foi bem-sucedida, digite no terminal:

```bash
seleniumbase

# ou

sbase
```

Se o comando for reconhecido e mostrar informações sobre o SeleniumBase, a instalação foi bem-sucedida.

## O WebDriver e seu funcionamento

O Selenium, para funcionar, precisa de um WebDriver, uma interface que o permite controlar os navegadores de forma automatizada.

### Como funciona

1. Você escreve comandos no código (para abrir uma URL ou clicar em um botão, por exemplo)
2. O WebDriver interpreta esses comandos e os executa no navegador
3. O navegador realiza as ações como se um usuário estivesse interagindo com ele

### Drivers específicos

Cada navegador possui seu driver específico:

- ChromeDriver para Chrome
- GeckoDriver para Firefox
- EdgeDriver para Edge
- SafariDriver para Safari

### Recomendação

Para fazer o web scraping, recomenda-se utilizar o Google Chrome. Ele é preparado para funcionar bem com a maioria dos sites e evita muitos dos problemas que podem ocorrer ao se usar outros navegadores.

### Configuração simplificada com SeleniumBase

O SeleniumBase já baixa e configura automaticamente a versão do ChromeDriver compatível com a versão do Google Chrome instalada no seu computador.

> **Observação importante**: Se você está no ambiente WSL (Windows Subsystem for Linux), lembre-se de baixar o Google Chrome para Linux para que o ChromeDriver funcione corretamente.
