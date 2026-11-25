# ArenaSports

Plataforma de automação e gestão de agendamentos de quadras esportivas (society, areia e poliesportivas).

## Sumário

1. [Introdução](#1-introdução)
    - [1.1. Descrição Geral do Software](#11-descrição-geral-do-software)
    - [1.2. Descrição dos Usuários](#12-descrição-dos-usuários)
2. [Requisitos Gerais do Software](#2-requisitos-gerais-do-software)
    - [2.1. Requisitos Funcionais](#21-requisitos-funcionais)
    - [2.2. Requisitos Não Funcionais](#22-requisitos-não-funcionais)
    - [2.3. Regras de Negócio](#23-regras-de-negócio)
3. [Diagramas UML](#3-diagramas-uml)
    - [3.1. Diagramas de Casos de Uso](#31-diagramas-de-casos-de-uso)
    - [3.2. Diagramas de Sequência](#32-diagramas-de-sequência)
    - [3.3. Diagramas de Atividades](#33-diagramas-de-atividades)

---

## 1. Introdução

### 1.1. Descrição Geral do Software

O sistema ArenaSports é uma plataforma voltada à automação e gestão de agendamentos de quadras esportivas (society, areia e poliesportivas). O sistema resolve o problema do choque de horários e da informalidade em reservas, permitindo que atletas visualizem agendas em tempo real, realizem reservas via PIX e que os administradores controlem o caixa e bloqueiem horários para manutenção.

### 1.2. Descrição dos Usuários

Os usuários desse sistema podem ser classificados em:

1. **Atleta / Cliente:** atletas locais que desejam consultar horários disponíveis, realizar reservas de quadras e contratar materiais adicionais (coletes, bolas).
2. **Atendente / Administrador:** responsável por gerenciar as quadras, validar reservas efetuadas presencialmente/balcão, bloquear horários para manutenção e emitir relatórios de faturamento.

---

## 2. Requisitos Gerais do Software

### 2.1. Requisitos Funcionais

| Identificador | Descrição | Escopo | Prioridade |
|:-------------:|-----------|:------:|:----------:|
| RF01 | O sistema deve permitir que o usuário crie um cadastro informando nome, e-mail, telefone e senha. | Geral | Essencial |
| RF02 | O sistema deve permitir o acesso do usuário por meio de login com e-mail e senha. | Geral | Essencial |
| RF03 | O sistema deve exibir a grade de horários disponíveis e ocupados por data e quadra. | Agendamento | Essencial |
| RF04 | O sistema deve permitir a reserva de um horário mediante confirmação de pagamento do sinal. | Agendamento | Essencial |
| RF05 | O sistema deve permitir que o usuário inclua aluguel de materiais adicionais (coletes, bolas) na reserva. | Agendamento | Desejável |
| RF06 | O sistema deve permitir que o usuário solicite o cancelamento de uma reserva realizada. | Agendamento | Importante |
| RF07 | O sistema deve permitir que o administrador bloqueie horários da agenda para manutenção. | Administração | Importante |
| RF08 | O sistema deve permitir a saída do usuário (logout). | Geral | Importante |

### 2.2. Requisitos Não Funcionais

| Identificador | Descrição | Categoria | Escopo | Prioridade |
|:-------------:|-----------|:---------:|:------:|:----------:|
| RNF01 | O sistema deve criptografar todas as senhas dos usuários. | Segurança | Geral | Essencial |
| RNF02 | O sistema deve processar as consultas de disponibilidade e respostas da agenda em até 2 segundos. | Desempenho | Agendamento | Importante |
| RNF03 | O sistema deve disponibilizar interface totalmente responsiva para navegação em dispositivos móveis. | Usabilidade | Geral | Essencial |
| RNF04 | O sistema deve oferecer tema escuro opcional para melhor experiência do usuário. | Usabilidade | Geral | Desejável |

### 2.3. Regras de Negócio

| Identificador | Descrição | Escopo | Prioridade |
|:-------------:|-----------|:------:|:----------:|
| RN01 | A reserva online de uma quadra só é confirmada mediante o pagamento prévio de no mínimo 50% do valor do horário. | Agendamento | Essencial |
| RN02 | Cancelamentos de reservas com direito a reembolso ou crédito só podem ser efetuados com até 6 horas de antecedência do horário agendado. | Agendamento | Importante |
| RN03 | O mesmo horário de uma quadra não pode ser reservado por dois clientes simultaneamente. | Agendamento | Essencial |
| RN04 | O horário selecionado fica pré-reservado por 10 minutos; caso o pagamento não seja concluído nesse período, a vaga é liberada automaticamente para a agenda. | Agendamento | Importante |
| RN05 | Clientes que realizarem mais de 5 reservas no mesmo mês recebem 10% de desconto no valor do próximo agendamento. | Financeiro | Desejável |

---

## 3. Diagramas UML

### 3.1. Diagramas de Casos de Uso

<img width="1745" height="1543" alt="image" src="https://github.com/user-attachments/assets/454b2f2c-15da-45ef-bc88-daecc7d3f038" />

<p align="center"><i>Figura 1 – Diagrama de Caso de Uso</i></p>

### 3.2. Diagramas de Sequência

#### Cadastrar

```mermaid
%%{init: {'theme':'base','themeVariables':{'actorBkg':'#f0fdfa','actorBorder':'#2dd4bf','actorTextColor':'#0f172a','actorLineColor':'#94a3b8','signalColor':'#1e293b','signalTextColor':'#1e293b','labelBoxBkgColor':'#ffffff','labelBoxBorderColor':'#475569','labelTextColor':'#1e293b','loopTextColor':'#1e293b'}}}%%
sequenceDiagram
    actor U as Usuário
    participant I as Interface
    participant C as Controle
    participant B as Banco de Dados

    U->>I: Seleciona Realizar Cadastro
    U->>I: Informa nome, e-mail, telefone e senha
    I->>I: Valida os campos
    I->>C: Envia dados válidos
    C->>B: Verifica se o e-mail existe
    B-->>C: Retorna resultado
    alt E-mail já cadastrado
        C-->>I: Retorna erro
        I-->>U: Exibe mensagem de erro
    else E-mail disponível
        C->>C: Criptografa a senha
        C->>B: Salva novo usuário
        B-->>C: Confirma cadastro
        C-->>I: Retorna sucesso
        I-->>U: Exibe confirmação e redireciona ao login
    end
```

#### Logar

```mermaid
%%{init: {'theme':'base','themeVariables':{'actorBkg':'#f0fdfa','actorBorder':'#2dd4bf','actorTextColor':'#0f172a','actorLineColor':'#94a3b8','signalColor':'#1e293b','signalTextColor':'#1e293b','labelBoxBkgColor':'#ffffff','labelBoxBorderColor':'#475569','labelTextColor':'#1e293b','loopTextColor':'#1e293b'}}}%%
sequenceDiagram
    actor U as Usuário
    participant I as Interface
    participant C as Controle
    participant B as Banco de Dados

    U->>I: Informa e-mail e senha
    U->>I: Clica em Entrar
    I->>C: Envia credenciais
    C->>B: Consulta usuário e senha
    B-->>C: Retorna resultado da consulta
    alt Credenciais válidas
        C->>C: Cria sessão ativa
        C-->>I: Confirma autenticação
        I-->>U: Exibe tela principal
    else Credenciais inválidas
        C-->>I: Retorna falha na autenticação
        I-->>U: Exibe mensagem de erro
    end
```

#### Consulta de horários

```mermaid
%%{init: {'theme':'base','themeVariables':{'actorBkg':'#f0fdfa','actorBorder':'#2dd4bf','actorTextColor':'#0f172a','actorLineColor':'#94a3b8','signalColor':'#1e293b','signalTextColor':'#1e293b','labelBoxBkgColor':'#ffffff','labelBoxBorderColor':'#475569','labelTextColor':'#1e293b','loopTextColor':'#1e293b'}}}%%
sequenceDiagram
    actor U as Usuário
    participant I as Interface
    participant C as Controle
    participant B as Banco de Dados

    U->>I: Seleciona data e tipo de quadra
    I->>C: Solicita horários disponíveis
    C->>B: Consulta agendamentos por quadra e data
    B-->>C: Retorna horários e status
    C->>C: Organiza a disponibilidade
    C-->>I: Envia lista de horários
    I-->>U: Exibe horários livres, ocupados e bloqueados
```

#### Reserva

```mermaid
%%{init: {'theme':'base','themeVariables':{'actorBkg':'#f0fdfa','actorBorder':'#2dd4bf','actorTextColor':'#0f172a','actorLineColor':'#94a3b8','signalColor':'#1e293b','signalTextColor':'#1e293b','labelBoxBkgColor':'#ffffff','labelBoxBorderColor':'#475569','labelTextColor':'#1e293b','loopTextColor':'#1e293b'}}}%%
sequenceDiagram
    actor U as Usuário
    participant I as Interface
    participant C as Controle
    participant B as Banco de Dados
    participant G as Gateway PIX

    U->>I: Seleciona horário livre
    I->>C: Solicita pré-reserva
    C->>B: Verifica disponibilidade
    B-->>C: Confirma horário livre
    C->>B: Altera status para PENDENTE
    C->>G: Solicita cobrança do sinal
    G-->>C: Retorna QR Code PIX
    C-->>I: Envia QR Code
    I-->>U: Exibe cobrança
    alt Pagamento confirmado em até 10 minutos
        G-->>C: Envia confirmação via Webhook
        C->>B: Confirma agendamento e registra pagamento
        B-->>C: Confirma gravação
        C-->>I: Envia comprovante
        I-->>U: Exibe reserva confirmada
    else Pagamento não confirmado no prazo
        C->>B: Libera horário pré-reservado
        B-->>C: Confirma liberação
        C-->>I: Informa expiração da reserva
        I-->>U: Exibe mensagem de horário liberado
    end
```

### 3.3. Diagramas de Atividades

#### Cadastro

```mermaid
flowchart TD
    Inicio([Início]) --> A("Acessar tela de cadastro")
    A --> B("Informar nome, e-mail, telefone e senha")
    B --> C("Dados válidos?")
    C -- Não --> D("Exibir mensagem de erro")
    D --> B
    C -- Sim --> E("Consultar e-mail no banco de dados")
    E --> F("E-mail já existe?")
    F -- Sim --> G("Informar que o e-mail já está cadastrado")
    G --> B
    F -- Não --> H("Criptografar senha")
    H --> I("Salvar cadastro no banco de dados")
    I --> J("Confirmar criação da conta")
    J --> K("Redirecionar para tela de login")
    K --> Fim([Fim])
    classDef step fill:#032a4c,stroke:#1e5a8a,color:#8fd3ff;
    classDef dec fill:#020b1a,stroke:#1e5a8a,color:#8fd3ff;
    classDef term fill:#031a33,stroke:#1e5a8a,color:#8fd3ff;
    class A,B,D,E,G,H,I,J,K step;
    class C,F dec;
    class Inicio,Fim term;
```

#### Logar

```mermaid
flowchart TD
    Inicio([Início]) --> A("Acessar tela de login")
    A --> B("Informar e-mail e senha")
    B --> C("Clicar em Entrar")
    C --> D("Validar credenciais no banco de dados")
    D --> E("Credenciais válidas?")
    E -- Não --> F("Exibir mensagem de erro")
    F --> B
    E -- Sim --> G("Criar sessão ativa")
    G --> H("Redirecionar para tela principal")
    H --> Fim([Fim])
    classDef step fill:#032a4c,stroke:#1e5a8a,color:#8fd3ff;
    classDef dec fill:#020b1a,stroke:#1e5a8a,color:#8fd3ff;
    classDef term fill:#031a33,stroke:#1e5a8a,color:#8fd3ff;
    class A,B,C,D,F,G,H step;
    class E dec;
    class Inicio,Fim term;
```

#### Consultar horários

```mermaid
flowchart TD
    Inicio([Início]) --> A("Usuário acessa a agenda")
    A --> B("Selecionar data e tipo de quadra")
    B --> C("Consultar horários no banco de dados")
    C --> D("Recuperar horários e respectivos status")
    D --> E("Organizar informações da agenda")
    E --> F("Exibir horários na interface")
    F --> G("Deseja consultar outra data?")
    G -- Sim --> B
    G -- Não --> Fim([Fim])
    classDef step fill:#032a4c,stroke:#1e5a8a,color:#8fd3ff;
    classDef dec fill:#020b1a,stroke:#1e5a8a,color:#8fd3ff;
    classDef term fill:#031a33,stroke:#1e5a8a,color:#8fd3ff;
    class A,B,C,D,E,F step;
    class G dec;
    class Inicio,Fim term;
```

#### Reservas

```mermaid
flowchart TD
    Inicio([Início]) --> A("Selecionar horário livre")
    A --> B("Verificar disponibilidade")
    B --> C("Horário disponível?")
    C -- Não --> D("Informar indisponibilidade")
    D --> A
    C -- Sim --> E("Pré-reservar horário por até 10 minutos")
    E --> F("Solicitar pagamento do sinal")
    F --> G("Gerar e exibir QR Code PIX")
    G --> H("Pagamento confirmado no prazo?")
    H -- Sim --> I("Receber confirmação do gateway")
    I --> J("Registrar pagamento no banco de dados")
    J --> K("Confirmar agendamento")
    K --> L("Emitir comprovante")
    L --> Fim([Fim])
    H -- Não --> M("Expirar pré-reserva")
    M --> N("Liberar horário na agenda")
    N --> O("Informar que a reserva expirou")
    O --> Fim
    classDef step fill:#032a4c,stroke:#1e5a8a,color:#8fd3ff;
    classDef dec fill:#020b1a,stroke:#1e5a8a,color:#8fd3ff;
    classDef term fill:#031a33,stroke:#1e5a8a,color:#8fd3ff;
    class A,B,D,E,F,G,I,J,K,L,M,N,O step;
    class C,H dec;
    class Inicio,Fim term;
```
