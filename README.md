# GitHub Actions

Automatizando ações com GitHub Actions


### O que é GitHUb Actions

GitHub Actions é uma plataforma de automação poderosa e flexível oferecida pelo GitHub, projetada para simplificar e automatizar o fluxo de trabalho de desenvolvimento de software. Essa ferramenta permite que desenvolvedores automatizem tarefas, desde a compilação e teste de código até a implantação e entrega contínua.

Com o GitHub Actions, você pode definir fluxos de trabalho (workflows) diretamente no repositório do GitHub usando arquivos de configuração YAML. Esses fluxos de trabalho podem ser acionados por eventos específicos, como push de código, criação de pull requests, ou até mesmo cronogramas regulares. A flexibilidade do GitHub Actions permite a integração perfeita com várias ferramentas e ambientes de execução.

Os workflows podem ser compostos por uma série de jobs, cada um contendo uma lista de passos (steps). Esses passos representam as diferentes etapas do processo de automação, como a execução de comandos, a configuração de ambientes, a instalação de dependências e a execução de testes automatizados.

GitHub Actions também oferece uma ampla variedade de ações predefinidas (actions), que são blocos de construção reutilizáveis que simplificam a configuração e execução de tarefas comuns. Além disso, é possível criar e compartilhar suas próprias ações personalizadas para atender às necessidades específicas do seu projeto.

Em resumo, o GitHub Actions é uma ferramenta poderosa para automatizar os processos de desenvolvimento, melhorar a eficiência da equipe e garantir a qualidade do código, tudo integrado diretamente ao ambiente familiar do GitHub.

### Descrição do processo

Para ser possivel usar o GitHub Actions é preciso ter uma pasta dentro do repositório chamada `.github` e dentro dessa pasta outra pasta chamada`workflows` com arquivos `yaml`, para o exemplo desse repositório tem-se o arquivo `ci.yml` que possui a configuração dos processos necessários para usar o GitHub Actions.

Para configurar o arquivo é preciso seguir alguns passos:

#### Passo 1: Definir o nome do workflow

```
name: Integração Contínua
```

#### Passo 2: Definir em que situação o script irá ser rodado.

No caso desse exemplo, toda vez que o ouver um `pull_request` ou um `push`.

```
on:
  pull_request:
    branches:
      - main
  push:
```

#### Criar as tarefas a serem executada

Para realizar esse passo, deve-se expecificar qual sistema operacional será usado, pode ser basicamete três: `Windows`, `MACos` ou `Ubuntu Linux`. Além disso, é preciso expecificar os `steps`, primeiro deve-se usar o `actions/checkout@v2`.

Esses passos anteriores são necessários para todo script GitHub Actions, por fim, você pode instalar os softwares necessários para realizar a atividade que você deseja, para o exemplo desse repositporio, é preciso instalar o `Python` o gerencdor de projetos `Poetry` e as bibliotecas que irá verificar a integridade dos código, sendo elas: `black`, `isort`, `pydocstyle` e `pytest`.

Além disso, é possivel criar variios `jobs` e executar os processos em paralelo, para isso basta replicar a estrutura como mostrado abaixo:

```
name: Integração Contínua

on:
  pull_request:
    branches:
      - main
  push:


jobs:
  execute_1:
    runs-on: ubuntu-latest
    steps:
      - name: Actions Checkout
        uses: actions/checkout@v2

      - name: Instala o Python 3.10
        uses: actions/setup-python@v5
        with:
          python-version: '3.10'

      - name: Instala o Poetry
        uses: Gr1N/setup-poetry@v8

      - name: Instala as Dependências
        run: poetry install

      - name: Executa o Black
        run: poetry run black app --check

      - name: Executa o Isort
        run: poetry run isort --check app/

  execute_2:
    runs-on: ubuntu-latest
    steps:
      - name: Actions Checkout
        uses: actions/checkout@v2

      - name: Instala o Python 3.10
        uses: actions/setup-python@v5
        with:
          python-version: '3.10'

      - name: Instala o Poetry
        uses: Gr1N/setup-poetry@v8

      - name: Instala as Dependências
        run: poetry install

      - name: Executa o Pydocstyle
        run: poetry run pydocstyle
        
      - name: Executa o Testes Unitários
        run: poetry run pytest
```

Nesse exemplo existem dois `jobs`, sendo eles `execute_1` e `execute_2` que são executados em paralelos.