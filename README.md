# Data Warehouse - Análise de Aberturas de Xadrez

*Aluno: Wendel Adriano Bitencourt


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

#### Tabela Fato
A tabela fato é o centro do modelo dimensional, integrando os dados das tabelas de dimensão.
![Tabela Fato](https://raw.githubusercontent.com/WendelBitencourt/DW-Analize-Aberturas-Xadrez/main/graficos/fato.png)

#### Tabelas Dimensão
A tabela dimensão representa uma parte do fato ou assunto que está sendo analisado.
![Tabela Dimensão](https://raw.githubusercontent.com/WendelBitencourt/DW-Analize-Aberturas-Xadrez/main/graficos/dimensoes.png)

#### Modelo Estrela
- **Modelo Estrela**: Tabelas de dimensão conectadas diretamente à tabela fato.
![Modelo estrela](https://raw.githubusercontent.com/WendelBitencourt/DW-Analize-Aberturas-Xadrez/main/graficos/Esquema%20Estrela.png)

## 3. Metodologia

O desenvolvimento do Data Warehouse foi dividido nas seguintes etapas:
- **Pesquisar fontes de dados**: Coleta de dados de partidas de xadrez em um arquivo CSV.
- **Modelagem das tabelas**: Criação das tabelas de dimensão e fato.
- **Inserção de dados nas tabelas**: População das tabelas com os registros coletados.
- **Análise dos dados**: Criação de gráficos e dashboards para análise visual dos dados.

## 4. Desenvolvimento

### 4.1 Fontes de Dados
Os dados utilizados para compor o Data Warehouse são oriundos de um arquivo CSV do site [https://www.kaggle.com/datasets/datasnaek/chess] contendo detalhes de partidas de xadrez.

### 4. 2 Desenvolvimento


### 4.1 Escolha do Tema e Problemática
O tema do projeto é "Análise de Aberturas de Xadrez". A problemática abordada foi identificar as aberturas mais eficazes e populares entre jogadores de diferentes níveis de habilidade, analisando tendências e a eficácia de diferentes estratégias de abertura. As perguntas foram baseadas em fatores como a eficácia das aberturas, popularidade ao longo do tempo e preferência entre jogadores altamente classificados.

### 4.2 Extração do Arquivo CSV para o Banco "ChessGames"
Os dados das partidas foram extraídos do arquivo CSV disponível no Kaggle (https://www.kaggle.com/datasets/datasnaek/chess) e carregados na tabela `games` do banco `ChessGames` para pré-processamento. 

#### Tabela `games` (Banco `ChessGames`)
```sql
CREATE TABLE games (
    id VARCHAR(255) PRIMARY KEY,
    rated ENUM('TRUE', 'FALSE'),
    created_at BIGINT,
    last_move_at BIGINT,
    turns INT,
    victory_status ENUM('mate', 'resign', 'stalemate', 'timeout'),
    winner ENUM('branco', 'preto', 'empate'),
    increment_code VARCHAR(255),
    white_id VARCHAR(255),
    white_rating INT,
    black_id VARCHAR(255),
    black_rating INT,
    opening_eco VARCHAR(10),
    opening_name VARCHAR(255),
    opening_ply INT
);
```

### 4.3 Criação do Banco "ChessDW" e Perguntas Respondidas

As tabelas de dimensão e fato foram criadas no banco ChessDW para estruturar a análise dos dados. O esquema estrela foi utilizado para organizar as informações de forma eficiente.



#### Tabelas do Banco "ChessDW"
#####Tabela DimJogador
```sql
CREATE TABLE DimJogador (
    jogador_id VARCHAR(255) PRIMARY KEY,
    rating INT
);
```
#####Tabela DimPartida
```sql
CREATE TABLE DimPartida (
    partida_id INT PRIMARY KEY,
    num_turnos INT,
    rated ENUM('TRUE', 'FALSE')
);


```
#####Tabela DimTempo
```sql
CREATE TABLE DimTempo (
    tempo_id INT PRIMARY KEY,
    data_partida DATE,
    ano INT,
    mes INT,
    dia INT,
    hora INT
);

```
#####Tabela DimAbertura
```sql
CREATE TABLE DimAbertura (
    abertura_id VARCHAR(10) PRIMARY KEY,
    nome_abertura VARCHAR(255)
);

```
#####Tabela FatoPartidas
```sql
CREATE TABLE FatoPartidas (
    fato_id INT PRIMARY KEY,
    partida_id INT,
    jogador_branco_id VARCHAR(255),
    jogador_preto_id VARCHAR(255),
    tempo_id INT,
    abertura_id VARCHAR(10),
    vencedor ENUM('branco', 'preto', 'empate'),
    FOREIGN KEY (partida_id) REFERENCES DimPartida(partida_id),
    FOREIGN KEY (jogador_branco_id) REFERENCES DimJogador(jogador_id),
    FOREIGN KEY (jogador_preto_id) REFERENCES DimJogador(jogador_id),
    FOREIGN KEY (tempo_id) REFERENCES DimTempo(tempo_id),
    FOREIGN KEY (abertura_id) REFERENCES DimAbertura(abertura_id)
);

```


### 4.4 Processo de Elaboração do DW
O processo de elaboração do Data Warehouse envolveu as seguintes etapas:

- Modelagem do Esquema:
    - Definição das tabelas de dimensão e fato para criar um esquema do tipo estrela.
    - Criação de tabelas de dimensão para jogadores, partidas, tempo e aberturas.
- Construção das Tabelas:
    - Criação das tabelas DimJogador, DimPartida, DimTempo, DimAbertura e FatoPartidas utilizando SQL.
- Carregamento dos Dados:
    - Importação dos dados das partidas a partir do arquivo CSV.
    - Inserção dos dados transformados nas tabelas de dimensão e fato.
- Análise dos Dados:
    - Desenvolvimento de consultas SQL para responder perguntas-chave sobre as aberturas de xadrez.
    - Criação de gráficos e dashboards para visualização dos resultados.




