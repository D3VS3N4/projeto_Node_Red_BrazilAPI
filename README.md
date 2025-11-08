# 🔍 Projeto Node-RED: Buscador de CEP e Lista de Corretoras (MQTT)

## Introdução

Este projeto implementa uma solução de back-end completa usando o ambiente **Node-RED** que integra **duas funcionalidades principais** baseadas na **BrasilAPI**:

1.  Um **Buscador de CEP** que recebe a requisição HTTP e retorna o endereço formatado.
2.  Uma funcionalidade de **Lista de Corretoras de Imóveis** que consulta e apresenta empresas cadastradas.

O diferencial deste projeto é a integração do protocolo **MQTT (Message Queuing Telemetry Transport)** para fornecer um **monitoramento de eventos em tempo real**. A cada busca de CEP realizada, um evento de notificação é disparado, demonstrando a capacidade de arquitetura de sistema assíncrona.

***

## 🛠️ Ferramentas Utilizadas

| Ferramenta | Tipo | Utilidade no Projeto |
| :--- | :--- | :--- |
| **Node-RED** | Plataforma Low-Code | Ambiente principal para construir e executar os fluxos (flows) lógicos. |
| **Node.js e NPM** | Runtime/Gerenciador | Base de execução do Node-RED. |
| **Git e GitHub** | Controle de Versão | Rastreamento de mudanças e hospedagem do código-fonte. |
| **API BrasilAPI** | Serviço Externo | Fonte de dados para **CEP** e **Corretoras de Imóveis**. |
| **Protocolo HTTP** | Comunicação Web | Recebe a requisição (`http in`) e envia a resposta HTML (`http response`). |
| **Protocolo MQTT** | Comunicação Assíncrona | Usado para o diferencial de monitoramento, via broker **`broker.hivemq.com`**. |

***

## ⚙️ Detalhes da Arquitetura do Fluxo

O fluxo é estruturado para lidar com as duas funcionalidades e gerenciar a resposta rápida ao usuário (HTTP) e o evento de sistema (MQTT) simultaneamente.

### 1. Funcionalidade Principal: Buscador de CEP e Monitoramento

Este fluxo do CEP é dividido em duas rotas paralelas dentro do nó **`Função MQTT`**:

| Componente | Função Principal |
| :--- | :--- |
| **Rota (`http in`)** | Define a URL de entrada para a busca de CEP (ex: `/cep/01001000`). |
| **Função MQTT** | Roteia a mensagem original (`msg`) para a Saída 1 (HTML) e a notificação (`msg_mqtt`) para a Saída 2 (MQTT). |
| **Fazer o visual do CEP** | Converte o JSON do CEP em uma página HTML amigável, configurando o status HTTP (**200** ou **404**). |
| **mqtt (out)** | Publica o evento JSON de sucesso no tópico **`d3v/projeto_brasilapi/cep_buscas`**. |

### 2. Funcionalidade: Lista de Corretoras de Imóveis

Este é um fluxo secundário que consulta e retorna a lista de empresas:

| Componente | Função Principal |
| :--- | :--- |
| **Rota (`http in`)** | Define a URL de entrada para acessar a lista (ex: `/corretoras`). |
| **http request** | Faz a requisição ao endpoint de corretoras da BrasilAPI. |
| **Processamento (Função/Template)** | Prepara o JSON da lista para ser exibido ao usuário. |
| **Envio da resposta** | Retorna a lista processada ao navegador. |

***

## ▶️ Como Executar o Programa (Passo a Passo Completo)

Siga estas instruções detalhadas para configurar o ambiente e colocar o projeto em funcionamento:

### 1. Instalação das Ferramentas Essenciais

1.  **Instalar o Node.js e NPM:** Baixe e instale a versão **LTS** do Node.js em `https://nodejs.org/`.
2.  **Instalar o Git:** Baixe e instale o Git em `https://git-scm.com/downloads`.
3.  **Instalar o Node-RED Globalmente:** Abra o Terminal (ou CMD/PowerShell) e execute:
    ```bash
    npm install -g node-red
    ```

### 2. Obter o Código do Projeto

1.  Abra o Terminal e navegue até a pasta onde deseja salvar o projeto (ex: `cd C:\Projetos`).
2.  Clone o repositório do GitHub:
    ```bash
    git clone https://github.com/D3VS3N4/projeto_Node_Red_BrazilAPI
    ```
3.  Entre no diretório do projeto:
    ```bash
    cd [Nome do diretório do projeto]
    ```

### 3. Iniciar o Servidor Node-RED e Importar o Fluxo

1.  **Inicie o servidor Node-RED** no Terminal (estando dentro da pasta do projeto):
    ```bash
    node-red
    ```
    O servidor estará ativo em `http://127.0.0.1:1880/`.

2.  Abra a URL no seu navegador.

3.  **Importar o Fluxo:**
    * No editor, clique no **Menu (três linhas)** > **Importar** > **Selecionar arquivo para importar**.
    * Navegue até a pasta do projeto e selecione o arquivo de fluxo JSON (ex: `fluxos/flows.json`).
    * Clique em **Deploy (Enviar)** para carregar a lógica.

### 4. Testar as Funcionalidades

#### Teste 1: Buscador de CEP e Monitoramento (Principal)

1.  Abra uma nova aba e teste a busca com um CEP válido:
    ```
    [http://localhost:1880/cep/SEUCEP]
    ```
2.  **Verifique a Página:** O endereço deve ser exibido em HTML.

#### Teste 2: Lista de Corretoras de Imóveis

1.  Acesse a rota de corretoras configurada no fluxo (ex: `/Corretoras`):
    ```
    [http://localhost:1880/Corretoras/]
    ```
