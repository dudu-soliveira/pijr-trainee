# Guia de Configuração de Projetos e Ambientes com UV

Este guia apresenta os conceitos básicos e o uso do **uv**, uma ferramenta que simplifica a instalação e o gerenciamento de diferentes versões do Python, criação de ambientes virtuais e controle de dependências em projetos.

## 1. Conceitos Básicos

Um **pacote** (ou biblioteca) é um conjunto de código pronto para resolver tarefas comuns (como trabalhar com dados ou acessar a internet). Para instalar ou remover pacotes, usamos um **gerenciador de pacotes**.

Um **ambiente virtual** é uma área isolada no disco que contém um interpretador Python e seus pacotes, garantindo que cada projeto use suas próprias versões sem conflitos.

## 2. Instalando o UV

O **uv** unifica o gerenciamento do próprio Python, de ambientes virtuais e de pacotes.

Escolha o método de instalação conforme seu sistema operacional:

### macOS/Linux

```bash
curl -LsSf https://astral.sh/uv/install.sh | sh
```

### Windows (PowerShell)

```powershell
powershell -ExecutionPolicy ByPass -c "irm https://astral.sh/uv/install.ps1 | iex"
```

## 3. Instalando e Gerenciando Versões do Python

Cada projeto pode precisar de uma versão específica do Python (por exemplo, 3.10 ou 3.12). O uv facilita isso:

- **Instalar** uma versão:

  ```bash
  uv python install 3.12
  ```

  Faz o download e prepara essa versão no seu computador.

- **Listar** as versões disponíveis:

  ```bash
  uv python list
  ```

- **Escolher** qual usar no projeto (gera um arquivo `.python-version` na pasta do projeto):

  ```bash
  uv python pin 3.12
  ```

## 4. Inicializando um Novo Projeto

Um projeto Python é uma pasta com arquivos de código e configuração. Para começar rapidamente:

```bash
uv init meu-projeto
cd meu-projeto
```

Esse comando cria:

```
.
├── .python-version    # versão do Python definida para este projeto
├── README.md          # descrição do projeto
├── main.py            # arquivo de exemplo com código inicial
├── pyproject.toml     # lista de dependências e metadados do projeto
└── uv.lock            # registros exatos de versão dos pacotes (após primeiro sync)
```

Edite o `main.py` para adicionar seu código.

## 5. Ambientes Virtuais com `uv venv`

Um ambiente virtual mantém isolados Python e pacotes de cada projeto.

Para criar um ambiente usando a versão de Python indicada em `.python-version`:

```bash
uv venv
```

Ou especifique diretamente:

```bash
uv venv --python 3.12
```

**Ativar** o ambiente para começar a usar:

| Sistema     | Comando de Ativação         |
| ----------- | --------------------------- |
| Windows     | `.\.venv\Scripts\activate`  |
| Linux/macOS | `source .venv/bin/activate` |

Após ativar, o terminal passa a usar o Python e os pacotes deste ambiente.

## 6. Instalando, Removendo e Listando Pacotes

Com o ambiente ativo, gerencie pacotes assim:

- **Adicionar** (instalar) pacotes no projeto:

  ```bash
  uv add requests pandas
  ```

  Isto atualiza o arquivo `pyproject.toml` e instala as bibliotecas.

- **Remover** pacotes:

  ```bash
  uv remove numpy
  ```

  Remove do `pyproject.toml` e desinstala.

- **Listar** pacotes instalados:

  ```bash
  uv tree
  ```

  Mostra quais bibliotecas o projeto usa e suas dependências.

## 7. Sincronizando e Trancando Versões

- **Sincronizar** (instalar ou remover conforme `pyproject.toml`):

  ```bash
  uv sync
  ```

- **Travar** versões exatas em `uv.lock` para garantir que todos usem o mesmo conjunto de pacotes:

  ```bash
  uv lock
  ```

## 8. Executando Comandos e Scripts

- Para **executar comandos** dentro do ambiente:

  ```bash
  uv run pytest  # testa seu código
  uv run main.py # executa seu programa
  ```

- Para **rodar scripts isolados** sem criar projeto:

  ```bash
  uv run script.py
  ```

  Você pode incluir dependências ao script:

  ```bash
  uv add --script script.py rich
  uv lock --script script.py
  ```

## 9. Referências e Recursos Adicionais

1. [Instalação do UV](https://docs.astral.sh/uv/getting-started/installation/)
2. [Gerenciamento de versões do Python](https://docs.astral.sh/uv/concepts/python-versions/)
3. [Ambientes virtuais com UV](https://docs.astral.sh/uv/pip/environments/)
4. [Iniciando projetos com ](https://docs.astral.sh/uv/guides/projects/)[`uv init`](https://docs.astral.sh/uv/guides/projects/)
5. [Stop Using Pip - UV Tutorial (vídeo em inglês)](https://www.youtube.com/watch?v=6pttmsBSi8M)
