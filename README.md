# Marco-2

# Projeto Coprocessador - Sistemas Digitais (PBL)

Este repositório contém o desenvolvimento de um coprocessador para a disciplina de Sistemas Digitais. Atualmente, o projeto encontra-se na finalização do **Marco 2**, focado na integração dos módulos fundamentais e estruturação do Datapath.

## 📌 Sumário

1. [Introdução e Definição do Problema](#1-introdução-e-definição-do-problema)
2. [Requisitos Principais](#2-requisitos-principais)
3. [Fundamentação Teórica](#3-fundamentação-teórica)
   - [3.1 DE1-SoC e a Lightweight HPS-to-FPGA Bridge](#31-de1-soc-e-a-lightweight-hps-to-fpga-bridge)
   - [3.2 MMIO (Memory-Mapped I/O)](#32-mmio-memory-mapped-io)
   - [3.3 /dev/mem e Syscalls](#33-devmem-e-syscalls)
   - [3.4 Polling](#34-polling)
4. [Co-processador ELM](#4-co-processador-elm)
   - [4.1 Descrição](#41-descrição)
   - [4.2 Barramentos](#42-barramentos)
   - [4.3 ISA — Conjunto de Instruções](#43-isa--conjunto-de-instruções)
5. [Descrição da Solução](#5-descrição-da-solução)
   - [5.1 Arquitetura Geral](#51-arquitetura-geral)
   - [5.2 Funções Expostas](#52-funções-expostas)
   - [5.3 Fluxo de Execução](#53-fluxo-de-execução)
6. [Modo de Uso](#6-modo-de-uso)
7. [Testes e Resultados](#7-testes-e-resultados)
8. [Erros e Limitações](#8-erros-e-limitações)
9. [Próximos Passos — Marco 3](#9-próximos-passos--marco-3)
10. [Referências](#10-referências)

---

## 1. Introdução e Definição do Problema
O principal objetivo deste trabalho é desenvolver um driver em Assembly ARM capaz de fazer a comunicação entre o sistema Linux executando no HPS e o co-processador ELM implementado na FPGA. Essa comunicação será realizada utilizando MMIO (Memory-Mapped I/O), onde o processador acessa registradores do hardware através de endereços de memória específicos.

Para isso, é necessário configurar corretamente a integração entre HPS e FPGA utilizando as bridges disponibilizadas pela plataforma SoC FPGA, além de controlar os PIOs responsáveis pelo envio e recebimento de dados do co-processador.
  
## 2. Requisitos Principais
Os requisitos fundamentais estabelecidos para esta fase do projeto incluem:
* Integrar o HPS com a FPGA utilizando as bridges HPS↔FPGA.
* Permitir comunicação entre Linux e o co-processador através de MMIO.
* Desenvolver um driver em Assembly ARM para controle do hardware.
* Implementar envio de instruções de 32 bits para o co-processador.
* Controlar os sinais Enable, Clear e Reset através do barramento Signals.
* Ler o estado do co-processador utilizando o barramento Data Out.
* Implementar polling para verificar as flags DONE, BUSY e ERROR.
* Ler arquivos binários (.bin) contendo imagem, bias e beta.
* Armazenar os dados em buffers na memória RAM.
* Enviar corretamente os dados para o hardware seguindo a ISA definida.
* Executar a instrução START para iniciar a inferência.
* Receber e interpretar o dígito predito retornado pelo co-processador.


## 3. Fundamentação Teórica (Marco 2)

## 4. Descrição da Solução

