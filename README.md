# Cidade Alerta - Rede de Emergência Urbana

Sistema distribuído desenvolvido para a disciplina de Sistemas Distribuídos da Universidade Federal de Uberlândia (UFU).

O projeto simula uma rede de resposta a emergências urbanas, onde diferentes entidades distribuídas cooperam para detectar, validar e atender incidentes como incêndios, acidentes, emergências médicas e ocorrências policiais.

## Sobre o Projeto

A aplicação foi construída como um micromundo distribuído composto por múltiplos processos independentes que se comunicam através de sockets TCP/UDP e mensageria assíncrona utilizando Apache ActiveMQ.

O fluxo do sistema ocorre da seguinte forma:

1. Sensores detectam incidentes.
2. O Centro de Monitoramento valida os eventos.
3. A Central de Coordenação analisa cada ocorrência.
4. As unidades responsáveis são despachadas.
5. Os eventos são registrados em um banco de ocorrências compartilhado.

O objetivo principal foi aplicar, na prática, conceitos fundamentais de Sistemas Distribuídos em um cenário realista de coordenação entre processos.

---

## Arquitetura do Sistema

```text
Sensor de Incidentes
          │
          ▼
Centro de Monitoramento
          │
          ▼
Central de Coordenação
          │
 ┌────────┼────────┬─────────┐
 ▼        ▼        ▼         ▼
Ambulância Bombeiros Viatura Apoio
```

Além disso, todos os componentes podem utilizar o Servidor de Nomes para descoberta dinâmica de serviços.

---

## Tecnologias Utilizadas

* Python 3
* Socket TCP
* Socket UDP
* Threads
* JSON
* Apache ActiveMQ Classic
* STOMP Protocol
* Sistemas Distribuídos

---

## Conceitos de Sistemas Distribuídos Implementados

### Processos Distribuídos

Cada entidade do sistema executa como um processo independente:

* Servidor de Nomes
* Central de Coordenação
* Centro de Monitoramento
* Sensor de Incidentes
* Ambulância
* Bombeiros
* Viatura Policial
* Unidade de Apoio

### Comunicação por Sockets

* TCP para comunicação confiável entre processos.
* UDP para descoberta de serviços através do Servidor de Nomes.

### Arquitetura Baseada em Mensagens

As mensagens são trocadas em formato JSON contendo:

* Tipo da mensagem
* Origem
* Conteúdo
* Relógio lógico
* Informações de sincronização

### Relógios Lógicos

Implementação de:

* Relógios de Lamport
* Relógios Vetoriais

Utilizados para ordenação causal dos eventos distribuídos.

### Exclusão Mútua Distribuída

Controle de acesso ao Banco de Ocorrências através de:

* REQUEST_SC
* GRANT_SC
* RELEASE_SC

Garantindo acesso exclusivo ao recurso compartilhado.

### Servidor de Nomes

Mecanismo de descoberta dinâmica de serviços utilizando comunicação UDP.

---

## Integração com Apache ActiveMQ

A comunicação assíncrona utiliza filas específicas para cada componente.

### Filas utilizadas

| Fila                  | Finalidade                |
| --------------------- | ------------------------- |
| incidentes.detectados | Publicação de incidentes  |
| incidentes.validados  | Incidentes analisados     |
| despacho.ambulancia   | Chamados médicos          |
| despacho.bombeiros    | Ocorrências de incêndio   |
| despacho.viatura      | Ocorrências policiais     |
| despacho.apoio        | Apoio logístico           |
| status.unidades       | Atualizações operacionais |

---

## Como Executar

### Pré-requisitos

* Python 3.x
* Java 11+ ou 17+
* Apache ActiveMQ Classic
* Biblioteca stomp.py

### Instalação da Dependência

```bash
pip install stomp.py
```

### Iniciar o ActiveMQ

```bash
activemq.bat start
```

Painel administrativo:

```text
http://localhost:8161

Usuário: admin
Senha: admin
```

---

## Ordem de Inicialização

Executar cada componente em um terminal separado:

```text
1. Servidor de Nomes
2. Central de Coordenação
3. Centro de Monitoramento
4. Unidade Ambulância
5. Unidade Apoio
6. Unidade Bombeiros
7. Unidade Viatura
8. Sensor de Incidentes
```

---

## Exemplo de Execução

1. O Sensor detecta um incêndio.
2. O Centro de Monitoramento valida a ocorrência.
3. A Central registra o incidente.
4. Bombeiros, Ambulância e demais unidades são acionadas.
5. As unidades solicitam acesso ao Banco de Ocorrências.
6. Os eventos são registrados utilizando exclusão mútua distribuída.
7. O atendimento é concluído.

---

## Resultados

O projeto permitiu aplicar na prática:

* Comunicação distribuída
* Coordenação entre processos
* Mensageria assíncrona
* Descoberta de serviços
* Exclusão mútua
* Ordenação lógica de eventos
* Integração com middleware de mensagens

Simulando uma arquitetura próxima de sistemas distribuídos utilizados em ambientes reais.

---

## Autor

Pedro Henrique Vieira Pinto

Bacharelado em Sistemas de Informação
Universidade Federal de Uberlândia (UFU)
