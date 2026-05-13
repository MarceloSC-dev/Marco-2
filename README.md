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
   - [5.2 Escolha do /dev/mem](#52-escolha-do-devmem)
   - [5.3 Mapa de Endereços dos PIOs](#53-mapa-de-endereços-dos-pios)
   - [5.4 Formato das Instruções](#54-formato-das-instruções)
   - [5.5 Funções Expostas](#55-funções-expostas)
   - [5.6 Fluxo de Execução](#56-fluxo-de-execução)
   - [5.7 Montagem da Instrução de 32 bits](#57-montagem-da-instrução-de-32-bits)
   - [5.8 Protocolo Enable e Polling](#58-protocolo-enable-e-polling)
6. [Modo de Uso](#6-modo-de-uso)
7. [Testes e Resultados](#7-testes-e-resultados)
8. [Erros e Limitações](#8-erros-e-limitações)
9. [Próximos Passos — Marco 3](#9-próximos-passos--marco-3)
10. [Referências](#10-referências)

---

## 1. Introdução e Definição do Problema

## 2. Requisitos Principais

## 3. Fundamentação Teórica

### 3.1 DE1-SoC e a Lightweight HPS-to-FPGA Bridge

### 3.2 MMIO (Memory-Mapped I/O)

### 3.3 /dev/mem e Syscalls

### 3.4 Polling

---

## 4. Co-processador ELM

### 4.1 Descrição

### 4.2 Barramentos

### 4.3 ISA — Conjunto de Instruções

---

## 5. Descrição da Solução

### 5.1 Arquitetura Geral

### 5.2 Escolha do /dev/mem

### 5.3 Mapa de Endereços dos PIOs

### 5.4 Formato das Instruções

### 5.5 Funções Expostas

### 5.6 Fluxo de Execução

### 5.7 Montagem da Instrução de 32 bits

### 5.8 Protocolo Enable e Polling

---

## 6. Modo de Uso

---

## 7. Testes e Resultados

---

## 8. Erros e Limitações

---

## 9. Próximos Passos — Marco 3

---

## 10. Referências