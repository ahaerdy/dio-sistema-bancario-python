# 🏦 Sistema Bancário com Python

Projeto desenvolvido como desafio prático do **Bootcamp Suzano Python Developer** na plataforma [DIO](https://www.dio.me/), dentro do módulo de fundamentos da linguagem Python.

---

## Descrição

Implementação de um sistema bancário simples em Python puro, sem uso de bibliotecas externas. O sistema simula operações essenciais de um banco digital: **depósito**, **saque** e **extrato**, com regras de negócio aplicadas via lógica de programação.

O projeto foi desenvolvido com foco em:

- Estruturas de controle de fluxo (`while`, `if/elif/else`)
- Entrada e validação de dados com `input()`
- Manipulação e formatação de strings (f-strings)
- Aplicação de regras de negócio em código procedural

---

## Funcionalidades

| Operação | Descrição |
|----------|-----------|
| Depósito | Aceita valores positivos e acumula no saldo |
| Saque | Permite até 3 saques diários com limite de R$ 500,00 por operação |
| Extrato | Lista todas as movimentações e exibe o saldo atual |
| Sair | Encerra o programa |

---

## Como o Projeto Foi Desenvolvido

### 1. Menu interativo com loop contínuo

O sistema roda dentro de um laço `while True`, mantendo o programa ativo até que o usuário escolha sair. O menu é exibido a cada iteração e a entrada do usuário é capturada com `input()`.

```python
menu = """
[d] Depositar
[s] Sacar
[e] Extrato
[q] Sair
=> """

while True:
    opcao = input(menu)
```

> O programa só é encerrado quando o usuário digita `q`, que aciona um `break` e interrompe o loop.

---

### 2. Variáveis de estado

Antes do loop, todas as variáveis de controle são inicializadas. Elas persistem entre as iterações, acumulando o estado da sessão:

```python
saldo = 0           # Saldo atual da conta
limite = 500        # Limite máximo por saque
extrato = ""        # Histórico de movimentações (string acumulada)
numero_saques = 0   # Contador de saques realizados na sessão
LIMITE_SAQUES = 3   # Constante: máximo de saques permitidos
```

> A convenção `LIMITE_SAQUES` em letras maiúsculas sinaliza que é uma **constante** — um valor que não deve ser alterado durante a execução.

---

### 3. Operação de Depósito

```python
if opcao == "d":
    valor = float(input("Informe o valor do depósito: "))

    if valor > 0:
        saldo += valor
        extrato += f"Depósito: R$ {valor:.2f}\n"
    else:
        print("Operação falhou! O valor informado é inválido.")
```

**Como funciona:**
- O valor digitado pelo usuário é convertido para `float` (aceita centavos).
- A validação garante que apenas valores positivos sejam aceitos.
- Se válido, o saldo é incrementado e a operação é registrada na string `extrato` usando uma **f-string** com formatação monetária (`:.2f` = duas casas decimais).

---

### 4. Operação de Saque

```python
elif opcao == "s":
    valor = float(input("Informe o valor do saque: "))

    excedeu_saldo  = valor > saldo
    excedeu_limite = valor > limite
    excedeu_saques = numero_saques >= LIMITE_SAQUES

    if excedeu_saldo:
        print("Operação falhou! Você não tem saldo suficiente.")
    elif excedeu_limite:
        print("Operação falhou! O valor do saque excede o limite.")
    elif excedeu_saques:
        print("Operação falhou! Número máximo de saques excedido.")
    elif valor > 0:
        saldo -= valor
        extrato += f"Saque: R$ {valor:.2f}\n"
        numero_saques += 1
    else:
        print("Operação falhou! O valor informado é inválido.")
```

**Como funciona:**

A operação de saque possui **quatro validações encadeadas**, cada uma com sua mensagem de erro específica:

| Verificação | Condição | Mensagem |
|-------------|----------|----------|
| Saldo insuficiente | `valor > saldo` | Saldo insuficiente |
| Acima do limite por saque | `valor > 500` | Excede o limite |
| Limite diário atingido | `numero_saques >= 3` | Máximo de saques excedido |
| Valor inválido | `valor <= 0` | Valor inválido |

Somente quando todas as validações são aprovadas o saque é efetivado: o saldo é decrementado, a operação é registrada no extrato e o contador de saques é incrementado.

---

### 5. Operação de Extrato

```python
elif opcao == "e":
    print("\n================ EXTRATO ================")
    print("Não foram realizadas movimentações." if not extrato else extrato)
    print(f"\nSaldo: R$ {saldo:.2f}")
    print("==========================================")
```

**Como funciona:**

- Exibe o conteúdo acumulado em `extrato`, que é uma string com todas as movimentações registradas ao longo da sessão.
- Utiliza um **operador ternário** (`... if ... else ...`) para exibir a mensagem padrão caso nenhuma movimentação tenha sido feita (`extrato` vazia é avaliada como `False` em Python).
- O saldo é exibido ao final com formatação monetária.

---

## ▶️ Como Executar

**Pré-requisito:** Python 3.x instalado.

```bash
# Clone o repositório
git clone https://github.com/seu-usuario/sistema-bancario-python.git

# Acesse a pasta
cd sistema-bancario-python

# Execute o script
python desafio.py
```

---

## Regras de Negócio

- ✅ Apenas **valores positivos** são aceitos em depósitos e saques.
- ✅ Limite de **R$ 500,00** por operação de saque.
- ✅ Máximo de **3 saques** por sessão.
- ✅ Não é possível sacar mais do que o saldo disponível.
- ✅ O extrato exibe todas as movimentações com formatação `R$ xxx.xx`.
- ✅ Quando não há movimentações, o extrato exibe mensagem informativa.

---

## 🗂️ Estrutura do Projeto

```
dio-sistema-bancario-python/
│
└── desafio.py       # Script principal com toda a lógica do sistema
```

---

## 🎓 Contexto de Aprendizado

Este projeto integra o percurso formativo do Bootcamp **Suzano Python Developer** (DIO) e explora os seguintes conceitos da linguagem Python:

- Variáveis e tipos primitivos (`int`, `float`, `str`)
- Entrada de dados com `input()` e conversão de tipos
- Estruturas condicionais (`if`, `elif`, `else`)
- Laço de repetição com `while` e uso de `break`
- Formatação de strings com f-strings e especificadores de formato
- Boas práticas de nomenclatura (constantes em maiúsculas)
- Lógica de validação com múltiplas condições encadeadas

---

## 📜 Certificado

Certificado de conclusão disponível em: [https://hermes.dio.me/certificates/7UHQZHML.pdf](https://hermes.dio.me/certificates/7UHQZHML.pdf)

---

## Referências:

- [Repositório de Estudos - Bootcamp Suzano Python Developer](https://github.com/ahaerdy/dio-learning/tree/main/Suzano%20-%20Python%20Developer)