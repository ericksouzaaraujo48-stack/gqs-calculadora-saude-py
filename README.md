# Calculadora de Saúde

![Python](https://img.shields.io/badge/Python-3.x-3776AB?logo=python&logoColor=white)
![Status](https://img.shields.io/badge/status-conclu%C3%ADdo-brightgreen)
![License](https://img.shields.io/badge/licen%C3%A7a-MIT-blue)

Sistema de linha de comando em Python para cálculos de saúde e bem-estar, desenvolvido como atividade prática da disciplina **Gestão e Qualidade de Software**, com foco em diagnóstico e correção de bugs em código legado, aplicando boas práticas de versionamento com Git e GitHub.

> Projeto forkado a partir de [`danhpaiva/gqs-calculadora-saude-py`](https://github.com/danhpaiva/gqs-calculadora-saude-py).

---

## Sumário

- [Sobre o Projeto](#-sobre-o-projeto)
- [Funcionalidades](#-funcionalidades)
- [Relatório de Bugs Encontrados](#-relatório-de-bugs-encontrados)
- [Detalhamento das Correções (Código)](#-detalhamento-das-correções-código)
- [Como Executar](#-como-executar)
- [Estrutura do Projeto](#-estrutura-do-projeto)
- [Tecnologias Utilizadas](#-tecnologias-utilizadas)
- [Processo de Correção](#-processo-de-correção)
- [Autor](#-autor)
- [Licença](#-licença)

---

## Sobre o Projeto

O **Calculadora de Saúde** é uma aplicação de terminal que oferece um menu interativo com cálculos relacionados à saúde do usuário. O objetivo deste exercício foi identificar, corrigir e documentar inconsistências lógicas e erros de execução presentes no código original, garantindo que o programa funcione corretamente do início ao fim.

## Funcionalidades

| Funcionalidade | Descrição |
|---|---|
| **Cálculo de IMC** | Calcula o Índice de Massa Corporal a partir do peso e da altura, retornando também a classificação (abaixo do peso, peso normal, sobrepeso ou obesidade). |
| **Recomendação de Água** | Calcula a meta diária de ingestão de água (em litros) com base no peso informado. |
| **Frequência Cardíaca Máxima** | Estima a frequência cardíaca máxima recomendada com base na idade do usuário. |
| **Sair** | Encerra a execução do programa de forma controlada. |

## Relatório de Bugs Encontrados

Durante os testes do código original, foram identificados **7 bugs**, cobrindo erros de sintaxe/tipo, lógica matemática e fluxo de execução:

| # | Local do Bug (função) | Comportamento Incorreto Observado | Solução Aplicada |
|---|---|---|---|
| 1 | `calcular_imc()` | O IMC era calculado multiplicando a altura por 2 (`altura * 2`) em vez de elevá-la ao quadrado, retornando um valor de IMC incorreto. | Fórmula corrigida para `peso / (altura ** 2)`, aplicando a potenciação correta. |
| 2 | `classificar_imc()` | Uso de operadores estritos (`<` e `>`) deixava valores limites (18.5, 24.9, 25.0, 30.0) sem classificação, retornando `None`. | Operadores ajustados para `>=` e `<=`, cobrindo corretamente os valores de fronteira. |
| 3 | `calcular_agua_diaria()` | A fórmula dividia o peso por 35 em vez de multiplicá-lo, gerando uma recomendação muito abaixo do esperado. | Fórmula corrigida para `(peso * 35) / 1000`, convertendo mililitros em litros. |
| 4 | `calcular_frequencia_cardiaca_maxima()` | A idade era somada a 220 (`220 + idade`), fazendo a FC máxima aumentar com a idade, o que é fisiologicamente incorreto. | Fórmula corrigida para `220 - idade`, seguindo o cálculo padrão. |
| 5 | `menu()` | A opção do usuário era capturada como `string`, sem conversão de tipo. | Adicionada conversão explícita com `int(input(...))`. |
| 6 | `main()` | As comparações `if opcao == 1`, `== 2` etc. nunca eram verdadeiras, pois `opcao` era `string` comparada a inteiros. | Corrigido junto ao Bug 5: com `opcao` como inteiro, as comparações passaram a funcionar corretamente. |
| 7 | `main()` (opção "Sair") | Ausência do comando `break`, fazendo o programa entrar em loop infinito mesmo após o usuário escolher encerrar. | Adicionado `break` ao final do bloco da opção 4, garantindo o encerramento correto do `while True`. |

## Detalhamento das Correções (Código)

Abaixo estão os trechos de código **antes** e **depois** de cada correção.

### Bug 1 — Cálculo do IMC (`calcular_imc`)

```python
# ❌ Antes
def calcular_imc(peso, altura):
    # Bug 1: Multiplicação em vez de potenciação no cálculo do IMC
    imc = peso / (altura * 2)
    return imc
```

```python
# ✅ Depois
def calcular_imc(peso, altura):
    # Corrigido: potenciação no cálculo do IMC
    imc = peso / (altura ** 2)
    return imc
```

### Bug 2 — Classificação do IMC (`classificar_imc`)

```python
# ❌ Antes
def classificar_imc(imc):
    # Bug 2: Faixas de classificação sobrepostas e sem retorno para valores limites
    if imc < 18.5:
        return "Abaixo do peso"
    elif imc > 18.5 and imc < 24.9:
        return "Peso normal"
    elif imc > 25.0 and imc < 29.9:
        return "Sobrepeso"
    elif imc > 30.0:
        return "Obesidade"
```

```python
# ✅ Depois
def classificar_imc(imc):
    # Corrigido: faixas ajustadas para incluir os valores limites
    if imc < 18.5:
        return "Abaixo do peso"
    elif imc >= 18.5 and imc <= 24.9:
        return "Peso normal"
    elif imc >= 25.0 and imc <= 29.9:
        return "Sobrepeso"
    elif imc >= 30.0:
        return "Obesidade"
```

### Bug 3 — Recomendação de Água (`calcular_agua_diaria`)

```python
# ❌ Antes
def calcular_agua_diaria(peso):
    # Bug 3: Fórmula dividindo o peso em vez de multiplicar por 35ml
    litros = (peso / 35)
    return litros
```

```python
# ✅ Depois
def calcular_agua_diaria(peso):
    # Corrigido: multiplicação por 35ml, convertendo para litros
    litros = (peso * 35) / 1000
    return litros
```

### Bug 4 — Frequência Cardíaca Máxima (`calcular_frequencia_cardiaca_maxima`)

```python
# ❌ Antes
def calcular_frequencia_cardiaca_maxima(idade):
    # Bug 4: Somando a idade em vez de subtrair de 220
    fc_max = 220 + idade
    return fc_max
```

```python
# ✅ Depois
def calcular_frequencia_cardiaca_maxima(idade):
    # Corrigido: subtração da idade de 220
    fc_max = 220 - idade
    return fc_max
```

### Bug 5 e 6 — Tipo da opção do menu (`menu` e `main`)

```python
# ❌ Antes
def menu():
    # ...
    # Bug 5: input() retorna string, mas o código não trata a conversão no menu
    opcao = input("Escolha uma opção (1-4): ")
    return opcao

def main():
    while True:
        opcao = menu()
        # Bug 6: As comparações abaixo falharão devido ao tipo de dado da 'opcao'
        if opcao == 1:
            ...
```

```python
# ✅ Depois
def menu():
    # ...
    # Corrigido: conversão para inteiro
    opcao = int(input("Escolha uma opção (1-4): "))
    return opcao

def main():
    while True:
        opcao = menu()
        # Corrigido: comparações funcionam pois 'opcao' agora é inteiro
        if opcao == 1:
            ...
```

### Bug 7 — Loop infinito ao sair (`main`)

```python
# ❌ Antes
elif opcao == 4:
    print("Encerrando o sistema...")
    # Bug 7: Ausência do break para sair do loop infinito
    print("Obrigado por usar nosso sistema!")
```

```python
# ✅ Depois
elif opcao == 4:
    print("Encerrando o sistema...")
    print("Obrigado por usar nosso sistema!")
    break  # Corrigido: encerra o loop while True
```

## Como Executar

### Pré-requisitos

- [Python 3.x](https://www.python.org/downloads/) instalado
- [Git](https://git-scm.com/) instalado

### Passo a passo

```bash
# 1. Clone o repositório
git clone https://github.com/SEU_USUARIO/gqs-calculadora-saude-py.git

# 2. Acesse a pasta do projeto
cd gqs-calculadora-saude-py

# 3. Execute o programa
python calculadora_saude.py
```

>  Em alguns sistemas operacionais, pode ser necessário usar `python3` em vez de `python`.

Ao executar, o menu abaixo será exibido no terminal:

```
==============================
  SISTEMA DE SAÚDE E BEM-ESTAR
==============================
1. Calcular IMC
2. Calcular Recomendação de Água
3. Calcular Frequência Cardíaca Máxima
4. Sair
```

Basta digitar o número da opção desejada e seguir as instruções exibidas para informar peso, altura ou idade.

## Estrutura do Projeto

```
gqs-calculadora-saude-py/
├── calculadora_saude.py   # Código-fonte principal do sistema
└── README.md              # Documentação do projeto
```

## Tecnologias Utilizadas

- **Python 3** — linguagem de programação utilizada no desenvolvimento do sistema
- **Git & GitHub** — controle de versão e hospedagem do repositório

## Processo de Correção

1. Fork do repositório original para a conta pessoal no GitHub.
2. Clonagem do repositório forkado para o ambiente local.
3. Execução e teste de todas as opções do menu para identificação dos bugs.
4. Correção de cada bug individualmente, com testes de regressão após cada ajuste.
5. Documentação de todos os bugs encontrados e das soluções aplicadas neste README.
6. Commit e push das alterações para o repositório remoto.

## Autor

#### Nome: Erick Souza Miranda Araujo
#### RA: 325130051
