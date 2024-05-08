# Data Warehouse - Análise de Aberturas de Xadrez

*Aluno: [Seu Nome]
*Orientador: [Nome do Orientador]

## 1. Introdução

Com o crescimento da quantidade de partidas de xadrez registradas digitalmente e disponíveis para análise, torna-se crucial organizar esses dados de forma eficiente para gerar insights valiosos sobre estratégias de abertura. Um Data Warehouse (DW) dedicado a essa análise pode fornecer formas de consulta eficientes e detalhadas.

Este projeto tem como objetivo a implementação de um Data Warehouse para análise das aberturas de xadrez, permitindo compreender a eficácia, popularidade e tendências dessas aberturas ao longo do tempo. 

### Objetivos Específicos:
- Extrair dados de partidas de xadrez e organizar as informações em um DW.
- Desenvolver consultas SQL que respondam perguntas-chave sobre a eficácia das aberturas.
- Criar gráficos e dashboards para análise visual dos dados.

## 2. Business Intelligence e Data Warehouse

### 2.1 Business Intelligence
O conceito de Business Intelligence (BI) surgiu para auxiliar na gestão de negócios por meio de tecnologias que otimizam a obtenção e análise de dados. O BI visa coletar e armazenar dados de diferentes fontes, transformando-os em informações concretas para gerar estratégias mais embasadas e tomadas de decisão mais eficientes.

### 2.2 Data Warehouse
Um Data Warehouse (DW) é um armazém de dados que integra um grande volume de dados sobre um assunto específico, normalmente espalhados de forma desorganizada.

Características básicas de um DW incluem:
- **Baseado em assuntos**: Integra dados importantes para a organização.
- **Integrado**: Remove inconsistências dos dados.
- **Não-volátil**: Dados não são atualizados diretamente no DW.
- **Variável em relação ao tempo**: Permite análises temporais detalhadas.

### 2.3 Modelagem Dimensional
A modelagem dimensional é uma arquitetura de projeto lógico, muito utilizada em DWs, para melhorar o desempenho de consultas. Ela ocorre de forma diferente do modelo relacional, focando em resultados rápidos para auxiliar na tomada de decisões.

#### Comparação entre Modelos Dimensional e Relacional
| Modelo Dimensional | Modelo Relacional |
|---------------------|-------------------|
| Mais fácil e intuitivo | Mais complexo |
| Tabelas Fato e Dimensão | Tabelas de Dados e Relacionamentos |
| Tabelas Fato são o núcleo | Todas as tabelas são normalizadas |

#### Tabela Fato
A tabela fato é o centro do modelo dimensional, integrando os dados das tabelas de dimensão.

#### Tabela Dimensão
A tabela dimensão representa uma parte do fato ou assunto que está sendo analisado.

#### Modelos Estrela e Floco de Neve
- **Modelo Estrela**: Tabelas de dimensão conectadas diretamente à tabela fato.
- **Modelo Floco de Neve**: Normaliza tabelas de dimensão, aumentando a complexidade.

## 3. Metodologia

O desenvolvimento do Data Warehouse foi dividido nas seguintes etapas:
- **Pesquisar fontes de dados**: Coleta de dados de partidas de xadrez em um arquivo CSV.
- **Modelagem das tabelas**: Criação das tabelas de dimensão e fato.
- **Inserção de dados nas tabelas**: População das tabelas com os registros coletados.
- **Análise dos dados**: Criação de gráficos e dashboards para análise visual dos dados.

## 4. Desenvolvimento

### 4.1 Fontes de Dados
Os dados utilizados para compor o Data Warehouse são oriundos de um arquivo CSV contendo detalhes de partidas de xadrez.

### 4.2 Modelagem das Tabelas
Foram modeladas as seguintes tabelas:

#### Tabela `DimJogador`
```sql
CREATE TABLE DimJogador (
    jogador_id VARCHAR(255) PRIMARY KEY,
    rating INT
);
