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
6. [Modo de Uso](#6-modo-de-uso)
7. [Testes e Resultados](#7-testes-e-resultados)
8. [Erros e Limitações](#8-erros-e-limitações)
9. [Próximos Passos — Marco 3](#9-próximos-passos--marco-3)
10. [Referências](#10-referências)

---

## Introdução e Definição do Problema
Este projeto tem como objetivo realizar a integração entre o co-processador ELM implementado na FPGA e o sistema Linux executando no HPS da placa DE1-SoC. O co-processador, desenvolvido em Verilog no Marco 1, é responsável por executar a inferência do modelo ELM diretamente em hardware.

No Marco 2, o foco principal é permitir que o processador ARM consiga se comunicar corretamente com o co-processador através de MMIO (Memory-Mapped I/O), utilizando as bridges entre HPS e FPGA disponíveis na placa. Para isso, foi utilizada a ferramenta Platform Designer no Quartus Prime para integrar o hardware ao sistema do HPS.

Além da parte de hardware, também foi desenvolvido um driver Linux com partes em Assembly ARM, responsável por fazer o controle e acesso aos registradores do co-processador.

O principal desafio desta etapa é garantir que a comunicação entre o Linux e o co-processador funcione de forma correta e estável, permitindo o envio e leitura de dados sem erros de sincronização. Para isso, foi necessário mapear os registradores do módulo, configurar a comunicação entre HPS e FPGA e implementar as rotinas de acesso ao hardware.

Ao final deste marco, o sistema deve estar apto para que, no Marco 3, uma aplicação em linguagem C consiga utilizar o co-processador através do driver desenvolvido.
## Requisitos Principais

### Integração HPS↔FPGA
Fazer a integração entre o HPS e o co-processador ELM na FPGA utilizando o Platform Designer, permitindo que o processador ARM consiga se comunicar com o hardware implementado em Verilog.

### Driver Linux em Assembly ARM
Desenvolver um driver Linux com rotinas críticas em Assembly ARM para realizar o controle do co-processador e o acesso aos registradores do hardware.

### Comunicação via MMIO
Implementar a comunicação utilizando MMIO (Memory-Mapped I/O), permitindo que o Linux consiga ler e escrever dados nos registradores do co-processador através de endereços de memória.

### Controle do Co-processador
Implementar as rotinas de controle do co-processador — incluindo início de inferência, monitoramento via flags Done/Busy/Error e leitura do resultado — através de uma API definida com funções como open, write, read e ioctl.

### Leitura e Envio de Dados
Garantir o envio correto dos dados de entrada para o co-processador e a leitura dos resultados retornados após a inferência.

### Demonstração de Estabilidade
Realizar testes para verificar se a comunicação entre HPS e FPGA está funcionando corretamente e de forma estável durante várias execuções.

## Fundamentação Teórica

### DE1-SoC e a Lightweight HPS-to-FPGA Bridge
A DE1-SoC é a placa utilizada no projeto. Ela junta duas partes principais: 
o HPS, que é o processador ARM responsável por rodar o Linux, e a FPGA, 
onde o co-processador ELM foi implementado em Verilog.

Como essas duas partes precisam trocar informações, a placa possui bridges 
que fazem essa comunicação. No projeto foi utilizada a Lightweight Bridge, 
que permite que o processador ARM consiga acessar os registradores do 
hardware na FPGA de forma mais simples e direta.

A placa conta com as seguintes especificações relevantes para o projeto:

- Processador ARM Cortex-A9 dual-core (HPS)
- FPGA Cyclone V
- 1GB de RAM DDR3
- Sistema operacional Linux embarcado rodando no HPS
- Lightweight Bridge com endereço base `0xFF200000`

### Platform Designer
O Platform Designer é uma ferramenta do Quartus que facilita a conexão 
entre o HPS e a FPGA de forma visual, sem precisar fazer toda a integração 
manualmente. Foi através dele que a comunicação entre o processador ARM e 
o co-processador ELM foi organizada no projeto.

Nele, foram adicionados o HPS e os PIOs usados para trocar informações 
com o hardware:

- um PIO de 32 bits para enviar dados (`Data In`);
- um PIO de 3 bits para os sinais de controle (`Signals`);
- um PIO de 32 bits para receber os resultados (`Data Out`). # botar imagens nos 3 

Depois disso, tudo foi conectado utilizando a Lightweight Bridge, que é 
a ponte responsável por permitir que o Linux consiga acessar os 
registradores da FPGA.

O Platform Designer também gera automaticamente os endereços de memória 
usados na comunicação. Foi ele que definiu:

- `Data In` → `0xFF200000`
- `Signals` → `0xFF200010`
- `Data Out` → `0xFF200020` #botar imangens nos 3 

Esses endereços são fundamentais porque é através deles que o driver 
consegue escrever e ler informações do co-processador. Sem isso, o Linux 
não teria como acessar o hardware implementado na FPGA.

### MMIO (Memory-Mapped I/O)
MMIO (Memory-Mapped I/O) é uma forma de comunicação onde os registradores 
do hardware funcionam como posições de memória. Na prática, isso significa 
que o Linux consegue controlar o co-processador apenas lendo e escrevendo 
em determinados endereços de memória. Dessa forma, é possível enviar dados 
para a FPGA, iniciar a inferência e depois ler o resultado retornado pelo 
hardware.

### /dev/mem e Syscalls
O `/dev/mem` é um recurso do Linux que permite acessar diretamente regiões 
da memória física do sistema. No projeto, ele foi utilizado para acessar 
os registradores do co-processador conectados pela Lightweight Bridge.

Para fazer esse acesso, o programa utiliza syscalls, que são chamadas do 
sistema operacional. Funções permitem abrir o `/dev/mem`, mapear os 
endereços da FPGA na memória do programa e depois liberar os recursos 
utilizados.

A syscall mais importante nesse processo é o `mmap()`, porque é ela que 
faz o mapeamento do endereço físico da FPGA, como o `0xFF200000`, para o 
espaço de memória do processo. Na prática, isso permite que o código em 
Assembly consiga acessar os PIOs diretamente utilizando ponteiros, como 
se estivesse acessando variáveis normais da memória.

### Polling
---

## 4. Co-processador ELM

### 4.1 Descrição

### 4.2 Barramentos

### 4.3 ISA — Conjunto de Instruções

---

## 5. Descrição da Solução



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
