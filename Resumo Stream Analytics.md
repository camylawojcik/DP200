#### Stream Analytics
  Process streaming data and respond to data anomalies in real time.
  Integra seus aplicativos e dispositivos IoT a um mecanismo de análise por streaming para obter insights com os dados de streaming em tempo real.
  - Detecta problemas ou reconhece e os responde;
  - Reconhece o comportamento sob diversas condições para impulsionar ainda mais aprimoramentos de tal componente ou sistema;
  - Disparar ações especifícas
  - A piece in the puzzle: não é uma ferramenta que faz tudo.
  - When Use: 
    Respond to data in real time or analyze large batches of data in a continous time-bound stream.
  - Data Ingestion:
    By configuring data inputs from first-class integration sources, these sources include:
    - __Event Hubs, Azure IOT Hub and Azure Blob__
    - IoT Hub: enrich relationship between your devices and your back-end systems.
    - Event-hubs: Provides big-data streaming services. Designed for high data throughput, allowing customers to send billions of requests per day. Uses a partitioned consumer model to scale out your data stream. Authentication through a shared key.
  - __Data Processing__
  Set up jobs with input and output pipelines.
    - Inputs are provided by Events Hubs, IoT Hubs or Azure Storage. The output can be route to many storage systems. Include Blob, SQL Database, Data Lake Storage, and cosmos DB.
    - After Stored, batch analytics in HdInsight, or send the output to a service like Event Hubs for consumption. Or PBI Streaming API.
  - __Queries__: Stream Analyticss Query Language, consistent with SQL.
  - __Data Security__:
    - At the transport layer between device and IoT Hub.
    - Event Hubs uses a shared key to secure the data transfer.
