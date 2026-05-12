# Marco-2

# Projeto Coprocessador - Sistemas Digitais (PBL)

Este repositório contém o desenvolvimento de um coprocessador para a disciplina de Sistemas Digitais. Atualmente, o projeto encontra-se na finalização do **Marco 2**, focado na integração dos módulos fundamentais e estruturação do Datapath.

## 📌 Sumário
1. [Introdução e Definição do Problema](#1-introdução-e-definição-do-problema)
2. [Requisitos Principais](#3-requisitos-principais)
3. [Arquitetura do Coprocessador](#4-arquitetura-do-coprocessador)
4. [Status do Desenvolvimento](#5-status-do-desenvolvimento)

---

## 1. Introdução e Definição do Problema
O projeto visa a implementação de um coprocessador capaz de auxiliar o processador principal em tarefas específicas de manipulação de dados. No contexto do **Marco 2**, o esforço está concentrado na integração física e lógica dos componentes (ULA, Banco de Registradores e Memória), garantindo que o fluxo de dados ocorra de maneira síncrona e correta entre os estágios.

O desafio proposto é criar uma arquitetura de hardware que suporte um conjunto básico de instruções (ISA) utilizando a metodologia de carga e armazenamento (*Load/Store*).
*   **Complexidade de Integração:** O problema central reside em conectar módulos independentes e garantir que os sinais de controle ativem os caminhos de dados corretos para cada instrução.
*   **Consistência de Dados:** Garantir que o resultado de uma operação esteja disponível para a instrução subsequente, respeitando os ciclos de clock.
  
## 2. Requisitos Principais
Os requisitos fundamentais estabelecidos para esta fase do projeto incluem:
*   **Operações Aritméticas:** Suporte a instruções de soma e manipulação de imediatos (`ADD`, `ADDI`).
*   **Acesso à Memória:** Implementação funcional de leitura e escrita em memória de dados (`LDUR`, `STUR`).
*   **Gerenciamento de Registradores:** Um banco de registradores robusto que suporte leitura dupla e escrita única por ciclo.
*   **Cálculo de Endereçamento:** Capacidade de calcular endereços efetivos usando base e deslocamento (offset).

## 3. Arquitetura do Coprocessador (Marco 2)
Até o momento, os seguintes blocos foram integrados no "caminho de dados":

### 3.1 Unidade Lógica e Aritmética (ULA)
Responsável pelo processamento de dados e cálculo de endereços. A ULA foi testada para operar com flags de status e suporte a diferentes operações de 64 bits.

### 3.2 Banco de Registradores
Estruturado para armazenar os operandos. O registrador `X0` foi fixado em zero conforme a arquitetura de referência, servindo de base para diversas operações de controle.

### 3.3 Integração (Juntando as Peças)
O foco atual é a conexão entre a saída da ULA e a interface de memória. O sistema de **Write-back** foi implementado para que os dados vindos da memória ou da ULA retornem corretamente ao Banco de Registradores no momento adequado.


**Desenvolvido como parte da graduação em Engenharia de Computação.**
