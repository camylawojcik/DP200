### Stream Analytics
  Process streaming data and respond to data anomalies in real time.
  Integra seus aplicativos e dispositivos IoT a um mecanismo de análise por streaming para obter insights com os dados de streaming em tempo real.
  - Detecta problemas ou reconhece e os responde;
  - Reconhece o comportamento sob diversas condições para impulsionar ainda mais aprimoramentos de tal componente ou sistema;
  - Disparar ações especifícas;
  - Existem 2 tipos de processamento:
    - Dados Streaming podem ser coletados em tempo real e persistido em um storage como um dado estático. O dado pode ser processado quando conveniente ou quando o custo for mais baixo;
    - Live data Streams tem uma necessidade de storage relativamente mais baixas. Requisitam mais processamento para executar calculos em janelas deslizantes sobre os dados continuamente recebidos para gerar insights;
  - A piece in the puzzle: não é uma ferramenta que faz tudo.
  - When Use: 
    Respond to data in real time or analyze large batches of data in a continous time-bound stream.
- __Process Events with Azure Stream Analytics__
  - __Event Producer__:ex: sensores ou processos que geram dados continuamente (monitor cardiaco);
  - __Event Ingest System:__ Obtém o dado do sistema fonte e passa por uma Analytics engine 
  - __Stream Analytics Engine__: o processamento é executado sob o fluxo de dados recebidos e as infos são extraídas. O stream Analytics expõem a linguagem de consulta SAQL, um conjunto do Transact-SQL personalizado para executar calculos sobre os dados de streaming;
    - __Event Process__: Engine que consome os dados do stream e entrega insights. Pode processar um evento por vez, ou vários (ex: pedágio)
  - __Event Consumer__: Aplicação que consome dados e toma ações específicas baseadas nos insights;  
- ### Data Ingestion:
    By configuring data inputs from first-class integration sources, these sources include:
    - __Event Hubs, Azure IOT Hub and Azure Blob__
    - IoT Hub: enrich relationship between your devices and your back-end systems.
    - Event-hubs: Provides big-data streaming services. Designed for high data throughput, allowing customers to send billions of requests per day. Uses a partitioned consumer model to scale out your data stream. Authentication through a shared key.
- ### Data Processing:
  Set up jobs with input and output pipelines.
    - Inputs are provided by Events Hubs, IoT Hubs or Azure Storage. The output can be route to many storage systems. Include Blob, SQL Database, Data Lake Storage, and cosmos DB.
    - After Stored, batch analytics in HdInsight, or send the output to a service like Event Hubs for consumption. Or PBI Streaming API.
  - Event Consumer: Destino da saída do stream analytics engine (Data Lake, Cosmos, SQL DB, Blob ou PBI)
- Garante o processamento e entrega de pelo menos um evento, para que eventos não sejam perdidos. Possui recursos de recuperação internos.
- Fornece pontos de verificação interno (built-in checkpoint) para produzir reusltados repetíveis;
- Permite in-memory compute, oferecendo performance superior. Contribuindo para um custo mais baixo.
- ### __Understand the Streaming Analytics__
  - A job is a unit of execution
  - Um pipeline consiste em 3 partes:
    - __Input:__ uma fonte
    - __Transformation query:__ ação sobre o input
    - __Output:__ identifica o destino da saída do dado transformado;
  - __Logs:__ Stream Analytics Log provides details about each job you rub.
    - __Dashboards:__ mostram métricas chaves para o Stream Analytics Jobs;
    - __Diagnostic Logs:__ parte chave da infra estrutura operacional. Ajuda a encontrar a causa raiz dos problemas de deploy em produção;
      - Desabilitado por default;
  
  - __Queries__: Stream Analyticss Query Language, consistent with SQL.
  - __Data Security__:
    - At the transport layer between device and IoT Hub.
    - Event Hubs uses a shared key to secure the data transfer.
- #### __Monitoring__
  - Jobs can monitored using.
    - Azure portal
    - PoserShell
    - .NET SDK
    - Visual Studio
  - Metrics Available for monitoring:
    - 4 Scenarios:
      - SU % utilization (stream units)
      - Runtime errors
      - Watermark delay (se uma base faltar, por ex)
      - Input Deserialization error (erro no código)
    - Setting up int the portal:
      - Alerts can be configured in the Portal on the job Page
      - Alerts are configured by job
      - Notifications are customizable
- ### Optimize
  - Stream Units:
    - Computing resources allocated to your job
    - Capacity is key
    - Review SU% metric
      - 80% should be the redline
    - Start with 6 SUs for queires not using PARTITION BY
  - Paralelization:
    - Partitioning is key for optimization
      - inputs are already partitioned
      - outputs need to partitioned
    - Events should go to the same partition of your input
    - PARTITION BY should always be used in all steps
    - Input partition must equal output partition
    
