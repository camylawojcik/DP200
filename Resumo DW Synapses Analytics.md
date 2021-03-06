## Azure Data Warehouse Synapses Analytics
- Escala Ilimitada;
- Query with __MPP__: Massive Parallel Process;
- Run Complex Queries;
- Ambiente Unificado combinando o data warehouse empresarial do SQL, às funcionalidades de análise de Big Data do Spark;
- SQL Analytics and SQL Pools: Modern Data Warehouse;
- Arquitetura que distribui o processamento em vários nodes;
- Relational Tables with columnar storage:
  - Menor custo de storage;
  - Mais performance;
- Analytics in massive scale;
- Comparado com o DWs Tradicionais, leva frações de segundo para finalizar consultas;
- T-SQL - Batch, streaming and interactive processing of data
- ### __SQL Pools using DW Units:__
  - CPU, Memory and IO are boundled into units of compute scale called SQL pool;
  - SQL Pool = Medida de recursos computacionais
    - O tamanho é determinado por DW Units (DWU);
    - + performace + DWU
    - Storage and compute costs são bilados separadamente
      - Mudar a quantidade de DWUs não afeta o custo de storage;
- __Vantagens:__
  - Ingestão de várias fontes;
  - Scale-out to distribute computational processing of data across a large cluster of nodes;
  - Poder computacional independente de storage;
  - Aumente ou diminua o poder computacional sem mover os dados;
  - Pause a capacidade computacional e pague apenas pelo armazenamento;
  - Continue com a capacidade computacional durante as horas operacionais;
  
- __Recursos:__
  - Gerenciamento de carga de trabalho;
    - Capacidade de priorizar cargas de trabalho de consulta que ocorrem no servidor, através de 3 áreas relacionadas:
      - Grupos de Carga de Trabalho:
        - Possibilita reservar recursos para determinados grupos
        - Limita a quantidade de recurso que pode ser consumida
        - Recursos compatilhados acessados com base no nível de importância;
        - Definir o tempo limite de uma consulta
        - Grupo criado usando o T-SQL;
      - Classificação da Carga de Trabalho:
        - Usando o T-SQL você pode criar um classificador para definir a importância de uma solicitação;
      - Importância da Carga de Trabalho:
        - Comando CREATE WORKLOAD CLASSIFIER
        - Consultas com prioridade mais alta recebem recursos antes;
        - A fila é FIFO e a medida que os recursos são liberados é respeitada a prioridade;
      - Cache do Conjunto de Resultados:
        - Os resultados das consultas são armezados no Pool SQL;
        - Tempo de resposta interativo para consultas repetidas;
        - O cache é removido com base no TLRU;
        - Você pode definir o cache no nível de banco ou de sessão;
      - Exibição Materializadas:
        - Pré computa os resultados e armazena;
        - Atualizado automaticamente quando os dados das tabelas subjacentes forem alterados (operação sincrona);
        - Essa funcionalidade de cache automático permite que o otimizador de consulta considere o uso de uma view indexada mesmo quando ela não é referenciada na consulta;
- Suporte a CI/CD;
- ColumnStore Ordenado;
- Mascara de dados dinâmicos;
- Segurança em nível de linha;
### __MPP__
  Possibilita escalar computing power sem interferir na storage
  - Isso é feito abstraindo CPU, Memória e i/o, agrupando-os em Units of Compute Scale: *SQL POOL*
  - Alterando o nivel de serviço, você altera o número de DWUs que estão alocados, o que ajusta a performance e o custo;
  - ### Node-based Architecture:
    - App conectam a um nó de controle e emitem comandos T-SQL para ele, que é o ponto único de entrada do DW;
    - O nó de controle executa o mecanismo do MPP e passa para os nós de computação executarem em paralelo
    - Os nós de pc. armazenam os dados de usuário no Azure Storage e executam as consultas paralelas.
    - O Data Movement Service (DMS) move os dados pelos nós conforme necessário para executar consultas em paralelo e retornar resultados precisos.
  - __Control Node:__ manage de parallel processing engine and optimizes the query by passing it through or distributing to multiples compute nodes. Após o processamento, o dado volta para o control node e é entregue ao usuário;
    - Os nodes trabalham em paralelo para acessar a storage e rodar a query
    - O MPP é executo nele para otimizar e coordenar as consultas paralelas;
  - __Computing Nodes:__  fornecem capacidade de computação.
    - Conforme vc paga para obter mais nós de computing, o SQL DW mapeia novamente as distribuições;
    - O número de intervalo de nós é de 1 a 60 e é determinado pelo nível de serviço para o DW;
  - __Data Movement Service__: Coordena a movimentação dos dados entre os nós de computação. 
    - Quando o DW executa a consulta, o trabalho é dividido em 60 consultas menores que são executadas em paralelo
    - Cada consulta é executada em uma distribuição
    - Uma distribuição é uma unidade básica de armazenamento e processamento para consultas paralelas.
    - Algumas consultas necessitam da movimentação de dados entre nós para que o resultado seja preciso. O DSM garante isso;
- __The Law of 60__
  - At lowest level, which is 100 dw units, there is one computer node and 60 different distributions that are broken out for that one node.
  - 1000 dw units 10 compute nodes 6 distributions per node 
  - 6000 compute units 60 compute nodes 1 distribution per node
- __Sharding patterns__: type of horizontal partitioning that splits large databases into smaller components to make it easy to manage. Is needed when the data set is top big to be stored in a single database;
_ __Table Geometries:__
  - Os dados são fragmentados em distribuições para otimizar o desempenho do sistema.
  - Tipos de Fragmentação:
    - __Hash:__
      - Indicado: Fatos e dimensões grandes;
      - Não indicado: Se a chave de distribuição não pode ser utilizada;
      - Uma função hash é determinada como coluna de distribuição;
    - __Round Robin:__ 
      - Indicado: Tabela de Staing/Temporária;
      - Quando não tem chave de ligação óbvia ou uma boa coluna candidata;
      - Não adequado: se a performance for lenta durante a movimentação dos dados;
      - Distribui os dados uniformemente entre os nós, mas sem qualquer otimização adicional;
      - Uma distribuição é escolhida de maneira aleatória e em seguida, buffers de linha são atribuídos a distribuições em sequência;
      - Carga rápida; Consulta mais lenta que hash;
    - __Replicate:__
      - Adequado: dimensões pequenas, star schema, com menos de 2gb após compactação;
      - Não indicado: para muitas transações, muitas alterações de DWU; tabelas com muitas colunas, mas com poucas usadas; Se você indexar uma tabela replicada;
      - Faz cache de uma cópia em cada nó;
      - Elimina a necessidade de transferir dados entre nós;    
    - __Replicate:__ fast query performance for small tables
  Comece com a distribuição Round Robin, mas almeje a Hash para aproveitar uma arquitetura paralela massiva.
  - Não distribua o formato varchar;
  - Use sys.dm_pwd_request_steps para analisar as movimentações por trás das consultas e examinar a melhor estratégia de distribuição;
  - Gen1 e Gen2 podem contar com 60 nós de computação atribuidos para processar dados quando o número máximo de DWu está selecionado. Gen2 tem 5x a capacidade de computação e 4x mais consultas simultaneas que o Gen1.
- __ETL/ELT:__
  - Polybase: allows for SQL DW to also be ETL
  - Databricks: também pode ser usado para transformação
- __Data Analysis Service:__ inserts the semantic layer between the end business user and the DW. Becomes an entrypoint that allows business users to create reports and dashboards without the need for complex joins or databases queries. Also adds a layers of data security, allowing only portions of the data to become exposed to the end user;

### __Optimize__
- Advisor Recommendations
- Use o Polybase para load e export (creat table as select);
- Hash Distribute large tables
  - Choose the correct distribution types
-Remember the rule of 60;
- Don't over partition
  - Maximize throughput by breaking gzip into 60+ files
- Shrink it down;
  - Minimize transction size;
  - Minimize column sizes;
  
### Monitoring in SQL Database and SQL Data Warehouse
  - Provide tools and methods to monitor usage
  - Add or remove resources (CPU, memory, I/O)
  - Troubleshoot potential issues
  - leverage recommendations to improve performance

  - Key metrics to be monitored
    - CPU usage
    - Wait statistics - why are queries waiting	 on resources?
    - I/O usage - I/O limits of underlying storage
    - Memory usage - memory available proportional to vCore number
    - Azure SQL Database includes tools and resources needed to monitor, troubleshoot, and fix potential performance issues
  - The tools:
    - Metrics chart - for DTU consumption and resources approaching max utilization
    - SQL Server Management Studio - Performance dashboard to monitor top resource-consuming queries (sql developer)
   -Query Performance Insight - identify queries that spend the most resources (Single and Elastic)
    - SQL Database Advisor - View recommendations for creating and dropping indexes, parameterizing queries, and fixing schema issues
    - Azure SQL Intelligence Insights - automatic monitoring of database performance

  

### __PolyBase__
  - Mais fácil e escalável;
  - Acessa dados externos armazenados no Blob, hadoop ou Data Lake, via T-SQL;
  - Extraia em TXT, carregue no Blob, Hadoop ou Data Lake;
  - Importe o dado no SQL DW staging tables
  - Transforme o dado;
  - Insira nas tabelas de Prod;
 - #### PolyBase Architecture 
  - Head Node: SQL Server instances que o PolyBase usa para fazer as consultas(Cada     PolyBase group tem apenas um Head Node)
    - Sql Server
    - Poly Base Engine
    - PolyBase DMS - Data Movement Service
  - Compute Node - pode conter vários, 
    - SQL Server instance
    - Logical group of sql server instances and PolyBase DMS
    - Permitem multiplos nodes que processam e movem os dados através do sistema
  - O melhor jeito de mover os dados para tabelas externas é conhecido com CTAS - CREATE TABEL AS SELECT

  - __Formatos:__
    - CSV, UTF-8 ou 16
    - Hadoop: rc files, ORC, Parquet
    - gzip and anppy compressed;
  - Storage Key para autenticar;
  - Statistics to improve query performance
